{
    "title": "Managing  PKI exits",
    "linkTitle": "Managing PKI exits",
    "weight": "210"
}This topic describes the steps involved in a PKI exit, as well as how
PKI exits work in Transfer CFT transport security. PKI processes and concepts
include:

- [PKI
    exit fields](#PKI_exit_fields)
- [Creating
    an SSL task](#Creating_an_SSL_task)
- [Terminating
    an SSL task](#Terminating_an_SSL_task)
- [Opening
    an SSL task](#Opening_an_SSL_task)
- [Sending
    a CA list](#Getting_a_CA_list_to_be_sent)
    -   [Getting
        a certificate to be sent](#Getting_a_certificate_to_be_sent)
    -   [Checking
        a received certificate](#Checking_a_received_certificate)
- [Encrypting with the private
    key](#Encrypting_with_the_private_key)

An exit is a point in a software program at
which a user-developed program can take control of program processes.
One type of exit is a PKI exit.

Axway provides a set of C-language user exits
with the Transfer CFT product. These exits are triggered by different
types of Transfer CFT processing events.

<span id="PKI_exit_fields"></span>

PKI exit fields
---------------

You can access additional transport security fields in
the End of Transfer
and Directory type
exits. These fields indicate if transport security has been implemented
and if so, the session parameters such as authentication mode, suite
negotiated, certificates used, and so on.

A user area may be populated by an external
PKI type exit and supplied to the End
of Transfer and Directory type
exits.

<span id="Creating_an_SSL_task"></span>

Creating an SSL task
--------------------

The cftpkie function is systematically called in the StartTask
phase. According to the Transfer CFT configuration and operating system,
an SSL task:

- Manages one or
    more sessions (SSLMTASK and SSLTTASK parameters)
- Can be executed
    and then stopped by the monitor (SSLWTASK parameter)

Each SSL task created generates a call to the cftpkie function,
indicating the StartTask phase.

The execution return code of the cftpkie function is used to
disable the call to this function for all subsequent phases.

In this phase, the cftpkie function can return the local certificate
internal datafile protection password.

<span id="Terminating_an_SSL_task"></span>

Terminating an SSL task
-----------------------

The EndTask phase is symmetrical to the StartTask phase.
Each SSL task terminated generates a call to the cftpkie function,
indicating the EndTask phase.

<span id="Opening_an_SSL_task"></span>

Opening an SSL task
-------------------

The cftpkie function call in the StartHandshake phase
corresponds to opening an SSL session on an established network connection,
for which no network message has yet been exchanged.

A new context (data structure) is created by Transfer CFT for the session
to be opened. This context is passed to the cftpkie function in
all other session-related phases. It contains the session properties and
is enhanced as the handshake progresses.

In this phase, the context contains the security profile (CFTSSL command
determining the handshake elements) and session network parameters (remote
partner address and listening port). In return, the PKI exit can modify
certain security profile elements.

<span id="Getting_a_CA_list_to_be_sent"></span>

Sending a CA list
-----------------

The cftpkie function in the GetCAList phase is only called in
server mode and corresponds to getting the list of supported authority
DNs. The remote (client) entity must provide a certificate signed by one
of these authorities. The following figure indicates the format that the
cftpkie function must respect when building the list.

********CA list format********

![Format of the certificate showing the corresponding byte for each element](/Images/TransferCFT/Image1876.gif)

The only type of certificate supported by Transfer CFT to date is RSA.
The first two bytes must therefore be set to 1 (list length = 1) and 1
(RSA type).

<span id="Getting_a_certificate_to_be_sent"></span>

### Getting a certificate to be sent

The cftpkie function call in the GetCertificate phase
corresponds to getting the complete user certificate chain, for authentication
by the remote entity.

In client mode, the context is enhanced with the list of authority DNs
supported by the server. The format of the list supplied to the exit is
the same as the one described in the GetCAList phase.

The following figure indicates the format that the cftpkie function
must respect when building the certificate.

********Format of a certificate chain to be sent********

![View of bytes for DER encoding](/Images/TransferCFT/Image1877.gif)

<span id="Checking_a_received_certificate"></span>

### Checking a received certificate

The cftpkie function call in the CheckCertificate phase
corresponds to checking a certificate chain received from the remote entity.
The format of the certificate chain passed to the cftpkie function
is the same as the one for the GetCertificate phase.

The TLS specification (RFC 2246) defines standard certificate check
error codes. If a certificate is refused, you must set the iAlertReason_w
field to a recognized value. This value is sent to the remote entity prior
to disconnection.

Examples of the main error codes are shown below and are followed by
the decimal value in parenthesis:

```
handshake_failure(40)          bad_certificate(42)          unsupported_certificate(43)
certificate_revoked(44)          certificate_expired(45)          certificate_unknown(46)
illegal_parameter     (47)          unknown_ca(48)          access_denied(49)
decode_error(50)              
decrypt_error(51)          export_restriction(60)
insufficient_security(71)          internal_error(80)          user_canceled(90)
```
<span id="Encrypting_with_the_private_key"></span>

Encrypting with the private key
-------------------------------

The cftpkie function call in the CryptData phase corresponds
to encrypting data with the private key associated with the local certificate
used for authentication purposes.

The cftpkie function can perform the whole operation or simply
return the private key and let Transfer CFT encrypt the data.

In client mode, this phase is only requested for mutual authentication
(server and client).

<span id="Alert_during_the_handshake_phase"></span>

### Alert during the handshake phase

The cftpkie function call in the HandshakeAlert phase
corresponds to an error detected by Transfer CFT or indicated by the remote
entity during the handshake phase. Transfer CFT subsequently deletes the
session context (the PKI exit is no longer called for this session) and
releases the network session, or waits for the remote entity to do so.

<span id="Session_established_end_of_handshake"></span>

### Session established end-of-handshake

The cftpkie function call in the HandshakeDone phase corresponds
to the end of the SSL handshake. The PKI type exit is now only called
for the session release event.

<span id="Releasing_an_SSL_session"></span>

### Releasing an SSL session

The cftpkie function call in the EndSession phase corresponds
to the release of an SSL session and the network connection as well.

<span id="SSL_session_context"></span>

### SSL session context

The SSL session context contains the properties of the corresponding
session. It is enhanced as the handshake phase progresses.

It also contains:

- Two SSL task references
    (long integer); one of the references (unique) is supplied by Transfer
    CFT; the other is returned by the cftpkie function in the StartTask
    phase
- Two SSL session
    references (long integer); one of the references (unique for a given task)
    is supplied by Transfer CFT; the other is returned by the cftpkie
    function in the StartHandshake phase
- A public area available
    to the cftpkie function; this area (4 KB) is unique for all the
    contexts of the same task; this area is not modified by Transfer CFT between
    two calls to the cftpkie function, even for two different contexts;
    two SSL tasks do not point to the same public area
- A private area
    available to the cftpkie function; this area is private to a context;
    its size is returned by the cftpkie function in the StartTask
    phase
- 64-byte global
    area available to the cftpkie function; this area is sent in the
    other Transfer CFT exit communication structures (file, directory and
    end of transfer exits); the contents of this area must be set when returned
    by the HandshakeDone phase

The following figure reviews the PKI exit phases for the server mode.

********PKI Exit Phases in Server Mode********

![SSL task execution beginning with a handshake](/Images/TransferCFT/Image1878.gif)

The GetCAList and CheckCertificate phases only exist if
the client must be authenticated.

The server determines whether the client must be authenticated. This
choice is dictated by the security profile (CFTSSL command).

The following figure reviews the PKI exit phases for the client mode.

********PKI exit phases in client mode********

![SSL Task execution from handshake to the end of the SSL task](/Images/TransferCFT/Image1879.gif)

The GetCertificate phase only exists if the client must be authenticated.
The server determines whether the client must be authenticated. If the
client does not have the elements required for authentication, it sends
an alert.
