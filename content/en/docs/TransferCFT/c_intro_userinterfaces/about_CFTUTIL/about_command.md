---
    "title": "Display product information",
    "linkTitle": "ABOUT - Displaying computer characteristics",
    "weight": "190"
---
Use the ABOUT command
---------------------

Use the ABOUT command to display
the Transfer CFT product, host, and key information. This command displays the characteristics of the platform
on which Transfer CFT is installed.

******Syntax******

{{% TransferCFT/snippets/about%}}

****Parameters****


| Parameter  | Description  |
| --- | --- |
| [COMMENT](../../command_summary/parameter_intro/comment) | Free comment.<br/> This comment is displayed and can be used to indicate a specific item of information, such as the customer name.<br/> This information is then used to determine a software license key. |
| [TYPE](../../command_summary/parameter_intro/type)  | Displays the Transfer CFT product, host, and key information.  |
| [KEY](../../command_summary/parameter_intro/key)  | Defines the number of keys that display.  |


****Example****

This command displays the following type of information:

```
CFTU20I
CFTU20I CFT Windows
CFTU20I Version 3.3.2 20140423
CFTU20I (C) Copyright AXWAY 1989-2018
CFTU20I ====> Starting Session on 28/04/2014 Time is 18:54:31
CFTU20I Parameters file :C:\\AxwayCFT332\\Transfer_CFT\\runtime\\data\\cftparm
CFTU20I Partners file :C:\\AxwayCFT332\\Transfer_CFT\\runtime\\data\\cftpart
CFTU20I Catalog file :C:\\AxwayCFT332\\Transfer_CFT\\runtime\\data\\cftcata
CFTU20I
CFT information :
\* product = CFT Windows
\* version = 3.3.2
\* level = SP1
\* upgrade = 7595
\* target = win-x86-64
Host information :
\* model =
\* hostname = MACH-A10229
\* sysname = Windows
\* machine = AMT_X8664
\* version = 6.1.7601
\* release = Seven Service Pack 1
\* distrib =
Axway information :
\* product = Amplify
Transfer CFT
\* version = 3.3.2_SP1.0
\* applied-patches =
\* forbidden-patches =
Key information :
\* idparm = IDPARM0
\* key = Lxxxxxxxxxxxxxxxxxxxxxxxxxx588S
\* CI97S
\* type = DATE
\* expire = 2018/04/14
\* sysname = win-x86-64
\* Nb Transfers = 999
\* Nb CPU = 2
\* Nb Partners = Max
\* In/Out Bandwidth = Unlimited
\* In/Out Transfer activation = Unlimited
\* Edition =
\* Options = BWP CLU FIP ACC ODT CLP CLP XSR SSL SSL
\* XTF WBS
CFTU00I ABOUT _ Correct ()
```
<span id="CFTTELL"></span>

Use the cfttell program
-----------------------

UNIX and Windows only

This executable file retrieves system information, for example information needed to request a key. To use `cfttell`:

- Navigate to the` <CFTDIRINSTALL>/bin` directory
- Run cfttell

Options:

- -l, -L each bit of information is displayed on a new line, this is the default option
- -s, -S values are listed on a single line, separated by spaces using the format key=value, this option is valid for target, version, and uid.
- -h display this help

Keys:

- TARGET: lists the target platform
- VERSION: returns the Transfer CFT version if Transfer CFT is installed
- HOSTINFO: lists the OS, Transfer CFT version, and related host and machine details
- UID: Generates a unique id.
- HOSTINFO: Display information used in key generation.

****Examples****

```
C:\\projects> cfttell target
win-x86-64
```

 

```
C:\\projects> cfttell version
3000
```

 

```
C:\\projects> cfttell hostinfo
CFT version : 3010
Target : win-x86-32
Processor architecture : x64
Processor type : Intel/AMD X8664
Processor ID :
Number of processors : 2
OS release : Windows Seven Service Pack 1
OS version : 6.1.7601
Host name : ITEM-12345
```
