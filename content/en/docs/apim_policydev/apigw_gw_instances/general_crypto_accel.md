{
"title": "Cryptographic acceleration",
  "linkTitle": "Cryptographic acceleration",
  "weight": 10,
  "date": "2019-10-17",
  "description": "Configure API Gateway to use an OpenSSL engine for cryptographic acceleration."
}
The API Gateway uses OpenSSL to perform cryptographic operations, such as encryption and decryption, signature generation and validation. OpenSSL exposes an *Engine API*, which makes it possible to plug in alternative implementations of some or all of the cryptographic operations implemented by OpenSSL. When configured appropriately, OpenSSL calls the engine's implementation of these operations instead of its own.

For example, a particular engine might provide improved implementations of the asymmetric operations RSA and DSA. This engine can then be plugged into OpenSSL so that whenever OpenSSL needs to perform either an RSA or DSA operation, it calls out to the engine's implementation of these algorithms rather than call its own.

Typically, OpenSSL engines provide a hardware implementation of specific cryptographic operations. The hardware implementation usually offers improved performance over its software-based counterpart, which is known as *cryptographic acceleration*.

You can configure cryptographic acceleration at the instance level in the API Gateway. To configure the API Gateway instance to use an OpenSSL engine instead of the default OpenSSL implementation, perform the following steps:

1. In the Policy Studio tree, expand **Environment Configuration** > **Listeners**.
2. Right-click the API Gateway instance, and select **Cryptographic Acceleration > Add OpenSSL Engine**.

## General configuration

The OpenSSL Engine Configuration dialog displays the name of the engine, the algorithms that it implements, together with any initialization and cleanup commands required by the engine. Complete the following fields:

**Name**: Enter an appropriate name for the engine in this field.

**Provides**: Enter a comma-separated list of cryptographic operations to be performed by the engine instead of OpenSSL. The engine must implement the listed operations, otherwise the default OpenSSL operations are used. The following operations are available:

`RSA` : RSA (Rivest Shamir Adleman) asymmetric algorithm

`DSA` : DSA (Digital Signature Algorithm) asymmetric algorithm  

`RAND` : Random number generation

`DH` : Diffie-Hellman anonymous key exchange algorithm

`ALL` : Engine's implementation of all cryptographic algorithms

For example, to configure the API Gateway to use the engine's implementation of the RSA, DSA, and DH algorithms only, enter the following in the **Provides** field:

```
RSA, DSA, DH
```

**Commands**: The OpenSSL engine framework allows a number of control commands to be invoked at various stages in the loading and unloading of a specific engine library. These commands can be issued before or after the initialization of the engine, and also before or after the engine is uninitialized. Control commands are based on text name-value pairs.

Typical uses for control commands include specifying the path to a driver library, logging configuration information, a password to access protected devices, a configuration file required by the engine, and so on.

OpenSSL control commands can be added by clicking the **Add** button. Enter the name of the command in the **Name** field, and its value in the **Value** field. This command *must* be supported by the engine.

Use the **When** drop-down list to select when the command is to be run. The options available are as follows:

`preInit` : Command is run before the engine is initialized (before the call to `ENGINE_init()`)

`postInit` : Command is run after the engine is initialized (after the call to `ENGINE_init()`)

`preShutdown` : Command is run before the engine shuts down (before the call to `ENGINE_finish()`)

`postShutdown` : Command is run after the engine shuts down (after the call to `ENGINE_finish()`)

## Cryptographic acceleration conversation: request-response

Hardware Security Modules (HSM) protect the private keys they hold using a variety of mechanisms, including physical tokens, passphrases, and other methods. When use of the private key is required by some agent, it must authenticate itself with the HSM, and be authorized to access this data.

Whatever the mechanism protecting the keys on the HSM, this commonly requires some interaction with the agent. The most common form of interaction required is for the agent to present a passphrase. The intent is generally that this is carried out by a real person, rather than produced mechanically by the agent. Other forms of interaction may include prompting the operator to insert a specific card into a card reader.

However, the requirement for an operator to enter a passphrase renders automated startup of services using the HSM impossible. Although weaker from a security standpoint, the server can conduct an automated dialog with a HSM when it requires access to a private key, presenting specific responses to specific requests, including feeding passphrases to it. Of course, this is futile if the dialog calls for the insertion of a physical token in a device.

The dialog for different keys on the same device is often the same. For example, a number of keys on a Thales nShield Solo HSM may require the server to present an operator passphrase for a preinserted card in a card-reader. The specific dialogs are therefore associated with the cryptographic engine.

Each dialog consists of a set of expected request-generated response pairs. The expected request takes the form of a regular expression. When the cryptographic device prompts for input, the text of this prompt is compared against each expected request in the conversation, until a match is found. When matched, the corresponding generated response is delivered to the HSM.

In the simplest case, consider a HSM producing the following prompt:

```
Enter passphrase for operator card Operator1:
```

You can identify this, for example, with the following regular expression:

```
"passphrase.*Operator1"
```

In the configured conversation, you can make the expected response to this prompt the passphrase for the specific card, for example:

```
"tellNoOne"
```

The server is somewhat at the mercy of the HSM for how this dialog continues. If the HSM continues to prompt for requests, the server can only attempt to respond. You may set the maximum expected challenge setting on the conversation to indicate a maximum number of prompts to expect from the HSM, at which point the server does its best to terminate the conversation, almost certainly failing to load the affected key.