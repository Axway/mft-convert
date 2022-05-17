---
    title: "Transport  security in CFTSSL "
    linkTitle: "CFTSSL configuration"
    weight: 170
---<span id="Defining_a_transport_security_profile"></span>

## Defining a transport security profile

Transfer CFT opens a secure session in client mode for any secure file
or message transfer in requester mode. Transfer CFT opens a secure session
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

## Client mode

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

<table>
   <tbody>
      <tr>
         <td class="BodyE-Column1-Body1">Parameter         </td>
         <td class="BodyD-Column1-Body1">Description         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2"><p>[CERFNAME = string1..64]</p>         </td>
         <td class="BodyD-Column1-Body2" width="45.371%"><p>File name root in which the remote user chain of certificates
is recorded.</p>
<p>If this parameter is set and the remote partner is authenticated,
the chain of certificates proposed by the partner is recorded in DER format
in the file, the root of which is composed of the CERFNAME parameter value
and the unique name set by CFT. The full name of the file can be accessed
via the &amp;SSLCFNAM symbolic variable or catalog query APIs. The file
is systematically deleted when the corresponding catalog entry is purged.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1"><p>CIPHLIST = {(num, num,
..)}</p>         </td>
         <td class="BodyD-Column1-Body1" width="45.371%"><p>List of algorithms supported.</p>
<p>Each value defines three algorithms:</p>
<ul>
<li>Authentication
algorithm</li>
<li>Encryption
algorithm</li>
<li>Sealing
algorithm</li>
</ul>
<p>This list is submitted to the server which then makes its
selection, depending on the client's preference.</p>
<p><strong><strong><strong><strong><span id="Supported_suites"></span>Supported suites</strong></strong></strong></strong></p>
<table>
   <thead>
      <tr>
<th class="HeadE-Column1-Header1"><p>Suite </p>         </th>
<th style="text-align: center;" class="HeadE-Column1-Header1"><p>Order used</p>         </th>
<th class="HeadE-Column1-Header1"><p>Authentication </p>         </th>
<th class="HeadE-Column1-Header1"><p>Confidentiality </p>         </th>
<th class="HeadD-Column1-Header1"><p>Integrity </p>         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td class="BodyE-Column1-Body1" data-valign="top" width="9%">49199 **         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body1">1         </td>
         <td class="BodyE-Column1-Body1" data-valign="top">ECDHE + RSA authentication         </td>
         <td class="BodyE-Column1-Body1" data-valign="top" width="20%">AES-128 GCM         </td>
         <td class="BodyD-Column1-Body1" data-valign="top" width="27%">SHA-256         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" data-valign="top" width="9%">49200 **         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body2">2         </td>
         <td class="BodyE-Column1-Body2" data-valign="top">ECDHE + RSA authentication         </td>
         <td class="BodyE-Column1-Body2" data-valign="top" width="20%">AES-256 GCM         </td>
         <td class="BodyD-Column1-Body2" data-valign="top" width="27%">SHA-384         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" data-valign="top" width="9%">49191 **         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body1">3         </td>
         <td class="BodyE-Column1-Body1" data-valign="top"><p>ECDHE + RSA authentication</p>         </td>
         <td class="BodyE-Column1-Body1" data-valign="top" width="20%">AES-128         </td>
         <td class="BodyD-Column1-Body1" data-valign="top" width="27%">SHA-256         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" data-valign="top" width="9%">49192**         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body2">4         </td>
         <td class="BodyE-Column1-Body2" data-valign="top">ECDHE + RSA authentication         </td>
         <td class="BodyE-Column1-Body2" data-valign="top" width="20%">AES-256         </td>
         <td class="BodyD-Column1-Body2" data-valign="top" width="27%">SHA-384         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" data-valign="top" width="9%">156 **         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body1">5         </td>
         <td class="BodyE-Column1-Body1" data-valign="top">RSA authentication         </td>
         <td class="BodyE-Column1-Body1" data-valign="top" width="20%">AES 128 GCM         </td>
         <td class="BodyD-Column1-Body1" data-valign="top" width="27%">SHA-256         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" data-valign="top" width="9%">157 **         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body2">6         </td>
         <td class="BodyE-Column1-Body2" data-valign="top">RSA authentication         </td>
         <td class="BodyE-Column1-Body2" data-valign="top" width="20%">AES 256 GCM         </td>
         <td class="BodyD-Column1-Body2" data-valign="top" width="27%">SHA-384         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" data-valign="top" width="9%">60*         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body1">7         </td>
         <td class="BodyE-Column1-Body1" data-valign="top">RSA authentication (512, 1024, 2048, or 4096)         </td>
         <td class="BodyE-Column1-Body1" data-valign="top" width="20%">AES-128         </td>
         <td class="BodyD-Column1-Body1" data-valign="top" width="27%">SHA-256         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" data-valign="top" width="9%">61*         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body2">8         </td>
         <td class="BodyE-Column1-Body2" data-valign="top">RSA authentication (512, 1024, 2048, or 4096)         </td>
         <td class="BodyE-Column1-Body2" data-valign="top" width="20%">AES-256         </td>
         <td class="BodyD-Column1-Body2" data-valign="top" width="27%">SHA-256         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" data-valign="top" width="9%">47
