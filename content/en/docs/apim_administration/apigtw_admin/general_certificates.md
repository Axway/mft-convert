{
"title": "Manage certificates and keys",
  "linkTitle": "Manage certificates and keys",
  "weight": "60",
  "date": "2019-10-18",
  "description": "Import CA certificates, and import and create server certificates and private keys in the certificate store."
}
For API Gateway to trust X.509 certificates issued by a specific Certificate Authority (CA), you must import that CA's certificate into the API Gateway's trusted certificate store. For example, if API Gateway is to trust secure communications (SSL connections or XML Signature) from an external SAML Policy Decision Point (PDP), you must import the PDP certificate, or the issuing CA certificate into the API Gateway certificate store.

In addition to importing CA certificates, you can import and create server certificates and private keys in the certificate store. These can be stored locally or on an external Hardware Security Module (HSM). You can also import and create public-private key pairs. For example, these can be used with the Secure Shell (SSH) File Transfer Protocol (SFTP) or with Pretty Good Privacy (PGP).

## View certificates and keys

To view the certificates and keys stored in the certificate store, select **Environment Configuration > Certificates and Keys > Certificates** in the **Configuration Studio Policy Studio** tree. Certificates and keys are listed on the following tabs in the **Certificates** window:

* **Certificates with Keys**: Server certificates with associated private keys
* **Certificates**: Server certificates without any associated private keys
* **CA**: Certificate Authority certificates with associated public keys

You can search for a specific certificate or key by entering a search string in the text box at the top of each tab, which automatically filters the tree.

![Certificates tab](/Images/APIGateway/general_certs_tab.png)

### Certificate management options

The following options are available at the bottom right of the window:

