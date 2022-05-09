{
    "title": "Exits in Transfer CFT z/OS",
    "linkTitle": "Exits in Transfer CFT z/OS",
    "weight": "270"
}When running under {{< TransferCFT/PrimaryCGorUM  >}}, you can use but not manage your {{< TransferCFT/suitevariablesTransferCFTName  >}} exits. To manage exits, use the information to manually configure as described in this section.

 Using the DLL mode for Transfer CFT exits 
-------------------------------------------

Transfer CFT exits that are written in COBOL, C language, or Assembler must be used in DLL mode. The advantages of this mode are:

- You do not need to recreate the LINK-EDIT when you change Transfer CFT versions
- You can mix using Assembler COBOL and C programs
- All Transfer CFT API are supported
- The ETEBAC3 protocol is obsolete, so samples for the ETEBAC3 exits are no longer available

Create exits in Assembler
-------------------------

To create exits with Assembler, access the following files:

- cftv2.SAMPLEA(AEX\*DLL):

    Samples for the ACCESS (AEXADLL), CATALOG (AEXEDLL), or FILE (AEXFDLL) are provided, and contain the steps required for a Transfer CFT EXIT:

> -   The main entry point, called ‘callexig\*’, is exported and is called by the Transfer CFT EXIT loader.
> -   The init function, the name is free, and samples are using EXEAINI, EXEINI, and EXFINI names. The init function returns the Transfer CFT EXIT run function address.
> -   The run function, the name is free, and samples use EXAXMP1, EXEXMP1, and EXFXMP1 names.
> -   Exits must be compiled with the GOFF assembler option.
> -   An example of the call to Transfer CFT APIs within an exit is provided in the AEXEDLL sample.

- distlib.MAC (AEX\*UST):
    -   Macro containing the DSECTs of the exchange areas with the Transfer CFT. Macros AEXAUST, AEXEUST, and AEXFUST are provided. Be sure to use the same value for VERSION= in the CFTEXIT as in the macro expansion.
- distlib.CNTL(LINKEXDL):
    -   Imports functions for a LINK-EDIT DLL.
- distlib.CNTL (LINRDXGn):
    -   Transfer CFT modules for various Transfer CFT exits. The value for ‘n’ may vary from 0 to 9. Use the naming conventions in the following table.

 


| Main exported EPA  | DLL name  | CFTEXIT PROG=value  |
| --- | --- | --- |
| calllexig0  | CFTDXG0  | CFTEXIG0  |
| callexig1  | CFTDXG1  | CFTEXIG1  |
| And up to callexig9  | And up to CFTDXG9  | up to CFTEXIG9  |


- Exits must be linked with CALL,REUS=RENT,DYNAM=DLL,CASE=MIXED options.
- Only the customer code is linked, and the Transfer CFT interfaces imported. For example:

`*  CFT/MVS file EXIT - DLL versionSETOPT  PARM(CALL,REUS=RENT,DYNAM=DLL,CASE=MIXED)* Customer codeINCLUDE   USER(AEXFDLL) ** the exit* Import EXIT support codeIMPORT    CODE,CFTDMAI,'exfrun1'MODE       AMODE(31)MODE       RMODE(ANY)NAME       CFTDXG5(R)`


| File  | Definition  |
| --- | --- |
| AEX*DLL | Sample program in Assembler. This sample provides the 3 steps needed in a Transfer CFT exit. |
| AEX*UST | Macro containing the DSECTs of the exchange areas with the Transfer CFT. |


Create exits in C
-----------------

To create exits with C, access the following files:

- cftv2.SAMPLEC(CEX\*DLL):

    Samples for the ACCESS (CEXADLL), Beginning-of-Transfer(CEXBDLL), CATALOG (CEXEDLL), or FILE (CEXFDLL) are provided, and contain the steps required for a Transfer CFT EXIT:

    -   The main entry point, ‘`callexig*`’, is exported and is called by the Transfer CFT EXIT loader.
    -   The `init `function, where the name is free, and samples use the EXAINI, EXEINI, EXFINI and EXBINI names. The `init `function returns the Transfer CFT EXIT run function address.
    -   The `run `function, where the name is free, and samples use the EXAXMP1, EXEXMP1, EXFXMP1, and EXBXMP1 names.
    -   Exits must be compiled with the options: LANG(EXTENDED), RENT, DLL, LONGNAME.
    -   An example of a call to Transfer CFT APIs within an exit is provided in the CEXEDLL sample.

- distlib.H (\*xeus)
- distlib.CNTL(LINKEXDL):
    -   Sample import functions for a LINK-EDIT DLL
- distlib.CNTL (LINRDXGn):
    -   Transfer CFT modules for various Transfer CFT exits. The value for ‘n’ may vary from 0 to 9. Use the naming conventions in the following table.


| Main exported EPA  | DLL name  | CFTEXIT PROG=value  |
| --- | --- | --- |
| calllexig0  | CFTDXG0  | CFTEXIG0  |
| callexig1  | CFTDXG1  | CFTEXIG1  |
| And up to callexig9  | And up to CFTDXG9  | Up to CFTEXIG9  |


- Exits must be linked with CALL, REUS=RENT, DYNAM=DLL, CASE=MIXED options.
- Only the customer code is linked, and the Transfer CFT interface imported. For example:

**`* CFT/zos exit TYPE=FILE- DLL version`**  
**`SETOPT PARM(CALL,REUS=RENT,DYNAM=DLL,CASE=MIXED)`**  
**`* Customer code`**  
**`INCLUDE USER(CEXFDLL) ** the exit`**  
**`* Import EXIT support code`**  
**`IMPORT CODE,CFTDMAI,'exfrun1'`**  
**`MODE AMODE(31)`**  
**`MODE RMODE(ANY)`**  
**`NAME CFTDXG5(R)`**


| File  | Definition  |
| --- | --- |
| CEX*DLL  | Sample program in C. This sample provides the 3 steps needed in a Transfer CFT exit.  |
| *XEUS  | The header containing the structure of the exchange areas with the Transfer CFT.  |


Create exits in COBOL
---------------------

To create exits with Cobol, access the following files:

- cftv2.SAMPLEO(OEX\*DLL):

    Samples for the ACCESS (OEXADLL), CATALOG (OEXEDLL), or FILE (OEXFDLL) are provided, and contain the steps required for a Transfer CFT EXIT:

    -   The main entry point, called ‘callexig\*’, is exported and is called by the Transfer CFT EXIT loader.
    -   The init function, the name is free, and samples are using EXAINI, EXEINI, and EXFINI names. The init function returns the Transfer CFT EXIT run function address.
    -   The run function, the name is free, and samples use EXAXMP1, EXEXMP1, and EXFXMP1 names.
    -   Exits must be compiled with options:

        NODYNAM,RENT,LIB,DLL,OBJECT,APOST,DATA(31)

        PGMNAME(LONGMIXED),EXPORTALL

    -   An example of the call to Transfer CFT APIs within an exit is provided in the OEXEDLL sample.

- distlib.COPY(OEX\*UST): for exit format V23
- distlib.COPY(OEX\*240): for exit format V24

<!-- -->

- distlib.CNTL(LINKEXDL):
    -   sample Imports functions for a LINK-EDIT DLL
- distlib.CNTL (LINRDXGn):
    -   Transfer CFT modules for various Transfer CFT exits. The value for ‘n’ may vary from 0 to 9. Use the naming conventions in the following table.


| Main exported EPA  | DLL name  | CFTEXIT PROG=value  |
| --- | --- | --- |
| calllexig0  | CFTDXG0  | CFTEXIG0  |
| callexig1  | CFTDXG1  | CFTEXIG1  |
| And up to callexig9  | And up to CFTDXG9  | Up to CFTEXIG9  |


- Exits must be linked with CALL,REUS=RENT,DYNAM=DLL,CASE=MIXED options.
- Only the customer code is linked, and the Transfer CFT interfaces imported. For example:

`* CFT/zos exit TYPE=FILE- DLL versionSETOPT PARM(CALL,REUS=RENT,DYNAM=DLL,CASE=MIXED)* Customer codeINCLUDE USER(OEXFDLL) ** the exit* Import EXIT support codeIMPORT CODE,CFTDMAI,'exfrun1'MODE AMODE(31)MODE RMODE(ANY)NAME CFTDXG5(R)`


| File  | Definition  |
| --- | --- |
| OEX*DLL  | Sample program in C. This sample provides the 3 steps needed in a Transfer CFT exit.  |
| OEX*UST  | Copy book containing the structure of the exchange areas with the Transfer CFT (format V23).  |
| OEX*240  | Copy book containing the structure of the exchange areas with the Transfer CFT (format V24).  |


Call APIs in exits
------------------

Calls to APIs, for catalog query and request deposits to Transfer CFT, are supported in exits. It is recommended that you use DLL support.

Calls to a Transfer CFT synchronous API are only supported in DLL.

**Exit examples summary for COBOL**

Source file

cftv2.

SAMPLEx

Jcl compilation

cftv2.

INSTALL

Command file

for link-edit

distlib.CNTL

Jcl for link-edit cftv2.

INSTALL

DLL name

OEXADLL

I91APICP

LINRDXG6

LINKEXLE

CFTDXG6

OEXEDLL

I91APICP

LINRDXG7

LINKEXLE

CFTDXG7

OEXFDLL

I91APICP

LINRDXG8

LINKEXLE

CFTDXG8

**Exit examples summary for C language**

Source file

cftv2.

SAMPLEx

Jcl compilation

cftv2.

INSTALL

Command file

for link-edit

distlib.CNTL

Jcl for link-edit cftv2.

INSTALL

DLL name

CEXADLL

I91APICP

LINRDXG0

LINKEXLE

CFTDXG0

CEXEDLL

I91APICP

LINRDXG1

LINKEXLE

CFTDXG1

CEXFDLL

I91APICP

LINRDXG2

LINKEXLE

CFTDXG2

CEXBDLL

I91APICP

LINRDXG9

LINKEXLE

CFTDXG9

**Exit examples summary for Assembler**

Source file

cftv2.

SAMPLEx

Jcl compilation

cftv2.

INSTALL

Command file

for link-edit

distlib.CNTL

Jcl for link-edit cftv2.

INSTALL

DLL name

AEXADLL

I91APICP

LINRDXG4

LINKEXLE

CFTDXG4

AEXEDLL

I91APICP

LINRDXG3

LINKEXLE

CFTDXG3

AEXFDLL

I91APICP

LINRDXG5

LINKEXLE

CFTDXG5