&lt;/td&gt;         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body1">9         </td>
         <td class="BodyE-Column1-Body1" data-valign="top">RSA authentication (512, 1024, 2048, or 4096)
&lt;/td&gt;         </td>
         <td class="BodyE-Column1-Body1" data-valign="top" width="20%">AES-128
&lt;/td&gt;         </td>
         <td class="BodyD-Column1-Body1" data-valign="top" width="27%">SHA-1
&lt;/td&gt;         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" data-valign="top" width="9%">53         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body2">10         </td>
         <td class="BodyE-Column1-Body2" data-valign="top">RSA authentication (512, 1024, 2048, or 4096)         </td>
         <td class="BodyE-Column1-Body2" data-valign="top" width="20%">AES-256         </td>
         <td class="BodyD-Column1-Body2" data-valign="top" width="27%">SHA-1         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" data-valign="top" width="9%">10
&lt;/td&gt;         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body1">11         </td>
         <td class="BodyE-Column1-Body1" data-valign="top">RSA authentication (512, 1024, 2048, or 4096)
&lt;/td&gt;         </td>
         <td class="BodyE-Column1-Body1" data-valign="top" width="20%">Triple DES
&lt;/td&gt;         </td>
         <td class="BodyD-Column1-Body1" data-valign="top" width="27%">SHA-1
&lt;/td&gt;         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" data-valign="top" width="9%">5
&lt;/td&gt;         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body2">12         </td>
         <td class="BodyE-Column1-Body2" data-valign="top">RSA authentication (512, 1024, 2048, or 4096)
&lt;/td&gt;         </td>
         <td class="BodyE-Column1-Body2" data-valign="top" width="20%">RC4
&lt;/td&gt;         </td>
         <td class="BodyD-Column1-Body2" data-valign="top" width="27%">SHA-1
&lt;/td&gt;         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" data-valign="top" width="9%">4
&lt;/td&gt;         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body1">13         </td>
         <td class="BodyE-Column1-Body1" data-valign="top">RSA authentication (512, 1024, 2048, or 4096)
&lt;/td&gt;         </td>
         <td class="BodyE-Column1-Body1" data-valign="top" width="20%">RC4
&lt;/td&gt;         </td>
         <td class="BodyD-Column1-Body1" data-valign="top" width="27%">MD5
&lt;/td&gt;         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" data-valign="top" width="9%">59*         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body2">14         </td>
         <td class="BodyE-Column1-Body2" data-valign="top">RSA authentication (512, 1024, 2048, or 4096)         </td>
         <td class="BodyE-Column1-Body2" data-valign="top" width="20%">None         </td>
         <td class="BodyD-Column1-Body2" data-valign="top" width="27%">SHA-256         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" data-valign="top" width="9%">2