* **Create/Import**: Click to create or import a new certificate and private key. For details, see [Configure an X.509 certificate](#configure-an-x-509-certificate).
* **Edit**: Select a certificate, and click to edit its existing settings.
* **View**: Select a certificate, and click to view more detailed information.
* **Remove**: Select a certificate, and click to remove the certificate from the certificate store.
* **Keystore**: Click this to export or import certificates to or from a Java keystore. For details, see [Manage certificates in Java keystores](#manage-certificates-in-java-keystores).

## Configure an X.509 certificate

To create a certificate and private key, click **Create/Import**. The **Configure Certificate and Private Key** dialog is displayed. This section explains how to use the **X.509 Certificate** tab on this dialog.

![Edit a certificate](/Images/APIGateway/general_certs_edit.png)

### Create a certificate

Configure the following settings to create a certificate:

* **Subject**: Click **Edit** to configure the *Distinguished Name* (DName) of the subject.
* **Alias Name**: This mandatory field enables you to specify a friendly name (or alias) for the certificate. Alternatively, you can click **Use Subject** to add the DName of the certificate in he text box instead of a certificate alias.
* **Public Key**: Click **Import** to import the subject's public key (usually from a PEM or DER-encoded file).
* **Version**: This read-only field displays the X.509 version of the certificate.
* **Issuer**: This read-only field displays the distinguished name of the CA that issued the certificate.
* **Choose Issuer Certificate**: Select to explicitly specify an issuer certificate for this certificate (for example, to avoid a potential clash or expiry issue with another certificate using the same intermediary certificate). You can then click the browse button on the right to select an issuer certificate. This setting is not selected by default.
* **Not valid before**: Select a date to define the start of the validity period of the certificate.
* **Not valid after**: Select a date to define the end of the validity period of the certificate.
* **Sign Certificate**: You must click this button to sign the certificate. The certificate can be self-signed, or signed by the private key belonging to a trusted CA whose key pair is stored in the certificate store.

### Import certificates

You can use the following buttons to import or export certificates into the certificate store:

* **Import Certificate**: Click to import a certificate (for example, from a `.pem` or `.der` file).
* **Export Certificate**: Click to export the certificate (for example, to a `.pem` or `.der` file).

## Configure a private key

Use the **Private Key**tab to configure details of the private key. By default, private keys are stored locally (for example, in the API Gateway certificate store). They can also be provided by an OpenSSL engine, or stored on a Hardware Security Module (HSM) if required.

API Gateway supports PKCS#11-compatible HSM devices.

![Edit a Private key](/Images/APIGateway/general_certs_edit_key_new.png)

### Private key stored locally

If the private key is stored in the API Gateway certificate store, select **Private key stored locally**. The following options are available for keys stored locally:

* **Private key stored locally**: This read-only field displays details of the private key.
* **Import Private Key**: Click to import the subject's private key (usually from a PEM or DER-encoded file).
* **Export Private Key**: Click to export the subject's private key to a PEM or DER-encoded file.

### Private key provided by OpenSSL engine

{{< alert title="Warning" color="warning" >}}Engines from OpenSSL 1.0 are not compatible with OpenSSL 1.1. When configuring and using an OpenSSL engine, make sure it has been made for version 1.1.{{< /alert >}}

If the private key that corresponds to the public key in the certificate is provided by an OpenSSL engine, select **Private key provided by OpenSSL Engine**.

Configure the following fields to associate a key provided by the OpenSSL engine with the current certificate:

* **Engine name**: Enter the name of the OpenSSL engine to use to interface to an HSM. All vendor implementations of the OpenSSL Engine API are identified by a unique name. See your vendor's penSSL engine implementation or HSM documentation to find out the name of the engine.
* **Key Id**: Enter the key ID used to uniquely identify a specific private key from all others stored on an HSM. When you complete this dialog, the private key is associated with the certificate that you are currently editing. Private keys are identified by their key ID by default.
* **Conversation**: If the HSM requires the server to provide a specific response to a specific request from the HSM, you can enter the response in this field. This enables the server to conduct an automated dialog with a HSM when it requires access to a private key. For example, in a simple case, the server response might be a specific passphrase.

### Private key stored on external HSM

If the private key that corresponds to the public key stored in the certificate resides on an external HSM, select **Private key stored on Hardware Security Module (HSM)**, and enter the name of the **Certificate Realm**.

To use the API Gateway's PKCS#11 engine to access objects in an external HSM, the corresponding HSM provider and certificate realms must also be configured. For more details, see [Configure HSMs and certificate realms](#configure-hsms-and-certificate-realms).

## Configure HSMs and certificate realms

{{< alert title="Warning" color="warning" >}}OpenSSL 1.1 no longer supports FIPS and requires use of X.509 cryptographic operations. HSM hardware, and its corresponding driver, must be compatible with those requirements.{{< /alert >}}

*Certificate realms* are abstractions of private keys and public key certificates, which mean that policy developers do not need to enter HSM-specific configuration such as slots and key labels. Instead, if a private key exists on an HSM, the developer can configure the certificate to show that its private key uses a specific certificate realm, which is simply an alias for a private key (for example, `JMS Keys`).

For example, on the host machine, an administrator could configure the `JMS Keys` certificate realm, and create a *keystore*for the realm, which requires specific knowledge about the HSM (for example, PIN, slot, and private key label). The certificate realm is the alias name, while the keystore is the actual private keystore for the realm.

**Manage HSMs with keystoreadmin**:

The `keystoreadmin` script enables you to perform the following tasks:

* Register an HSM provider
* List registered HSM providers
* Create a certificate realm
* List certificate realms

For example, if a policy developer is using JMS, and wants to indicate that private keys exist on an HSM, they could indicate that the certificate is using the `JMS Keys` certificate realm. On each instance using the configuration, it is the responsibility of the administrator to create the `JMS Keys` certificate realm.

For more details, enter `keystoreadmin` in the following directory, and perform the instructions at the command prompt:

```
INSTALL_DIR/apigateway/posix/bin
```

When you enter `keystoreadmin` without arguments, this displays an interactive menu with the following options:

`1` - `Change group or instance`

When registering HSMs or configuring certificate realms, you must choose the local group and instance to configure.

`2` - `List registered HSM providers`

Display the HSMs that are currently registered.

`3` - `Register an HSM provider`

Before creating certificate realms, you must first register the HSM. This option guides you through the steps. The HSM must be installed, configured, and active, and you must know the full path to the HSM device driver (PKCS#11). You give the HSM an alias (for example, LunaSA), which you use later when registering certificate realms.

`4` - `List Certificate Realms`

List configured certificate realms and associated keystores.

`5` - `Create a Certificate Realm`

Creates a keystore and assign it to a certificate realm.

### Register an HSM provider

You must first register an HSM provider as follows:

1. Open a command prompt in the API Gateway `bin` directory (for example, `INSTALL_DIR/apigateway/posix/bin`).
2. Enter the `keystoreadmin` command.
3. Select option `3) Register an HSM provider`.
4. If prompted, select the appropriate API Gateway group or instance.
5. You are prompted for a provider alias name. The alias is local only. For example, if registering a LunaSA HSM, you might enter the `LunaSA` alias.
6. For convenience, `keystoreadmin` searches for supported HSM drivers. If found, it shows the list of supported drivers. If none are found, this does not mean the driver does not exist. You must see your HSM documentation for the location of the drivers. For example:
7. Choose from one of the following:1) /LunaSA/cryptoki.dll o) Other q) Quit
8. If successful, `keystoreadmin` loads the driver and displays its details. For example:

```
    Registering HSM provider...
    Initializing HSM...
    Crypto Version:2.20
    Manufacter Id:SafeNet, Inc.
    Library Description:Chrystoki
    Library Version:5.1 Device registered.
```

### Create a certificate realm and associated keystore

To create a certificate realm and associated keystore, perform the following steps:

1. Open a command prompt in the API Gateway `bin` directory (for example, `INSTALL_DIR/apigateway/posix/bin`).
2. Enter the `keystoreadmin` command.
3. Select option `5) Create a Certificate Realm`.
4. You are prompted to enter a certificate realm name. This certificate realm name is used in Configuration Studio Policy Studio when configuring the private key of the corresponding X.509 certificate. The realm name is case sensitive (for example, `JMS Keys`).
5. The registered HSMs are listed. For example, select option `1) HSM`.
6. The command connects to the selected HSM, and a list of available slots is displayed. Select the slot containing the private key to use for the certificate realm (for example, select slot `1`).
7. You are prompted to input the PIN passphrase for the slot. The passphrase will not echo any output.
8. When you enter the correct PIN passphrase for the slot, this displays a list of private keys. Choose the key to use for the certificate realm. For example:

   ```
   Choose from one of the following:
       1) server1_priv
       2) jms_priv
       q) Quit

   Select option:2
   ```
9. You are prompted for a file name for the keystore. For example:

   ```
   Certificate realm filename [jms keys.ks]:Successfully created the certificate realm:JMS KeysPress any key to continue...
   ```
10. The keystore is output to the API Gateway instance directory. For example:

```
apigateway/groups/group-2/instance-1/conf/certrealms/jms keys.ks
```

Each gateway instance must have its certificate realm configured. When finished creating certificate realms, you must restart the gateway instance for the changes to take effect.

### Start API Gateway when using an HSM

When the gateway is configured to use certificate realms, these realms are initialized on startup, and a connection to the corresponding HSM is established. This requires the PIN passphrase for the specific HSM slots. At startup, you can manually enter the required HSM slot PIN passphrase, or you can automate this instead.

#### Start API Gateway with manually entered PIN passphrase

When the gateway is configured to use an HSM, API Gateway stops all processing, prompts for the HSM slot PIN passphrase, and waits indefinitely for input. For example:

```
INFO    07/Jan/2015:16:31:54 Initializing certificate realm 'JMS Keys'...
Enter passphrase for Certificate Realm, "JMS Keys":
```

The gateway does not reprompt if the PIN passphrase is incorrect. It logs the error and continues, while any services that use the certificate realm cannot use the HSM.

#### Start API Gateway with automatic PIN passphrase

You can configure the gateway to start and initialize the HSM by invoking a command script on the operating system to obtain the HSM slot PIN passphrase. This enables the gateway for automatic startup without manually entering the PIN passphrase.

To configure an automatic PIN passphrase, perform the following steps:

1. Edit the vpkcs11.xml instance configuration file of API Gateway. For example:

   ```
   apigateway/groups/group-2/instance-1/conf/vpkcs11.xml
   ```
2. Add a `PASSPHRASE_EXEC` command that contains the full path to the script that executes and obtains the passphrase. The script must have user execute permission to write the passphrase to stdout, and it must have the necessary operating system protection settings to prevent unauthorized access to the PIN passphrase.

   The following example shows a vpkcs11.xml file that invokes the hsmpin.sh to echo the passphrase:

   ```
   <?xml version="1.0" encoding="utf-8"?>
    <ConfigurationFragment provider="cryptov">

       <Engine name="vpkcs11" defaultFor="">
           <EngineCommand when="preInit" name="REALMS_DIR"
                value="$VINSTDIR/conf/certrealms" />
           <EngineCommand when="preInit" name="PASSPHRASE_EXEC"
                value="$VDISTDIR/hsmpin.sh" />
       </Engine>
       </ConfigurationFragment>
   ```
3. API Gateway provides the certificate realm as an argument to the script, so you can use the same script to initialize multiple realms. The following examples show scripts that write a PIN of `1234` to stdout when initializing the `JMS Keys` certificate realm:

   ```
   #!/bin/sh
   case $1 in
     "JMS Keys")
       # Test your echo command in sh if using special characters.
       echo 1234
       ;;
   esac
   ```

## Configure SSH key pairs

