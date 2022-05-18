---
title: "nspassw"
linkTitle: "nspassw"
weight: 2370
--- <span id="nspassw"></span>

### nspassw

#### CFTPART

******[NSPASSW =
*string*]******

*string32* SFTP

*string8* ODETTE, PeSIT E

The remote partner checks that the value received equals the value of
the NRPASSW parameter documented in the description of the corresponding
CFTPART.

This parameter value is case sensitive in CFTUTIL commands if you enclose the value in " " quotes.

> **Note**
>
> If you begin a password with an indirection character (Unix @, Windows #) when using SFTP, it is considered a reference to a file and not part of the password. Please see the SFTP pages for more information.

[Return to Command index](../../)
