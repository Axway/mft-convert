{
    "title": "Transport  security in CFTSSL ",
    "linkTitle": "CFTSSL configuration",
    "weight": "170"
}<span id="Defining_a_transport_security_profile"></span>

Defining a transport security profile
-------------------------------------

{{% TransferCFT/snippets/cftssl_concept%}}

> **Note**
>
> Note: The Transfer CFT CFTSSL object in client mode is a dynamic object. However, when set to server mode this object is static (you must restart Transfer CFT for the value to be taken into account).

Client mode
-----------

Use this command to describes a security profile.

### Syntax

`CFTSSL`

` ID   identifier,`

`     DIRECT =       CLIENT,`

`     [USERCID =       identifier,]`

`     CIPHLIST =       (num, num, ..),`

`     [ROOTCID =       (identifier, identifier,..),]`

`     [DEPTH =       {10 &#124;num},]`

`     [VERSION =       {TLSV1 &#124; SSLV3},]`

`     [PARM =       string,]`

`     [DNUSER =       (string, string,..),]`

`     [DNISSUER =       (string, string,..),]`

`     [CERFNAME =       string,]`

`     [MODE =       {REPLACE &#124; CREATE &#124; DELETE},]`

`     [ORIGIN        = string,]`

`     [TRACE         = num,]`

`[VERIFY = { ENFORCED &#124;  NONE (NOTE: SAME AS OPTIONAL &#124; REQUIRED) }, ]`

### Parameters