To configure public-private key pairs in the certificate store, select **Environment Configuration > Certificates and Keys > Key Pairs**. The **Key Pairs** window enables you to add, edit, or delete PEM public-private key pairs, which are required for the Secure Shell (SSH) File Transfer Protocol (SFTP).

### Add a key pair

To add a public-private key pair, click **Add** on the right, and configure the following settings in the dialog:

* **Alias**: Enter a unique name for the key pair.
* **Algorithm**: Enter the algorithm used to generate the key pair. Defaults to `RSA`.
* **Load**: Click to select the public key or private key files to use. The **Fingerprint** field is auto-populated when you load a public key. The keys must be OpenSSL compatible PEM keys. RSA keys are supported, but DSA keys are not supported. The keys must not be passphrase protected.

### Edit a key pair

To edit a public-private key pair, select a key pair alias in the table, and click **Edit** on the right. For example, you can load a different public key and private key. Alternatively, double-click a key pair alias in the table to edit it.

You can delete a selected key pair from the certificate store by clicking **Remove** on the right. Alternatively, click **Remove All**.

### Manage SSH keys

You can use the `ssh-keygen` command provided on Linux to manage SSH keys.

{{< alert title="Note" color="primary" >}}
With the release of OpenSSH 7.8, the default private key format for private keys generated from `ssh-keygen` has changed from OpenSSL compatible PEM files to a custom key format created by the OpenSSH developers.{{< /alert >}}

This OpenSSH format is not supported by API Gateway, so the keys should be created in PEM format before the keys can be used. For example, the following command creates an SSH key in PEM format:

```
ssh-keygen -t rsa -m pem -f my_key
```

The following command removes a passphrase (enter the old passphrase, and enter nothing for the new passphrase):

```
ssh-keygen -p -m pem -f my_key
```

The following command outputs the key fingerprint:

```
 ssh-keygen -lf my_key.pub
```

## Configure PGP key pairs

To configure Pretty Good Privacy (PGP) key pairs in the certificate store, select **Environment Configuration > Certificates and Keys > PGP Key Pairs**. The **PGP Key Pairs** window enables you to add, edit, or delete PGP public-private key pairs.

### Add a PGP key pair

To add a PGP public-private key pair, click the **Add** on the right, and configure the following settings in the dialog:

**Alias**: Enter a unique name for the PGP key pair.
**Load**: Click **Load** to select the public key and private key files to use.

The PGP keys added must not be passphrase protected.

### Edit a PGP key pair

To edit a PGP key pair, select a key pair alias in the table, and click **Edit** on the right. For example, you can load a different public key and private key. Alternatively, double-click a key pair alias in the table to edit it.

You can delete a selected PGP key pair from the certificate store by clicking **Remove** on the right. Alternatively, click **Remove All**.

### Manage PGP keys

