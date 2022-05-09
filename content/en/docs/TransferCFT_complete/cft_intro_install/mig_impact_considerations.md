{
    "title": "Migration or upgrade impact and considerations ",
    "linkTitle": "Migration/upgrade impact and considerations",
    "weight": "80"
}You should be aware of the following features or parameters, which were modified since their inception, and how these changes may impact your upgrade or migration.

Key element

Originating versions

Updated versions

Description

Discontinued PassPort PS

3.9 and lower

3.10 and higher

If you were using PassPort PS as your PKI type, we recommend the following steps prior to an upgrade or migration:

1. Execute `CFTUTIL uconfunset id=pki.type` to set the type to the local PKI database.

1. Export all Transfer CFT certificates and keys from PassPort PS.

1. Import these keys and certificates in the PKI database.

Deprecated Habilitation Access Management

3.8 and lower

3.9 and higher

 

z/OS case sensitive file names

3.8 and lower

3.9 and higher

File names on USS systems are case sensitive and no longer need to be enclosed in quotes (“”).

- Prior to version 3.9, you required quotation marks to ensure case sensitivity.
- As of version 3.9, case sensitivity is applied universally.

**Example**

An example regression, if the USS the path is defined as follows:


|   | USS path  | Send  | Consequence  |
| --- | --- | --- | --- |
| 3.8  | /DIR/FILE  | FNAME=&quot;/dir/file&quot;  | file found  |
| 3.9  | /DIR/FILE  | FNAME=&quot;/dir/file&quot;  | file not found  |
| 3.9  | /DIR/FILE  | FNAME=&quot;/DIR/FILE&quot;  | file found  |


API and Exits

all versions

to any version

You must recompile any API or Exit programs that are used by Transfer CFT.

Configuration cache feature

(cft.server.parm.cache_size)

Lower than 3.8

3.8 and higher

<span id="parmcache"></span>The default value is now 5000 instead of zero, making the cache feature active by default.

This means that updates no longer occur dynamically; you can execute `RECONFIG `type`=PARMCACHE `or wait for a cache timeout as defined in `cft.server.parm.cache_timeout (60 seconds).`

cft.listcat_compat = No

cft.state_compat = No

Lower than 3.8

3.8 and higher

Modified the default value for the `cft.listcat_compat `(lstcompat) and `  cft.state_compat     `(stacompat) parameters from YES to NO.

Amazon S3

Lower than 3.8

3.8 and higher

When using Amazon S3, the default setting FACTION=VERIFY is no longer ignored.

If you would like to continue to have the same behavior of overwriting the file, please use FACTION=DELETE. Note, though, that the file is not available during the transfer.

CFTUIPREF

Lower than 3.8

3.8 and higher

After an upgrade you may need to check user privileges for creating filters in the CFTUIPREF object.

Copilot Java applet

Lower than 3.8

3.8 and higher

The Copilot Java applet was removed from the product. Users are invited to use the Transfer CFT UI or Flow Manager for a graphical UI experience.

SQLite database

3.7 and lower

3.8 and higher

The CFTPARM object's PARTFNAM and PKIFNAME fields are obsolete for Windows, UNIX, and HP NonStop.

FTYPE=T

*Windows only*

3.2.4 to 3.7 without appropriate patch or SP\*

3.8 and higher

On Windows systems, note the following difference when FTYPE=T.

- For versions 3.2.4 to 3.7 without the patch, an empty line terminated by a 1A character is transmitted.
- Prior to 3.2.4 and for the versions with the SP or patch applied, an empty line terminated by a 1A character is not transmitted.

\*3.7 SP1 (patch), 3.3.2 SP8, 3.6 SP3, 3.8

Visual C++ Redistributable Package for Visual Studio 2019
&lt;/td&gt;

3.6 and lower

3.7 and higher

Transfer CFT on Windows requires the **Visual C++ Redistributable Package for Visual Studio 2019** for proper functioning. This provides the necessary library files (DLL) for Transfer CFT.

You must install `vcredist_x64.exe` prior to installing or upgrading Transfer CFT.

**Issue**

If you perform an upgrade without first installing the Redistributable package, the runtime is not imported and Transfer CFT will not operate correctly. The following information displays in the `<installdir>/install.log` file:

`Script stderr:`

`child killed: unknown signal`

` `

`Fail to import RUNTIME data.`

`Problem running post-install step. Installation may not complete correctly`

`Fail to import RUNTIME data.`