&lt;/td&gt;         </td>
         <td style="text-align: center;" class="BodyE-Column1-Body1">15         </td>
         <td class="BodyE-Column1-Body1" data-valign="top">RSA authentication (512, 1024, 2048, or 4096)
&lt;/td&gt;         </td>
         <td class="BodyE-Column1-Body1" data-valign="top" width="20%">None
&lt;/td&gt;         </td>
         <td class="BodyD-Column1-Body1" data-valign="top" width="27%">SHA-1
&lt;/td&gt;         </td>
      </tr>
      <tr>
         <td class="BodyB-Column1-Body2" data-valign="top" width="9%">1
&lt;/td&gt;         </td>
         <td style="text-align: center;" class="BodyB-Column1-Body2">16         </td>
         <td class="BodyB-Column1-Body2" data-valign="top">RSA authentication (512, 1024, 2048, or 4096) 
&lt;/td&gt;         </td>
         <td class="BodyB-Column1-Body2" data-valign="top" width="20%">None
&lt;/td&gt;         </td>
         <td class="BodyA-Column1-Body2" data-valign="top" width="27%">MD5
&lt;/td&gt;         </td>
      </tr>
   </tbody>
</table>
<blockquote>
<p><strong>Note</strong></p>
<p>* To comply with security standards, as of Transfer CFT version 3.2.0 the use of the cipher suites 59, 60, and 61 is restricted to TLS 1.2 exclusively. This means that you cannot negotiate a session with another partner (monitor) that is using a TLS version lower than 1.2 with these cipher suites.</p>
</blockquote>
<blockquote>
<p><strong>Note</strong></p>
<p>** These cipher suites are only available for Transfer CFT 3.2.2 and higher and are restricted to use with TLS 1.2.</p>
</blockquote>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2"><p>[DEPTH = {10 &#124; num}]</p>         </td>
         <td class="BodyD-Column1-Body2" width="45.371%"><p>Maximum number of intermediate authorities authorized for
the remote certificate.</p>
<p>This optional parameter has a numeric value between 0 and
10 (default value). 0 means that only self-signed certificates are accepted.
1 means that only certificates that are self-signed or signed by a recognized
root authority are accepted.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1"><p>DIRECT = CLIENT</p>         </td>
         <td class="BodyD-Column1-Body1" width="45.371%"><p>Security profile for the client mode.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2"><p>[DNISSUER = string1..512]</p>         </td>
         <td class="BodyD-Column1-Body2" width="45.371%"><p>List of values to be checked in the DN of the entity that directly issued the remote certificate.</p>
<p>Strings are limited to 512 bytes each. A check is
performed as follows:</p>
<ul>
<li>dnuser='C=FR/O=
Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this
string.</li>
<li>dnuser='C=UK,O=Axway'
means that the remote certificate must contain 'C=UK' string OR 'O=Axway'
string.</li>
</ul>
<p>Note that the different attributes of the dnuser or dnissuer
string are separated by the '/' character.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1"><p>[DNUSER = string1..512]</p>         </td>
         <td class="BodyD-Column1-Body1" width="45.371%"><p>List of values to be checked in the remote certificate DN.</p>
<p>Strings are limited to 512 bytes each. A check is
performed as follows:</p>
<ul>
<li>dnuser='C=FR/O=
Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this
string.</li>
<li>dnuser='C=UK,O=Axway'
means that the remote certificate must contain 'C=UK' string OR 'O=Axway'
string.</li>
</ul>
<p>Note that the different attributes of the dnuser or dnissuer
string are separated by the '/' character.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2"><p>ID = identifier</p>         </td>
         <td class="BodyD-Column1-Body2" width="45.371%"><p>Security profile identifier.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1"><p>[MODE = {REPLACE
