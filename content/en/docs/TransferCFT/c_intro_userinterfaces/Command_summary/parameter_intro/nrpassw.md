---
    title: "nrpassw"
    linkTitle: "nrpassw"
    weight: 2320
---<span id="nrpassw"></span>

### nrpassw

#### CFTPART

**[NRPASSW = string]**

*string32*     SFTP

*string8*     ODETTE, PeSIT E

Partner sign-on password, authorizing a local site access right check.

The remote partner must submit this password to the local {{< TransferCFT/axwayvariablesComponentShortName  >}}, during the connection phase. On the remote partner end, you must declare this
password as the NSPASSW parameter of the CFTPART object
used in the transfer.

This parameter value is case sensitive in CFTUTIL commands if you enclose the value in " " quotes.

> **Note**
>
> If you begin a password with an indirection character (Unix @, Windows #) when using SFTP, it is considered a reference to a file and not part of the password. Please see the SFTP pages for more information.

 

[Return to Command index](../../)