**Corrective action**

1. Install the Redistributable package.
1. From the `cmd `console, load the profile.
1. Import the runtime data by running the import command to complete the upgrade.
1. Check that the script executed correctly.

LISTPKI

3.6 and lower

3.7 and higher

To use the new LISTPKI format, copy the `dspcnf.xml` model file from `<installdir>/distrib/template/conf` to the `<runtimedir>/conf.`

CFTACCNT

3.5 and lower

3.6 and higher

Updated the documentation for the account file in v24 format. Please note the changes in field length as described in the CFTACCNT list.

SORTBY

3.5 and lower

3.6 and higher

Catalog records are no longer displayed by IDTU. To have the same display as in previous versions, use the SORTBY parameter as follows:  
`listcat sortby=idtu`

EBICS

3.5 and lower

3.6 and higher

Use the Axway EBICS client. Please refer to the [EBICS client documentation](https://docs.axway.com/bundle/EBICSClient_10_allOS_en_HTML5/page/ebics_client_documentation_home.html) for product details.

BUFSIZE

FBUFSIZE

3.3.2 and lower

*Unix and IBM i only*

3.4, 3.6 SP2 and lower, 3.7 and 3.8

A BUFSIZE or FBUFSIZE value greater than 32 kiB may lead to Transfer CFT failing to exchange messages between CFTTPRO and CFTTFIL.
If you have set a value higher than 32 kiB, please decrease it to 32768.

> **Note**
>
> Note: As of 3.6 SP3, 3.8 SP1, and 3.9, the internal value limit is 32768.

PKIFNAME

3.4 and lower

3.5 and higher

You can no longer reference a certificate with the PKIFNAME format (`CFTPARM:PKIFNAME=TXT://certificate`).

Previously, when implementing an integrated
PKI, the PKIFNAME parameter could indicate a flat-file database (`PKIFNAME=TXT://certificate`). If you were using this kind of file and then migrate, you must manually import all certificates into the PKI database.

CFTCRON

Lower than 3.4

3.4 and higher

An upgrade from a version lower than Transfer CFT 3.4 to 3.4 or higher may fail due to an incorrect time syntax because the CFTCRON time syntax is checked when creating or editing a CFTCRON object.

PKIPASSW

3.3.2 or lower

3.4 and higher

Removed the PKIPASSW parameter from PKI commands (still available for CFTPARM).

> **Note**
>
> Note: In earlier versions of Transfer CFT, the PKIPASSW parameter was used for encryption in the multiple PKI commands. This functionality is now replaced by the UCONF crypto.key_fname parameter.

****Impact****

If you are using PKIEXT to export keys during a manual migration, you must use the same PKIPASSW (CFTPARM object) as was originally used to import the key. Using the same logic, to re-import a key that you extracted using PKIEXT, you require the same CFTPARM [PKIPASSW](../../c_intro_userinterfaces/command_summary/parameter_intro/pkipassw).

For information on exporting keys, please refer to [Using PKIEXT](../../transport_security_start_here/certificates/pkiutil_cli_intro/pkiext).

32-bit releases

3.3.2 and lower

3.4 and higher

End of 32-bit version deliveries.

Some default values

3.3.2 and lower

3.4 and higher

Updated default values of the following parameters to optimize and standardize among platforms.

Object

Parameter

Old default

New default

**CFTPARM**

 

 

 

 

 

 

MAXTRANS

128 (Win), 256
(os400, unix, vms), 990 (z/OS)

256

MAXTASK

1 (Win), 16
(os400, unix, vms), 400 (z/OS)

8

TRANTASK

14 (z/OS), 16
(os400, unix, vms), 128 (win)

3

WAITTASK

1441

10

SSLMTASK

1 (Win), 16
(os400, unix, vms), 64 (z/OS)

8

SSLTTASK

14 (z/OS), 16
(os400, unix, vms), 128 (win)

3

SSLWTASK

1441

10

CFTNET

 

type

x25

TCP

maxcnx

32

384

CFTPROT type=PeSIT prof=ANY

concat

no

yes

multart

no

yes

segment

no

yes

rpacing

36

32767

spacing

36

32767

rrusize

4056

32750

srusize

4056

32750

disctc

90

60

disctd

120

10

disctr

45

45

discts

165

60

rchkw

2

3

schkw

2

3

rcomp

10

0

scomp

10

0

sserv

PESIT

GSIT

**CFTPROT type=ODETTE**

tcp

CFT

OFTP

**CFTTCP**

retryw

7

1

retryn

6

4

retrym

12

12

cnxinout

2

4

**Impact**

Check the use in your flows and modify according.

cft.server.processing_scripts_variables_blacklist

3.3.2 SP3 and lower

3.3.2 SP4 and higher

POSIX Regular Extended expression that defines forbidden characters.

TLS

3.2.x and higher

not applicable

When migrating to 3.2.x or higher, SSL transfers may fail with a DIAGP e105s86 or e75s89 when performing transfers with the versions listed below (with the error occurring on the remote {{< TransferCFT/suitevariablesTransferCFTName  >}}).

Affected versions:

- All 3.1.3 SP7 and lower
- All 3.0.1 SP3 and lower

On even older versions, we recommend setting the CFTPROT:CONCAT parameter to No.

CA certificate chains

3.1.3 and lower

3.2.2 and higher

In {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.1.3 and lower, you can perform a SSL transfer even if the certificate chain is not complete (not signed by a ROOT CA).

**Impact**

In {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.2.2 and higher, the certificate chain must be complete for a transfer to succeed.

For more information, see [Unknown CA leads to a failed certificate verification](../../troubleshoot_intro/admin_troubleshooting_server/troubleshoot_security#Unknown)

PKIPASSW

3.1.3 and lower

3.3.2 and higher

When upgrading from 3.1.3 to 3.3.2, first check that the PKIPASSW length value is not greater than 8 characters.

If the value is 8 or less, you can proceed with the upgrade.

If the PKIPASSW value in the CFTPARM command is greater than 8 characters, perform the steps in the solution below.

****Solution****

Prior to migration you  must truncate the password on the Transfer CFT 3.1.3:

1. Export the CFTPARM.  
    `CFTUTIL cftext type=parm, fout=file_parm.out`
1. Modify the PKIPASSW in the file. For example, if the old value was `PKIPASSW=12345678910`, replace it with `PKIPASSW=12345678.`
1. Reimport:  
    `CFTUTIL config type=input,fname=file_parm.out`
1. Continue the Transfer CFT 3.3.2 upgrade process.

Copilot client

3.1.3 or lower

3.2.2 and higher

The Copilot application changed from a Java applet to a Java Web Start program.

****Impact****

Copilot requires Java 7 or higher.

ROOTCID=NONE

3.1.3

3.2.2 and higher

Non authentication method was available in 3.1.3 and lower (anonymous TLS connection).

****Impact****

This support has been removed in {{< TransferCFT/suitevariablesTransferCFTName  >}} 3.2.2 and higher.
You must update the ROOTCID parameter.

TLS

3.1.3 or lower

3.2.2 and higher

To comply with security standards, as of Transfer CFT version 3.2.2 the use of the cipher suites 59, 60, and 61 is restricted to TLS 1.2 exclusively.

****Impact****

This means that if some of your partners use a version of Transfer CFT lower than 3.2.2 that does not support TLS 1.2, and you are using ciphers 59, 60 and 61, which requires TLS 1.2 in version 3.2.2 and higher, you must add another cipher in the cipher list and remove ciphers 59, 60, 61 from the partner's cipher list.

> **Note**
>
> Note: You do not have to remove ciphers 59, 60, 61 in the partner cipher list if you apply the Transfer CFT patch 3.0.1 SP11.

Symbolic variable prefix

*HP NonStop only*

2.3.2 and lower

3.3.2 and higher

The symbolic variable prefix has changed from the circumflex (^) to an ampersand (&).

****Example****

`FNAME=^NFNAME` becomes `FNAME=&NFNAME`

Rotate the log

3.0.1 or lower

3.1.3 and higher

Changed the switch log feature behavior.

In version 3.0.1 or lower, there were two files that automatically alternated.

****Impact****

In version 3.1.3 and higher if you want to continue this functionality, you must set the alternate log file's uconf value `cft.cftlog.afname` to the alternate file path (for example, `$CFTRUNTIME/log/cftloga`).

Demo certificates

3.0.1 or lower

3.1.2 and higher

Axway no longer delivers the template certificates used in the Transfer CFT SSL.

****Impact****

If you were using the demo certificates, import your proper certificates and replace in the PKI database as the Demo certificates are expired.

CFTPARM 

key parameter

2.7.1 or lower

3.0.1 and higher

If you had the CFTPARM key parameter set directly to a value, you must modify this so that key parameter points to an indirection file containing the license key.