&#124; CREATE &#124; DELETE}]</p>         </td>
         <td class="BodyD-Column1-Body1" width="45.371%"><p>Action for the command. For DELETE mode, the command is
deleted from the PARAMETERS database; only the ID and DIRECT parameters
are required.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2">[ORIGIN = string ]         </td>
         <td class="BodyD-Column1-Body2" width="45.371%">This parameter indicates the origin of an object.         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1"><p>PASSW</p>
<p>string</p>         </td>
         <td class="BodyD-Column1-Body1" width="45.371%"><p>The parameter is the PassPort entity password for a user's certificate (the password that corresponds with the USERCID). This parameter enables PassPort connectivity.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2">[PARM = string1..64]         </td>
         <td class="BodyD-Column1-Body2" width="45.371%"><p>Freeform parameter associated with the security profile.</p>
<p>This local data item is not used by the SSL protocol. It
can be reused as a symbolic variable in an end of transfer procedure (&amp;SSLPARM).
It is also passed to the end of transfer, directory or file exits.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1"><p>[ROOTCID = (identifier,
identifier, ...)]</p>         </td>
         <td class="BodyD-Column1-Body1" width="45.371%"><p>List of certificate authorities. This list can reference a maximum of 10 identifiers in the local certificate database.</p>
<p>In client mode, this list is used to check the server
certificate. Only certificates signed by one of the authorities in the
ROOTCID parameter are accepted.</p>
<blockquote>
<p><strong>Note</strong></p>
<p>To use more than 10 identifiers, you can refer to the PKIENTITY information.</p>
</blockquote>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2">[TRACE = number ]         </td>
         <td class="BodyD-Column1-Body2" width="45.371%">SSL trace level.         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1"><p>[USERCID = identifier]</p>         </td>
         <td class="BodyD-Column1-Body1" width="45.371%"><p>Local user certificate. Within the context of the Transfer
CFT integrated PKI, this identifier
refers to the identifier of a user certificate in the database.</p>
<p>This parameter is used to select a user certificate for
authentication by the server (if requested by the server). The first user
certificate found (signed by one of the authorities proposed by the server)
is used.</p>
<p>The &amp;USERID and &amp;PART symbolic variables are accepted.<br />
These variables are then substituted with the name of the transfer owner
(refer to the CFTAPPL, CFTSEND and CFTRECV commands) and the local identifier
of the transfer partner respectively.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2"><p>[VERSION = {TLSV1 &#124; SSLV3 &#124; TLSV1COMP &#124; SSLV3COMP}]</p>         </td>
         <td class="BodyD-Column1-Body2" width="45.371%"><p>Session version.</p>
