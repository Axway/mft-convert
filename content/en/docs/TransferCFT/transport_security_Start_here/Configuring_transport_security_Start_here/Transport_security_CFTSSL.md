---

    title: Transport  security in CFTSSL 
    linkTitle: CFTSSL configuration
    weight: 180

---
<span id="Defining_a_transport_security_profile"></span>

## Defining a transport security profile

Transfer CFT opens an SSL session in client mode for any secure file
or message transfer in requester mode. Transfer CFT opens an SSL session
in server mode for any incoming call requiring a secure protocol.

The properties of each SSL session opened by Transfer CFT in both client
and server modes are determined by a security profile.

A security profile monitors the:

- Required authentication
    mode: simple (server only) or mutual (server and client) authentication
- Authentication,
    encryption and sealing algorithms to be used
- Certificate to
    be sent for local authentication and the RSA authentication algorithm
- Checks to be performed
    on the remote certificate for remote authentication and the RSA authentication
    algorithm

The CFTSSL command describes a security
profile. The DIRECT parameter indicates the mode to which the security
profile applies: DIRECT=CLIENT for client mode or DIRECT=SERVER for server
mode.

> **Note**
>
> The Transfer CFT CFTSSL object in client mode is a dynamic object. However, when set to server mode this object is static (you must restart Transfer CFT for the value to be taken into account).

## Client mode SSL

QQQ\_QQQ\_CHECK these tables were REALLY big, I don't think they are viable in markdown

### Syntax of CFTSSL in client mode

`CFTSSL`

` ID   identifier,`

`     DIRECT =       CLIENT,`

`     [USERCID =       identifier,]`

`     CIPHLIST =       (num, num, ..),`

`     [ROOTCID =       (identifier, identifier,..),]`

`     [DEPTH =       {10 |num},]`

`     [VERSION =       {TLSV1 | SSLV3},]`

`     [PARM =       string,]`

`     [DNUSER =       (string, string,..),]`

`     [DNISSUER =       (string, string,..),]`

`     [CERFNAME =       string,]`

`     [MODE =       {REPLACE | CREATE | DELETE},]`

`     [ORIGIN        = string,]`

`     [TRACE         = num,]`

`[VERIFY = { ENFORCED |  NONE (NOTE: SAME AS OPTIONAL | REQUIRED) }, ]`

### Description of CFTSSL in client mode

Use this command to describes a security profile.

### Parameters of CFTSSL in client mode

#### \[CERFNAME = string1..64\]

File name root in which the remote user chain of certificates
is recorded.

If this parameter is set and the remote partner is authenticated,
the chain of certificates proposed by the partner is recorded in DER format
in the file, the root of which is composed of the CERFNAME parameter value
and the unique name set by CFT. The full name of the file can be accessed
via the &SSLCFNAM symbolic variable or catalog query APIs. The file
is systematically deleted when the corresponding catalog entry is purged.

#### CIPHLIST = {(num, num, ..)}

List of algorithms supported.

Each value defines three algorithms:

- Authentication
    algorithm
- Encryption
    algorithm
- Sealing
    algorithm

This list is submitted to the server which then makes its
selection, depending on the client's preference.

********<span class="autonumber"></span><span id="Supported_suites"></span>Supported suites********

QQQ\_QQQ\_QQQ\_QQQ