| Parameter  | Description  |
| --- | --- |
| [CERFNAME = string1..64] | File name root in which the remote user chain of certificates is recorded.<br/> If this parameter is set and the remote partner is authenticated, the chain of certificates proposed by the partner is recorded in DER format in the file, the root of which is composed of the CERFNAME parameter value and the unique name set by CFT. The full name of the file can be accessed via the &amp;SSLCFNAM symbolic variable or catalog query APIs. The file is systematically deleted when the corresponding catalog entry is purged. |
| CIPHLIST = {(num, num, ..)} | List of algorithms supported.<br/> Each value defines three algorithms:<br/> • Authentication algorithm<br/> • Encryption algorithm<br/> • Sealing algorithm<br/> This list is submitted to the server which then makes its selection, depending on the client's preference.<br/> ********<span id="Supported_suites"></span>Supported suites********<br/> {{% TransferCFT/snippets/cipher_suites%}}  |
| [DEPTH = {10 &#124; num}] | Maximum number of intermediate authorities authorized for the remote certificate.<br/> This optional parameter has a numeric value between 0 and 10 (default value). 0 means that only self-signed certificates are accepted. 1 means that only certificates that are self-signed or signed by a recognized root authority are accepted. |
| DIRECT = CLIENT | Security profile for the client mode. |
| [DNISSUER = string1..512] | List of values to be checked in the DN of the entity that directly issued the remote certificate.<br/> Strings are limited to 512 bytes each. A check is performed as follows:<br/> • dnuser='C=FR/O= Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this string.<br/> • dnuser='C=UK,O=Axway' means that the remote certificate must contain 'C=UK' string OR 'O=Axway' string.<br/> Note that the different attributes of the dnuser or dnissuer string are separated by the '/' character. |
| [DNUSER = string1..512] | List of values to be checked in the remote certificate DN.<br/> Strings are limited to 512 bytes each. A check is performed as follows:<br/> • dnuser='C=FR/O= Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this string.<br/> • dnuser='C=UK,O=Axway' means that the remote certificate must contain 'C=UK' string OR 'O=Axway' string.<br/> Note that the different attributes of the dnuser or dnissuer string are separated by the '/' character. |
| ID = identifier | Security profile identifier. |
| [MODE = {REPLACE &#124; CREATE &#124; DELETE}] | Action for the command. For DELETE mode, the command is deleted from the PARAMETERS database; only the ID and DIRECT parameters are required. |
| [ORIGIN = string ]  | This parameter indicates the origin of an object.  |
| PASSW<br/> string | The parameter is the PassPort entity password for a user's certificate (the password that corresponds with the USERCID). This parameter enables PassPort connectivity. |
| [PARM = string1..64]  | Freeform parameter associated with the security profile.<br/> This local data item is not used by the SSL protocol. It can be reused as a symbolic variable in an end of transfer procedure (&amp;SSLPARM). It is also passed to the end of transfer, directory or file exits. |
| [ROOTCID = (identifier, identifier, ...)] | List of certificate authorities. This list can reference a maximum of 10 identifiers in the local certificate database.<br/> In client mode, this list is used to check the server certificate. Only certificates signed by one of the authorities in the ROOTCID parameter are accepted.<br/> <blockquote> **Note**<br/> Note: To use more than 10 identifiers, you can refer to the PKIENTITY information.<br/> </blockquote>  |
| [TRACE = number ]  | SSL trace level.  |
| [USERCID = identifier] | Local user certificate. Within the context of the Transfer CFT integrated PKI, this identifier refers to the identifier of a user certificate in the database.<br/> This parameter is used to select a user certificate for authentication by the server (if requested by the server). The first user certificate found (signed by one of the authorities proposed by the server) is used.<br/> The &amp;USERID and &amp;PART symbolic variables are accepted.<br /> These variables are then substituted with the name of the transfer owner (refer to the CFTAPPL, CFTSEND and CFTRECV commands) and the local identifier of the transfer partner respectively. |
| [VERSION = {TLSV1 &#124; SSLV3 &#124; TLSV1COMP &#124; SSLV3COMP}] | Session version.<br/> {{% TransferCFT/snippets/version%}} Limitation: The header length policy in the protocol header is only available for the PeSIT (ANY) protocol. |
| VERIFY  | Sets the authentication mode requirement.<br/> • ENFORCED: Ensures client authentication with the server. The transfer fails if the server does not ask for the client certificate during the handshake.<br/> • OPTIONAL and REQUIRED: The same as NONE (enabling backward compatibility), but should not be used.<br/> • NONE: No authentication required. |


Server mode
-----------

Use this command to describes a security profile.

### Syntax

`CFTSSL`  
`     ID                 =     identifier,`  
`     DIRECT          =       SERVER,`  
`     [USERCID          =       identifier,]`  
`     CIPHLIST          =       (num, num, ..),`  
`     [VERIFY          =       { REQUIRED &#124; OPTIONAL &#124; NONE},]`

`     [ROOTCID          =       (identifier, identifier,..),]`  
`     [DEPTH          =       {10,num},]`  
`     [VERSION          =       {TLSV1 &#124; SSLV3},]`  
`     [PARM          =       string,]`

`      [ORIGIN        =     string,]`

`      [TRACE         =     num,]     [DNUSER          =       (string, string,..),]`  
`     [DNISSUER     =       (string, string,..),]`  
`     [CERFNAME     =       string,]`  
`     [MODE          =       {REPLACE &#124; CREATE &#124; DELETE},]`

### Parameters


| Parameter  | Description  |
| --- | --- |
| ID = identifier | Identifier of the security profile. |
| [CERFNAME = string1..64] | File name root used to record the remote user chain of certificates.<br/> If this parameter is set and the remote partner is authenticated, the chain of certificates proposed by the partner is recorded in DER format in a file, the root of which comprises the CERFNAME parameter value and the unique name set by Transfer CFT. The full name of the file can be accessed via the &amp;SSLCFNAM symbolic variable or catalog query APIs. The file is systematically deleted when the corresponding catalog entry is purged. |
| CIPHLIST = {(num, num, ..)} | List of the algorithms supported.<br/> Each value defines three algorithms:<br/> • Authentication algorithm<br/> • Encryption algorithm<br/> • Sealing algorithm<br/> This list is compared with the list proposed by the client in order of preference, for the purpose of determining the suite to be negotiated. |
| [DEPTH = {10 &#124; num}] | Maximum number of intermediate authorities authorized for the remote certificate.<br/> This optional parameter has a numerical value between 0 and 10, the default value. 0 signifies that only self-signed certificates are accepted. 1 signifies that only certificates that are self-signed or signed by a recognized root authority are accepted. |
| DIRECT = SERVER | The security profile is applicable in the server mode. |
| [DNISSUER = string1...512] | Strings that are limited to 512 bytes each. A check is performed as follows:<br/> • dnuser='C=FR/O= Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this string.<br/> • dnuser='C=UK,O=Axway' means that the remote certificate must contain 'C=UK' string OR 'O=Axway' string.<br/> Note that the different attributes of the dnuser or dnissuer string are separated by the '/' character. |
| [DNUSER = string1..512] | Strings that are limited to 512 bytes each. A check is performed as follows:<br/> • dnuser='C=FR/O= Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this string.<br/> • dnuser='C=UK,O=Axway' means that the remote certificate must contain 'C=UK' string OR 'O=Axway' string.<br/> Note that the different attributes of the dnuser or dnissuer string are separated by the '/' character. |
| [MODE = {REPLACE &#124; CREATE &#124; DELETE}] | Action for the command. For DELETE mode, the command is deleted from the PARAMETERS database; only the ID and DIRECT parameters are required. |
| [ORIGIN = string ]  | This parameter indicates the origin of an object.  |
| [PARM = string1..64] | Freeform parameter associated with the security profile.<br/> This local data item is not used by the SSL protocol. It can be reused as a symbolic variable in an end of transfer procedure (&amp;SSLPARM). It is also passed to the end of transfer, directory or file exits. |
| [ROOTCID = (identifier, identifier, ...)] | List of the certificate authorities. This list references identifiers in the local certificate database.<br/> This list has two functions:<br/> • A user certificate can be chosen for authentication by the client<br/> • If the client must be authenticated, the list of authority DNs supported by the server is supplied<br/> <blockquote> **Note**<br/> Note: To use more than 10 identifiers, refer to the PKIENTITY information.<br/> </blockquote>  |
| [TRACE = number ]  | SSL trace level.  |
| [USERCID = identifier] | Reference of a user certificate in the local certificate database. The purpose of this identifier is to select a user certificate for authentication by the client.<br/> If the server requires client authentication, it provides the client with the list of authority DNs supported during the SSL protocol negotiation phase.<br/> <blockquote> **Note**<br/> Note: This parameter is ignored when the CFTSSL command is used for additional controls in the server mode (CFTSSL command indicated by a CFTPART command).<br/> </blockquote>  |
| VERIFY = {REQUIRED &#124; OPTIONAL &#124; NONE} | Sets the authentication mode requirement.<br/> The VERIFY and USERCID parameters set the security profile authentication mode.<br/> <blockquote> **Note**<br/> Note: With the SSL protocol, the server determines whether the client must be authenticated. If the server wants the client to be authenticated, then the server must first be authenticated by the client. Due to the algorithms supported by Transfer CFT (refer to the CIPHLIST parameter), the server must currently be authenticated by an X.509 certificate.<br/> </blockquote> The following list describes the possible cases for a security profile in server mode (the USERCID parameter is mandatory for a server CFTSSL command).<br/> • REQUIRED: The server and the client must be authenticated. (Default)<br/> • OPTIONAL: The server and the client must be authenticated. An invalid certificate is tolerated by the server. <br/> • NONE : Only the server must be authenticated.  |
| [VERSION = {TLSV1 &#124; SSLV3 &#124; TLSV1COMP &#124; SSLV3COMP}] | SSL session version.<br/> ****Client mode****<br/> In Client mode (DIRECT=CLIENT), TLSV1COMP or SSLV3COMP set the header length in NSDU to enable compatibility with other products.<br/> ****Server mode****<br/> In server mode (DIRECT=SERVER) the header length is automatically detected for all SSL versions (SSLV3, TLSV1, SSLV3COMP, TLSV1COMP).<br/> Limitation: The header length policy in the protocol header is only available for the PeSIT (ANY) protocol. |


Associate the security profile with the CFTPROT object
------------------------------------------------------

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
