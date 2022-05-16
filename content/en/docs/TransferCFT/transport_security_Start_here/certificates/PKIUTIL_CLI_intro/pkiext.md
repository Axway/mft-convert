---

    title: Using PKIEXT
    linkTitle: Using PKIEXT
    weight: 300

---
You can use the PKIEXT command to extract the certificates and keys from the Transfer CFT PKI database. PKIEXT generates as an output the configuration commands used to reconstitute the PKI database, certificates, and keys in the KPRIV format (an internal Transfer CFT format).

Additionally, this page describes how to export an SSH public key for [SFTP](../../../../protocols_start_here/sftp_intro).

## Parameters

You can use a mix of the ID and TYPE parameters to create extraction filters. See the [examples](#Examples) below for details.

QQQ\_QQQ\_QQQ


| Parameter  | Description  |
| --- | --- |
| ID  | Identifier of either the certificate or key to be extracted.<br/> When this parameter is not defined, all of the certificates and keys are extracted pending if TYPE is defined. |
| BASE64  | Export base64 data:<br/> • Yes<br/> • No (default)<br/> When exporting data from the PKI database, you can request Base64 data instead of files. This way, additional files are not created. |
| FOUT | Name of the file where the command’s standard output is redirected.<br/> This generated file can then be interpreted directly by PKIUTIL.<br/> If this parameter is not defined, the standard output is displayed. |
| [ INUM = {number0...99} ]  | Internal number for the intermediate certificates in an imported chain of certificates (in the PKI database). You can use this option to select a specific intermediate certificate.  |
| PASSWORD  | The password length must be between 4 and 64 characters.<br/> • When using a password, PKIEXT exports a certificate/key pair in PKCS#12 format instead of DER (certificate) and KPRIV (key). &lt;/li&gt;<br/> • When using a password, PKIEXT exports a key in PKCS#8 format instead of KPRIV. &lt;/li&gt; |
| ROOTCID  | The certificate authority ID. See an example usage in <a href="../../../../c_intro_userinterfaces/command_summary/parameter_intro/rootcid">ROOTCID</a>.  |
| TYPE  | This parameter defines the certificate or key type to be extracted.<br/> Possible values:<br/> • ALL: extracts ROOT, INTER, and USER in PKICER, PKIKEY, and PKIENTITY<br/> • USER<br/> • ROOT<br/> • INTER<br/> • ENTITY<br/> • KEY<br/> • CERT: extracts ROOT, INTER, and USER in PKICER |


<span id="Examples"></span>

## Examples

This example extracts only the ROOT certificates of the PKI database.

```
PKIUTIL PKIEXT FOUT=PKI.EXT,TYPE=ROOT
```

This example extracts only the element of ID "MYKEY", regardless its type (PKIKEY, PKIENTITY...).

```
PKIUTIL PKIEXT FOUT=PKI.EXT,ID=MYKEY
```

This example extracts only the ROOT certificates with the ID "MYCERT" (this could be useful if multiple certificates have the same ID but are of a different TYPE).

```
PKIUTIL PKIEXT FOUT=PKI.EXT,TYPE=ROOT,ID=MYCERT
```

The following command exports <span class="code">`MY_CERT`</span>, which is an existing user certificate, in PKCS12 format by adding a password <span class="code">`Mypassword`</span>.

```
PKIUTIL PKIEXT ID=MY_CERT, FOUT=PKI.CONF, PASSWORD=Mypassword
```

After exporting the certificate, open the<span class="code">` PKI.CONF`</span> file, where the INAME is the name of the exported PKCS12 certificate (and the password equal to <span class="code">`Mypassword`</span>).

To use the Base64 option:

```
PKIUTIL PKIEXT FOUT=BAR.cmd, BASE64=YES
```

Which results in a single <span class="code">`BAR.cmd`</span> command file.

## Exporting an SSH public key for SFTP

When using SFTP, you can export the public key in an SSH\_RSA format to share with software other than Transfer CFT. To perform an extract, you must have originally imported the key with the same PKIPASSW as used in the CFTPARM object. If not, the export returns a key in KPRIV format instead of SSH\_RSA format (which is usable only by Transfer CFT).

## Importing and exporting keys

You can use PKIEXT to export keys from the local database. To perform an extract, you must use the same PKIPASSW (CFTPARM object) as was originally used to import the key. Using the same logic, to re-import a key that you extracted using PKIEXT, you require the same CFTPARM [PKIPASSW](../../../../c_intro_userinterfaces/command_summary/parameter_intro/pkipassw).

<span class="autonumber">**Problem**: </span>Due to native OS encoding (for example, ASCII on Linux and EBCDIC on z/OS), when you export a key to a different operating system the decode operation may fail even when both systems are using the same password.

<span class="autonumber">**Solution**: </span>Use the correct encoding and put the PKIPASSW in a file, for example, the ASCII string "<span class="code">`password`</span>" on an EBCDIC system. Then point the CFTPARM PKIPASSW to this file, for example<span class="code">` PKIPASSW=#|@/path/to/pkipass_file`</span>. The PKIPASSW is consequently read with the correct encoding, and the file is correctly deciphered.

> **Note**
>
> In earlier versions of Transfer CFT, the PKIPASSW parameter was used for encryption in multiple PKI commands. This functionality is now replaced by the UCONF crypto.key\_fname parameter.