| Suite  | Order used | Authentication  | Confidentiality  | Integrity  |
| --- | --- | --- | --- | --- |
| 49199 **  | 1  | ECDHE + RSA authentication  | AES-128 GCM  | SHA-256  |
| 49200 **  | 2  | ECDHE + RSA authentication  | AES-256 GCM  | SHA-384  |
| 49191 **  | 3  | ECDHE + RSA authentication | AES-128  | SHA-256  |
| 49192**  | 4  | ECDHE + RSA authentication  | AES-256  | SHA-384  |
| 156 **  | 5  | RSA authentication  | AES 128 GCM  | SHA-256  |
| 157 **  | 6  | RSA authentication  | AES 256 GCM  | SHA-384  |
| 60*  | 7  | RSA authentication (512, 1024, 2048, or 4096)  | AES-128  | SHA-256  |
| 61*  | 8  | RSA authentication (512, 1024, 2048, or 4096)  | AES-256  | SHA-256  |
| 47 &lt;/td&gt;  | 9  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | AES-128 &lt;/td&gt;  | SHA-1 &lt;/td&gt;  |
| 53  | 10  | RSA authentication (512, 1024, 2048, or 4096)  | AES-256  | SHA-1  |
| 10 &lt;/td&gt;  | 11  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | Triple DES &lt;/td&gt;  | SHA-1 &lt;/td&gt;  |
| 5 &lt;/td&gt;  | 12  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | RC4 &lt;/td&gt;  | SHA-1 &lt;/td&gt;  |
| 4 &lt;/td&gt;  | 13  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | RC4 &lt;/td&gt;  | MD5 &lt;/td&gt;  |
| 59*  | 14  | RSA authentication (512, 1024, 2048, or 4096)  | None  | SHA-256  |
| 2 &lt;/td&gt;  | 15  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | None &lt;/td&gt;  | SHA-1 &lt;/td&gt;  |
| 1 &lt;/td&gt;  | 16  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | None &lt;/td&gt;  | MD5 &lt;/td&gt;  |


> **Note**
>
> \* To comply with security standards, as of Transfer CFT version 3.2.0 the use of the cipher suites 59, 60, and 61 is restricted to TLS 1.2 exclusively. This means that you cannot negotiate a session with another partner (monitor) that is using a TLS version lower than 1.2 with these cipher suites.

> **Note**
>
> \*\* These cipher suites are only available for Transfer CFT 3.2.2 and higher and are restricted to use with TLS 1.2.

#### \[DEPTH = {10 | num}\]

Maximum number of intermediate authorities authorized for
the remote certificate.

This optional parameter has a numeric value between 0 and
10 (default value). 0 means that only self-signed certificates are accepted.
1 means that only certificates that are self-signed or signed by a recognized
root authority are accepted.

#### DIRECT = CLIENT

Security profile for the client mode.

#### \[DNISSUER = string1..512\]

List of values to be checked in the DN of the entity that directly issued the remote certificate.

Strings are limited to 512 bytes each. A check is
performed as follows:

- dnuser='C=FR/O=
    Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this
    string.
- dnuser='C=UK,O=Axway'
    means that the remote certificate must contain 'C=UK' string OR 'O=Axway'
    string.

Note that the different attributes of the dnuser or dnissuer
string are separated by the '/' character.

#### \[DNUSER = string1..512\]

List of values to be checked in the remote certificate DN.

Strings are limited to 512 bytes each. A check is
performed as follows:

- dnuser='C=FR/O=
    Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this
    string.
- dnuser='C=UK,O=Axway'
    means that the remote certificate must contain 'C=UK' string OR 'O=Axway'
    string.

Note that the different attributes of the dnuser or dnissuer
string are separated by the '/' character.

#### ID = identifier

Security profile identifier.

\[MODE = {REPLACE
| CREATE | DELETE}\]

Action for the command. For DELETE mode, the command is
deleted from the PARAMETERS database; only the ID and DIRECT parameters
are required.

#### \[ORIGIN = string \]

This parameter indicates the origin of an object.

#### PASSW = string

The parameter is the PassPort entity password for a user's certificate (the password that corresponds with the USERCID). This parameter enables PassPort connectivity.

#### \[PARM = string1..64\]

Freeform parameter associated with the security profile.

This local data item is not used by the SSL protocol. It
can be reused as a symbolic variable in an end of transfer procedure (&SSLPARM).
It is also passed to the end of transfer, directory or file exits.

#### \[ROOTCID = (identifier, identifier, ...)\]

List of certificate authorities. This list can reference a maximum of 10 identifiers in the local certificate database.

In client mode, this list is used to check the server
certificate. Only certificates signed by one of the authorities in the
ROOTCID parameter are accepted.

> **Note**
>
> To use more than 10 identifiers, you can refer to the PKIENTITY information.