You can use the freely available GNU Privacy Guard (GnuPG) tool to manage PGP key files (available from <http://www.gnupg.org/>). For example:

The following command creates a PGP key:

```
gpg --gen-key
```

For more details, see <http://www.seas.upenn.edu/cets/answers/pgp_keys.html>

The following command enables you to view the PGP key:

```
gpg -a --export
```

The following command exports a public key to a file:

```
gpg --export -u 'UserName ' -a -o public.key
```

The following command exports a private key to a file:

```
gpg --export-secret-keys -u 'UserName ' -a -o private.key
```

The following command lists the private keys:

```
gpg --list-secret-keys
```

## Global import and export options

This section describes global import and export options available when managing certificates and keys.

The following global configuration options apply to both the **X.509 Certificate** and **Private Key** tabs:

* **Import Certificate + Key**: Use this option to import a certificate and a key (for example, from a `.p12` file).
* **Export Certificate + Key**: Use this option to export a certificate and a key (for example, to a `.p12` file).

Click **OK** when you have finished configuring the certificate and private key.

### Manage certificates in Java keystores

You can also export a certificate to a Java keystore. You can do this by clicking **Keystore** on the main **Certificates** window. Click the browse button at beside the **Keystore** field at the top right to open an existing keystore, or click **New Keystore** to create a new keystore. Choose the name and location of the keystore file, and enter a passphrase for this keystore when prompted. Click **Export to Keystore**, and select a certificate to export.

Similarly, you can import certificates and keys from a Java keystore into the certificate store. To do this, click **Keystore** on the main **Certificates** window. On the **Keystore** window, browse to the location of the keystore by clicking the browse button beside the **Keystore** field. The certificates/keys in the keystore are listed in the table.

To import any of these keys to the certificate store, select the box next to the certificate or key to import, and click **Import to Trusted certificate store**. If the key is protected by a password, you are prompted for this password.

You can also use the **Keystore** window to view and remove existing entries in the keystore. You can also add keys to the keystore and to create a new keystore. Use the appropriate button to perform any of these tasks.

## Generate a CSR and import the certificate and key

You can use a Certificate Signing Request (CSR) from a Certificate Authority (CA) to obtain certificates used by the gateway. Policy Studio can generate both X.509 certificates and associated private keys. However, it cannot generate a CSR. You may need to generate a CSR to request a CA to issue a certificate for use in the gateway.

This topic explains how to generate a CSR using the open source OpenSSL tool. It explains how to import the resulting certificate and corresponding private key into Policy Studio to be used in gateway configuration (for example, for SSL, signing, and encryption).

### How are certificates and keys stored in API Gateway?

The gateway runtime incorporates X.509 certificate and private key material in its configuration store. However, this is not a Java Key Store (JKS).

A common misunderstanding is that certificates are trusted because they are imported into the gateway configuration, and displayed in the **Certificates** view in Policy Studio. However, imported certificates are not trusted by default, and must be configured in Policy Studio.

### What is OpenSSL?

OpenSSL is a free, popular and robust open source toolkit implementing Secure Sockets Layer (SSL) v2 and v3, Transport Layer Security (TLS) v1, and a full-strength general purpose cryptography library. You can use OpenSSL to easily create CSRs. You can also use OpenSSL to create self-signed certificates for use in SSL or message-level security scenarios in a development environment for testing purposes. However, if required, it is considerably faster and easier to create self-signed developer certificates directly in Policy Studio, and there is no need to use an external CA.

### Create a private key and CSR

You can use Policy Studio to generate certificates and private keys. However, certificates created in this way must be signed (self-signed or by a private key already configured in the tool). In most cases, this is not appropriate, so you should create the certificate and private key using a 3rd party tool such as OpenSSL.

The private key is required to generate the X.509 certificate and corresponding CSR. For example, the following command creates a private key file named `mycompanyca.key` with a key length of 2048 bits (strong) and a corresponding CSR in the file `mycompany.csr`:

```
openssl req -out mycompany.csr -new -newkey rsa:2048 -nodes \
  -keyout mycompany.key
Country Name (2 letter code) []:US
State or Province Name (full name) [Some-State]:Massachusetts
Locality Name (e.g., city) []:Boston
Organization Name (e.g., company) [My Company Ltd]:Acme
Organizational Unit Name (e.g., section) []:
Common Name (e.g., server's hostname) [myserver]:api.acme.comEmail
Address []:api-admin@acme.com

Please enter the following 'extra' attributes to be sent with your certificate request
A challenge password []:
An optional company name []:Acme
```

You can now submit the CSR to a public or private CA, and the CA will issue the certificate to be imported into API Gateway for use in SSL and message signing operations.

### Submit the CSR to the CA

Each CA will have a slightly different process for submitting a CSR. However, most CAs provide a web-based interface for this purpose. After you submit the CSR to the appropriate CA, the CA will provide a signed version of the certificate, which you can then import into Policy Studio.

### Import the certificate and key into Policy Studio

When you receive a signed certificate from the CA after submitting the CSR generated earlier, you must import both the certificate and associated private key into the appropriate key store in Policy Studio to enable SSL and message-level security. Depending on the CA used and how the certificate and key were generated, the certificate an key can be in separate files or combined in a single file.

Perform the following steps:

1. In Policy Studio, connect to an API Gateway instance.
2. In the tree on the left, select **Certificates**. The certificates are displayed in the pane on the right.
3. Depending on your use case, click the appropriate import button at the bottom right:

   * Design-time: Click **Keystore**, click **Add to keystore** on the subsequent dialog box. This imports the certificate and private key into the key store for Policy Studio.
   * Run-time: Click **Create/Import**. This import the certificate and private key into the runtime key store for API Gateway.
4. Configure the **X.509 Certificate** tab as follows:

   * Click **Import Certificate** if the certificate is in a separate file. If the certificate and key are in the same file, click **Import Certificate + Key**.
   * Browse to `mycompany.crt` or the file that contains both the certificate and private key. Ensure that the correct file type is set in the file selector at the bottom right (usually `.pem`).
5. Configure the **Private Key** tab as follows:

   * Select the **Private Key** tab (you must import both the certificate and the associated private key).
   * Click **Import Private Key**.
   * Browse to `mycompany.key`. Ensure that the correct file type is set in the file selector at the bottom right.
6. Click **OK** to complete importing the key and certificate.