---
    title: "mac"
    linkTitle: "mac"
    weight: 1870
---### mac

#### CFTFILE, PKIFILE, CFTINIT

****[ MAC= <u>YES</u> &#124; NO ]****

*Available on Unix, Windows, and HP NonStop*

The integrity control checks for corrupted records in the CFTPARM, CFTPART, CFTPKI, CFTUCONF, CFTCOM database files. If a record is corrupted, it is displayed in the trace `cft.out` file and it is no longer accessible. Deleting this record is the only available action.

##### Using CFTFILE and PKIFILE:

```
CFTUTIL CFTFILE TYPE=PARM&#124;PART&#124;COM, FNAME=<database_file>, MODE=CREATE, MAC=YES&#124;NO
PKIUTIL PKIFILE FNAME=<database_file>, MODE=CREATE, MAC=YES&#124;NO
```

Integrity control is activated by default (MAC=YES). To disable if for the CFTFILE and PKIFILE commands, set MAC=NO:

```
CFTUTIL CFTFILE TYPE=PARM&#124;PART&#124;COM, FNAME=<database_file>, MODE=CREATE, MAC=NO
PKIUTIL PKIFILE FNAME=<database_file>, MODE=CREATE, MAC=NO
```

> **Note**
>
> If the PARM, PART, and PKI data are saved in the same cftparm file, integrity control is configured using the first CFTFILE command. You cannot mix integrity control for PARM, PART, and PKI.

##### Using CFTINIT

You can implement integrity control while performing the `cftinit `command as follows:

```
cftinit [-check] [-mac=yes&#124;no] -c (-common) [filename] [filename] [...]
```

Or, in multi-node:

```
cftinit [-check] [-mac=yes&#124;no] -n (-node) <node_id>
```