#### \[TRACE = number \]

SSL trace level.

#### \[USERCID = identifier\]

Local user certificate. Within the context of the Transfer
CFT integrated PKI, this identifier
refers to the identifier of a user certificate in the database.

This parameter is used to select a user certificate for
authentication by the server (if requested by the server). The first user
certificate found (signed by one of the authorities proposed by the server)
is used.

The &USERID and &PART symbolic variables are accepted.  
These variables are then substituted with the name of the transfer owner
(refer to the CFTAPPL, CFTSEND and CFTRECV commands) and the local identifier
of the transfer partner respectively.

#### \[VERSION = {TLSV1 | SSLV3 | TLSV1COMP | SSLV3COMP}\]

Session version.

Transfer CFT supports:

- TLS version 1.2, 1.1, 1.0 (TLSV1 | TLSV1COMP keyword)
- SSL version 3.0 (SSLV3 | SSLV3COMP keyword)

**About sessions**

In Transfer CFT, the SSL/TLS session version is proposed by the client and negotiated with the server.

**Client mode**

DIRECT=CLIENT

In client mode, TLSV1COMP or SSLV3COMP sets the header length in the NSDU to enable compatibility with other products.

**Server mode**

DIRECT=SERVER

In server mode, the header length is automatically detected for all SSL versions (SSLV3, TLSV1, SSLV3COMP, TLSV1COMP).

Limitation: The header length policy in the protocol header is only available for the PeSIT (ANY) protocol.

#### VERIFY

Sets the authentication mode requirement.

- ENFORCED: Ensures client authentication with the server. The transfer fails if the server does not ask for the client certificate during the handshake.
- OPTIONAL and REQUIRED: The same as NONE (enabling backward compatibility), but should not be used.
- NONE: No authentication required.

## CFTSSL command in Server mode

### Syntax of CFTSSL in Server mode

`CFTSSL`  
`     ID                 =     identifier,`  
`     DIRECT          =       SERVER,`  
`     [USERCID          =       identifier,]`  
`     CIPHLIST          =       (num, num, ..),`  
`     [VERIFY          =       { REQUIRED | OPTIONAL | NONE},]`

`     [ROOTCID          =       (identifier, identifier,..),]`  
`     [DEPTH          =       {10,num},]`  
`     [VERSION          =       {TLSV1 | SSLV3},]`  
`     [PARM          =       string,]`

`      [ORIGIN        =     string,]`

`      [TRACE         =     num,]     [DNUSER          =       (string, string,..),]`  
`     [DNISSUER     =       (string, string,..),]`  
`     [CERFNAME     =       string,]`  
`     [MODE          =       {REPLACE | CREATE | DELETE},]`

### Description

Use this command to describes a security profile.

### Parameters of CFTSSL in Server mode

#### ID = identifier

Identifier of the security profile.

#### \[CERFNAME = string1..64\]

File name root used to record the remote user chain of
certificates.

If this parameter is set and the remote partner is authenticated,
the chain of certificates proposed by the partner is recorded in DER format
in a file, the root of which comprises the CERFNAME parameter value and
the unique name set by Transfer CFT. The full name of the file can be
accessed via the &SSLCFNAM symbolic variable or catalog query APIs.
The file is systematically deleted when the corresponding catalog entry
is purged.

#### CIPHLIST = {(num, num, ..)}

List of the algorithms supported.

Each value defines three algorithms:

- Authentication
    algorithm
- Encryption
    algorithm
- Sealing
    algorithm

This list is compared with the list proposed by the client
in order of preference, for the purpose of determining the suite to be
negotiated.

#### \[DEPTH = {10 | num}\]

Maximum number of intermediate authorities authorized for
the remote certificate.

This optional parameter has a numerical value between 0
and 10, the default value. 0 signifies that only self-signed certificates
are accepted. 1 signifies that only certificates that are self-signed
or signed by a recognized root authority are accepted.

DIRECT = SERVER

The security profile is applicable in the server mode.

#### \[DNISSUER = string1...512\]

Strings that are limited to 512 bytes each. A check is
performed as follows:

- dnuser='C=FR/O=
    Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this
    string.
- dnuser='C=UK,O=Axway'
    means that the remote certificate must contain 'C=UK' string OR 'O=Axway'
    string.

Note that the different attributes of the dnuser or dnissuer
string are separated by the '/' character.

#### \[DNUSER = string1..512\]

Strings that are limited to 512 bytes each. A check is
performed as follows:

- dnuser='C=FR/O=
    Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this
    string.
- dnuser='C=UK,O=Axway'
    means that the remote certificate must contain 'C=UK' string OR 'O=Axway'
    string.

Note that the different attributes of the dnuser or dnissuer
string are separated by the '/' character.

#### \[MODE = {REPLACE | CREATE | DELETE}\]

Action for the command. For DELETE mode, the command is
deleted from the PARAMETERS database; only the ID and DIRECT parameters
are required.

#### \[ORIGIN = string \]

This parameter indicates the origin of an object.

#### \[PARM = string1..64\]

Freeform parameter associated with the security profile.

This local data item is not used by the SSL protocol. It
can be reused as a symbolic variable in an end of transfer procedure (&SSLPARM).
It is also passed to the end of transfer, directory or file exits.

#### \[ROOTCID = (identifier, identifier, ...)\]

List of the certificate authorities. This list references
identifiers in the local certificate database.

This list has two functions:

- A user
    certificate can be chosen for authentication by the client
- If the
    client must be authenticated, the list of authority DNs supported by the
    server is supplied

> **Note**
>
> To use more than 10 identifiers, refer to the PKIENTITY information.

#### \[TRACE = number \]

SSL trace level.

#### \[USERCID = identifier\]

Reference of a user certificate in the local certificate
database. The purpose of this identifier is to select a user certificate
for authentication by the client.

If the server
requires client authentication, it provides the client with the list of
authority DNs supported during the SSL protocol negotiation phase.

> **Note**
>
> This parameter
> is ignored when the CFTSSL command is used for additional controls in
> the server mode (CFTSSL command indicated by a CFTPART command).

#### VERIFY = {REQUIRED | OPTIONAL | NONE}

Sets the authentication mode requirement.

The VERIFY and USERCID parameters set the security profile
authentication mode.

> **Note**
>
> With the SSL
> protocol, the server determines whether the client must be authenticated.
> If the server wants the client to be authenticated, then the server must
> first be authenticated by the client. Due to the algorithms supported
> by Transfer CFT (refer to the CIPHLIST parameter), the server must currently
> be authenticated by an X.509 certificate.

The following list describes the possible cases for a security
profile in server mode (the USERCID parameter is mandatory for
a server CFTSSL command).

- REQUIRED: The server and the client must be authenticated. (Default)
- OPTIONAL: The server and the client must be authenticated. An invalid
    certificate is tolerated by the server. 
- NONE : Only the server must be authenticated. 

#### \[VERSION = {TLSV1 | SSLV3 | TLSV1COMP | SSLV3COMP}\]

SSL session version.

****Client mode****

In Client mode (DIRECT=CLIENT), TLSV1COMP or SSLV3COMP set the header length in NSDU to enable compatibility with other products.

****Server mode****

In server mode (DIRECT=SERVER) the header length is automatically detected for all SSL versions (SSLV3, TLSV1, SSLV3COMP, TLSV1COMP).

Limitation: The header length policy in the protocol header is only available for the PeSIT (ANY) protocol.

### Associate the security profile with the CFTPROT object

The SSL parameter is used to associate a security profile with a protocol
definition.

- For a security profile related to incoming calls (server mode), the
    SSL parameter value must correspond to the identifier of a DIRECT=SERVER
    SSL command.
- For a default security profile related to outgoing calls (client mode),
    the SSL parameter value must correspond to the identifier of a DIRECT=CLIENT
    SSL command.

If you do not define the CFTPROT SAP parameter when using the SSL protocol,
then the server mode for CFTSSL is not mandatory.

****Related topics****

- [CFTPROT](../../../admin_intro/admin_config_commands/transfer_protocol_concepts)