<p>Transfer CFT supports:</p>
<ul>
<li>TLS version 1.2, 1.1, 1.0 (TLSV1 &#124; TLSV1COMP keyword)</li>
<li>SSL version 3.0 (SSLV3 &#124; SSLV3COMP keyword)</li>
</ul>
<p><strong>About sessions</strong></p>
<p>In Transfer CFT, the SSL/TLS session version is proposed by the client and negotiated with the server.</p>
<p><strong>Client mode</strong></p>
<p>DIRECT=CLIENT</p>
<p>In client mode, TLSV1COMP or SSLV3COMP sets the header length in the NSDU to enable compatibility with other products.</p>
<p><strong>Server mode</strong></p>
<p>DIRECT=SERVER</p>
<p>In server mode, the header length is automatically detected for all SSL versions (SSLV3, TLSV1, SSLV3COMP, TLSV1COMP).</p>
<p>Limitation: The header length policy in the protocol header is only available for the PeSIT (ANY) protocol.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1">VERIFY         </td>
         <td class="BodyD-Column1-Body1" width="45.371%"><p>Sets the authentication mode requirement.</p>
<ul>
<li>ENFORCED: Ensures client authentication with the server. The transfer fails if the server does not ask for the client certificate during the handshake.</li>
<li>OPTIONAL and REQUIRED: The same as NONE (enabling backward compatibility), but should not be used.</li>
<li>NONE: No authentication required.</li>
</ul>         </td>
      </tr>
   </tbody>
</table>

## Server mode

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

<table>
   <tbody>
      <tr>
         <td class="BodyE-Column1-Body1">Parameter         </td>
         <td class="BodyD-Column1-Body1">Description         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" width="28.19%"><p>ID = identifier</p>         </td>
         <td class="BodyD-Column1-Body2" width="47.156%"><p>Identifier of the security profile.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" width="28.19%"><p>[CERFNAME = string1..64]</p>         </td>
         <td class="BodyD-Column1-Body1" width="47.156%"><p>File name root used to record the remote user chain of
certificates.</p>
<p>If this parameter is set and the remote partner is authenticated,
the chain of certificates proposed by the partner is recorded in DER format
in a file, the root of which comprises the CERFNAME parameter value and
the unique name set by Transfer CFT. The full name of the file can be
accessed via the &amp;SSLCFNAM symbolic variable or catalog query APIs.
The file is systematically deleted when the corresponding catalog entry
is purged.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" width="28.19%"><p>CIPHLIST = {(num, num,
..)}</p>         </td>
         <td class="BodyD-Column1-Body2" width="47.156%"><p>List of the algorithms supported.</p>
<p>Each value defines three algorithms:</p>
<ul>
<li>Authentication
algorithm</li>
<li>Encryption
algorithm</li>
<li>Sealing
algorithm</li>
</ul>
<p>This list is compared with the list proposed by the client
in order of preference, for the purpose of determining the suite to be
negotiated.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" width="28.19%"><p>[DEPTH = {10
&#124; num}]</p>         </td>
         <td class="BodyD-Column1-Body1" width="47.156%"><p>Maximum number of intermediate authorities authorized for
the remote certificate.</p>
<p>This optional parameter has a numerical value between 0
and 10, the default value. 0 signifies that only self-signed certificates
are accepted. 1 signifies that only certificates that are self-signed
or signed by a recognized root authority are accepted.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" width="28.19%"><p>DIRECT = SERVER</p>         </td>
         <td class="BodyD-Column1-Body2" width="47.156%"><p>The security profile is applicable in the server mode.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" width="28.19%"><p>[DNISSUER = string1...512]</p>         </td>
         <td class="BodyD-Column1-Body1" width="47.156%"><p>Strings that are limited to 512 bytes each. A check is
performed as follows:</p>
<ul>
<li>dnuser='C=FR/O=
Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this
string.</li>
<li>dnuser='C=UK,O=Axway'
means that the remote certificate must contain 'C=UK' string OR 'O=Axway'
string.</li>
</ul>
<p>Note that the different attributes of the dnuser or dnissuer
string are separated by the '/' character.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" width="28.19%"><p>[DNUSER = string1..512]</p>         </td>
         <td class="BodyD-Column1-Body2" width="47.156%"><p>Strings that are limited to 512 bytes each. A check is
performed as follows:</p>
<ul>
<li>dnuser='C=FR/O=
Axway/ OU=MFT Demonstration', means the remote certificate DN must contain this
string.</li>
<li>dnuser='C=UK,O=Axway'
means that the remote certificate must contain 'C=UK' string OR 'O=Axway'
string.</li>
</ul>
<p>Note that the different attributes of the dnuser or dnissuer
string are separated by the '/' character.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" width="28.19%"><p>[MODE = {REPLACE &#124;
CREATE &#124; DELETE}]</p>         </td>
         <td class="BodyD-Column1-Body1" width="47.156%"><p>Action for the command. For DELETE mode, the command is
