{
    "title": "Using  PKI exits: Start here",
    "linkTitle": "Using PKI exits",
    "weight": "190"
}This book describes the rules for implementing the PKI exit. This book
begins with this topic which describes
the following PKI exit concepts:

- [Using
    PKI exits](#Using_PKI_exits__Start_here)
- [Activating](#Activating)
- [Processing](#Processing)
- [Defining
    a PKI exit](#Defining_a_PKI_exit)

<span id="Using_PKI_exits"></span>

PKI exit overview
-----------------

The PKI exit is used to extract all or part of an external PKI, third-party
software, or OS APIs. It enables a user to take control at different stages
when opening an SSL session, to handle authentication in place of the
Transfer CFT PKI.

The PKI exit cannot be used to manage the SSL handshake phase directly.
It does not, for example, allow you to use security algorithms other than
those supported by Transfer CFT.

The PKI exit can be used when:

- Creating an SSL
    task StartTask
- Opening an SSL
    session (StartHandshake)
- Getting a user
    certificate chain to be sent (GetCertificate)
- Getting a list
    of CAs to be sent (GetCAList)
- Checking a user
    certificate received (CheckCertificate)
- Encrypting using
    the private key (CryptData)
- Activating an alert
    during the handshake phase (HandshakeAlert)
- The SSL session
    is established (HandshakeDone)
- The SSL session
    is released (EndSession)
- An SSL task has
    been terminated (EndTask)

<span id="Activating"></span>

### Activating

You do not need to modify the Transfer CFT configuration to implement
a PKI exit. The PKI exit is automatically called in the SSL task initialization
phase (StartTask). In return, it indicates whether it must then
be called for the other phases.

A default exit is supplied with the Transfer CFT product. During the
StartTask phase, this exit indicates that it is not to be requested
during the subsequent phases.

<span id="Processing"></span>

### Processing

A PKI exit co-exists with the Transfer CFT PKI, the local certificate
internal datafile specific to the product.

It is first called at each step described previously and:

- May handle processing
    associated with each step
- May ignore any
    processing associated with each step, in which case, the Transfer CFT
    PKI takes over

<span id="Defining_a_PKI_exit"></span>

### Defining a PKI exit

****Unix/Windows only****

A Transfer CFT PKI exit is called through a C function called `cftpkie`. How the PKI exit is processed by Transfer CFT then depends on the operating system.

To define a PKI exit, generate a DLL and set the following UCONF parameters:

- Set `pki.type` = `exit`
- Specify `pki.exit.libpath` as the path to the dynamic load library

A sample of how to generate this exit is located in the `<cft.runtime_dir>/src/exit` directory. You can invoke a makefile to generate this DLL (among other things) from the `cftpkie.c` source file located in the same directory. Before calling this makefile, load the Transfer CFT profile.

You can define only one user function, `cftpkie`, for Transfer
CFT, with the syntax as follows:

```
int
cftpkie(struct pkicom \*zpki)
```

The `pkicom `structure is used to exchange data between Transfer CFT and
the user function. The function must return an integer, which represents
a general execution code (processing successfully completed by the function,
processing error, processing not handled by the exit).

In addition to the information found in this User Guide, the `<cft.install_dir>/inc/cftpkie.h` file contains all the definitions required to develop a user function, such as constants, communication structure definition, and comments.
