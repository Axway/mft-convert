{
    "title": "pkipassw",
    "linkTitle": "pkipassw",
    "weight": "2630"
}<span id="pkipassw"></span>

### pkipassw

#### CFTPARM

****[PKIPASSW = string 1...64]****

The encryption password of the private key in the local certificate
base. There are two ways to specify the password, by either:

- Value:
    The value assigned to the parameter is used directly as a password.
- Reference
    to a file: The value assigned to the parameter is the name of a file,
    the first record of which contains the password. As such, the file name
    must be prefixed with an \#&#124;@, PKIPASSW=\#myfile for example, where the password
    is specified in the **myfile** file.
    The first file record must contain the password in plain format.

The password is not recorded in the local
certificate database. It is strongly recommended that you use the same
password for all private keys. The same password must be declared in the
configuration, so that {{< TransferCFT/axwayvariablesComponentLongName  >}} can access the private keys.

**Problem**: Due to native OS encoding (for example, ASCII on Linux and EBCDIC on z/OS), when you export a key to a different operating system the decode operation may fail even when both systems are using the same password.

**Solution**: Use the correct encoding and put the PKIPASSW in a file, for example, the ASCII string "`password`" on an EBCDIC system. Then point the CFTPARM PKIPASSW to this file, for example` PKIPASSW=#&#124;@/path/to/pkipass_file`. The PKIPASSW is consequently read with the correct encoding, and the file is correctly deciphered.

[Return to Command index](../../)
