---
title: "Using  the PKICER command"
linkTitle: "Using PKICER"
weight: 230
---Use the PKICER command to:

* Import,
    delete or update a root or intermediate certificate authority in the local
    database
* Import,
    delete or update a user certificate (with or without the associated private
    key) in the local database
* Import
    the private key associated with a user certificate

Users with several certificates signed by different authorities
can import all their certificates with the same identifier.

## Working with certificates

****Syntax****

```
PKICER
ID    
=     string,

[CHECK    
=     {YES &#124; NO},]
[COMMENT  
=     string,]
[IDATA     
=     string,]
[IFORM    
=     {PKCS12 &#124; DER &#124; PEM &#124; PKCS7,]
[IKDATA    
=     {base64/PEM},]
[IKFORM   
=     {DER &#124; PEM &#124; PKCS8},]
[IKNAME   
=     string,]
[IKPASSW  
=   string,]
[INAME    
=     string,]
[ITYPE    
=     {ALL &#124; USER &#124; ROOT &#124; INTER},]
[MODE    
=     {REPLACE &#124; CREATE &#124; DELETE},]
[STATE    
=     {ACT &#124; INACT},]
[ROOTCID  
=     string,]
[PKICER    = string,]

```

Some parameters are available only in command line, as indicated in the table.


| ID = string1..32 | Unique local identifier of the certificate to be created, replaced or deleted. |
| --- | --- |
| [CHECK = YES &#124; NO] | Certificate check during import: this option is only applicable for a user or intermediate authority certificate.<br/> A check is performed:<br/> • To establish whether the root or intermediate authority certificate exists in the local database<br/> • On the certificate signature<br/> • To determine whether the public key matches the private key (if the private key is to be imported)<br/> **Command line only** |
| [COMMENT = string1..64] | Comment associated with the certificate: for the CREATE or REPLACE operations only. |
| IDATA  | Source as base64 or PEM data instead of a file. If you use PKIUTIL IDATA, you cannot also use INAME, and vice versa.  |
| IKDATA  | Source as base64 or PEM data instead of a file. If you use PKIUTIL IKDATA, you cannot also use IKNAME, and vice versa. |
| [IFORM = PKCS12 &#124; DER &#124; PEM &#124; PKCS7] | Format of the certificate to be imported. It must be specified if the INAME parameter is set. |
| [IKFORM = PKCS8 &#124; DER &#124; PEM ] | Format of the private key to be imported. This parameter must be specified if the IKNAME parameter is set. |
| [IKNAME = string1..64] | File from which the private key, that is associated with a user certificate, to be imported or updated must be read.<br/> • This parameter is not significant if the certificate format is PKCS#12 as the certificate and private key are declared in the same source file.<br/> • If you call PKIUTIL with IKDATA, you cannot use IKNAME. |
| [IKPASSW = string1..64] | Source file protection password. This is the source file protection password, and must be specified for encrypted PEM (PKCS#5), PKCS#8 encrypted private key formats, PKCS#7, or for PKCS#12 certificate formats.<br/> There are two ways to specify the password:<br/> • By value: the value assigned to the parameter is used directly as a password<br/> • By reference to a file: the value assigned to the parameter is the name of a file, the first record of which contains the password; in this case, the file name must be preceded by a # or @ sign depending on the OS. On Windows, for example, IKPASSW=#myfile where the password is specified in the <code>myfile </code>file; the first file record must contain the password in plain format. |
| [INAME = string1..128] | Source file containing the certificate to be imported or updated.<br/> • This parameter is not allowed in DELETE mode.<br/> • If you call PKIUTIL with IDATA, you cannot use INAME. |
| [ITYPE = ALL &#124; USER &#124; ROOT &#124; INTER] | Type of certificate to be imported.<br/> This parameter is mandatory for the modes DELETE (deleting a certificate from the database) and REPLACE (updating a certificate).<br/> This field must be specified for an X.509 certificate, Version 1 or 2. The type of an X.509 version 3 certificate is determined automatically. For version 3, the ITYPE parameter is matched against the type detected:<br/> • ALL: certificate type not checked<br/> • USER: user certificate<br/> • ROOT: root authority certificate<br/> • INTER: intermediate authority certificate |
| [MODE = REPLACE &#124; CREATE &#124; DELETE] | Action on the certificate. The REPLACE action imports or updates an existing certificate in the database.<br/> If importing a certificate chain, user certificate and all intermediate authority certificates, all certificates are recorded in the local database. The user certificate is recorded with the identifier generated from the ID parameter. Intermediate authority certificates are recorded with internal identifiers in the local internal datafile and cannot be viewed. |
| [PASSWORD]  | If you use a password, the PKICER certificate is exported in PKCS#12 format (rather than KPRIV).  |
| [ROOTCID = string] | This parameter is mandatory for the DELETE (deletion of a certificate from the database) and REPLACE (update of a certificate) modes. It indicates the identifier of the authority of the certificate to be deleted or updated.<br/> This parameter must even be indicated for an authority certificate. In this case, the ID and ROOTCID parameters have the same value. |
| [STATE = ACT &#124; INACT] | Status of the imported certificate.<br/> By default, the certificate is active and can be used by Transfer CFT. |