deleted from the PARAMETERS database; only the ID and DIRECT parameters
are required.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" width="28.19%">[ORIGIN = string ]         </td>
         <td class="BodyD-Column1-Body2" width="47.156%">This parameter indicates the origin of an object.         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" width="28.19%"><p>[PARM = string1..64]</p>         </td>
         <td class="BodyD-Column1-Body1" width="47.156%"><p>Freeform parameter associated with the security profile.</p>
<p>This local data item is not used by the SSL protocol. It
can be reused as a symbolic variable in an end of transfer procedure (&amp;SSLPARM).
It is also passed to the end of transfer, directory or file exits.</p>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" width="28.19%"><p>[ROOTCID = (identifier,
identifier, ...)]</p>         </td>
         <td class="BodyD-Column1-Body2" width="47.156%"><p>List of the certificate authorities. This list references
identifiers in the local certificate database.</p>
<p>This list has two functions:</p>
<ul>
<li>A user
certificate can be chosen for authentication by the client</li>
<li>If the
client must be authenticated, the list of authority DNs supported by the
server is supplied</li>
</ul>
<blockquote>
<p><strong>Note</strong></p>
<p>To use more than 10 identifiers, refer to the PKIENTITY information.</p>
</blockquote>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" width="28.19%">[TRACE = number ]         </td>
         <td class="BodyD-Column1-Body1" width="47.156%">SSL trace level.         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" width="28.19%"><p>[USERCID = identifier]</p>         </td>
         <td class="BodyD-Column1-Body2" width="47.156%"><p>Reference of a user certificate in the local certificate
database. The purpose of this identifier is to select a user certificate
for authentication by the client.</p>
<p>If the server
requires client authentication, it provides the client with the list of
authority DNs supported during the SSL protocol negotiation phase.</p>
<blockquote>
<p><strong>Note</strong></p>
<p>This parameter
is ignored when the CFTSSL command is used for additional controls in
the server mode (CFTSSL command indicated by a CFTPART command).</p>
</blockquote>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body1" width="28.19%"><p>VERIFY = {REQUIRED
&#124; OPTIONAL &#124; NONE}</p>         </td>
         <td class="BodyD-Column1-Body1" width="47.156%"><p>Sets the authentication mode requirement.</p>
<p>The VERIFY and USERCID parameters set the security profile
authentication mode.</p>
<blockquote>
<p><strong>Note</strong></p>
<p>With the SSL
protocol, the server determines whether the client must be authenticated.
If the server wants the client to be authenticated, then the server must
first be authenticated by the client. Due to the algorithms supported
by Transfer CFT (refer to the CIPHLIST parameter), the server must currently
be authenticated by an X.509 certificate.</p>
</blockquote>
<p>The following list describes the possible cases for a security
profile in server mode (the USERCID parameter is mandatory for
a server CFTSSL command).</p>
<ul>
<li>REQUIRED: The server and the client must be authenticated. (Default)</li>
<li>OPTIONAL: The server and the client must be authenticated. An invalid
certificate is tolerated by the server. </li>
<li>NONE : Only the server must be authenticated. </li>
</ul>         </td>
      </tr>
      <tr>
         <td class="BodyE-Column1-Body2" width="28.19%"><p>[VERSION = {TLSV1 &#124; SSLV3 &#124; TLSV1COMP &#124; SSLV3COMP}]</p>         </td>
         <td class="BodyD-Column1-Body2" width="47.156%"><p>SSL session version.</p>
<p><strong><strong>Client mode</strong></strong></p>
<p>In Client mode (DIRECT=CLIENT), TLSV1COMP or SSLV3COMP set the header length in NSDU to enable compatibility with other products.</p>
<p><strong><strong>Server mode</strong></strong></p>
<p>In server mode (DIRECT=SERVER) the header length is automatically detected for all SSL versions (SSLV3, TLSV1, SSLV3COMP, TLSV1COMP).</p>
<p>Limitation: The header length policy in the protocol header is only available for the PeSIT (ANY) protocol.</p>         </td>
      </tr>
   </tbody>
</table>

## Associate the security profile with the CFTPROT object

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
