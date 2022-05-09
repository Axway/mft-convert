{
    "title": "Programming interfaces",
    "linkTitle": "Build APIs and exits",
    "weight": "250"
}This section describes the Transfer CFT batch programming interfaces for Transfer CFT.

Transfer CFT APIs in a z/OS environment
---------------------------------------

Transfer CFT APIs are described in the sub-book [Using APIs](../../using_apis). This programming interface can be accessed in the Assembler, COBOL and C programming languages.

Transfer CFT APIs use the DLL mode exclusively, with samples located in the following libraries:

- cftv2.SAMPLEA for Assembler
- cftv2.SAMPLEC for C
- cftv2.SAMPLEO for COBOL

The programs are compiled with the main options:

- RENT, GOFF, LIST(133) for Assembler
- DLL, LONGNAME, RENT for C
- DLL, RENT for COBOL NODYNAM

To LINK-EDIT a program that uses Transfer CFT services, you must import references called by the Transfer CFT user. Additionally, you require a JOBLIB/STEPLIB containing the LOAD-MODULE CFTDAPI and CFTDSCP for execution.

The usable imports list is defined in member distlib.CNTL(LINKAPID).

 Programming examples
---------------------

Samples are located in the CFTV2.SAMPLEA, SAMPLEO, or SAMPLEC files.

- Assembler
    -   AAPIDLL: Uses CFTAI CFTAC
    -   AAPIUST: Selection area description ’macro’, catalog returned, and so on
- COBOL NODYNAM
    -   OAPIC: Uses CFTC
    -   OAPII: Uses CFTI
    -   OAPIW: Deposit a request (using CFTU) and wait for the result (CFTI)
    -   OAPIX: Uses cftaix
    -   OAPISYN: Deposit of requests (VIA synchronous API, or communication)
- Cobol Copybook
    -   OAPICST: Constants and common descriptions
    -   OAPIINF: Structure returned by function GETXINFO
    -   OAPIMSG: Structure of the message returned by cftau
- OAPIUST: Global Copy book
    -   OAPISL: Selection area description (for compatibility)
    -   OAPICA: catalog area description returned (for compatibility)
    -   OAPICX: catalog area description returned by cftaix API (for compatibility)
- OAPI24: Global Copy book - equivalent to OAPIUST (long fields)
    -   OAPISL4 : Selection area description (long fields)
    -   OAPICA4 : catalog area description returned (long fields)
    -   OAPICX4 : catalog area description returned by cftaix API (long fields)
- Cobol Copybook API 2
    -   OCFTAPI2

### COBOL API subroutines called in static mode

#### COBOL DYNAM

This section describes the special use of COBOL compiled with the DYNAM option.

The DYNAM compile option is incompatible with the DLL option. To be able to use the CFT APIs, an interface (called 'OAPIFC') is provided. You must only call this interface one time, and return function pointers corresponding to DLL API entry points.

****Implementation****

1. In the working storage section, add the copy book OCFTAPD2.
1. In the procedure division, call the module 'OAPIFC' once:

`if   (ipc-loaded = zero) then`

`call 'OAPIFC' using ipc-par end-call`

`end-if`

****Syntax to call CFT APIs****

`Call cftu using parameters…`

`Call cfti using parameters…`

`Call cftc using parameters…`

Etc.

In this case cftu, cfti, and cftc are defined as pointer functions (see OCFTAPD2).

> **Note**
>
> Note: If the pointers are not initialized by OAPIFC an abend 0C1 occurs.

- Delivered samples:  
    OAPIWS, OAPIIS, OAPICS, OPAIXS, OAPI2AS, OAPI2BS
- Compilation options:  
    CBL DYNAM,RENT,DATA(31),NODLL,PGMNAME(LONGMIXED)
- Link-edit model:  
    Example OAPIWS sample:  
    INCLUDE USER(OAPIWS)  
    MODE AMODE(31)  
    MODE RMODE(ANY)  
    NAME OAPIWS(R)

1. To run the sample OAPIWS , use INSTALL(I93APIRN) and replace:  
    //OAPIW EXEC CFTAPI,PROG=OAPIW,APIP='SEND PART=PARIS,IDF=TXT'  
    By  
    //OAPIWS EXEC CFTAPI,PROG=OAPIWS,APIP='SEND PART=PARIS,IDF=TXT'

#### C

- CAPIX: uses cftaix

<!-- -->

- CAPII: use cfti

<!-- -->

- CAPIW: Deposit of a request and wait for the result

<!-- -->

- CAPIC: Uses CFTAI CFTAC CFTAU

<!-- -->

- CAPISYN: Deposit of requests (VIA synchronous API, or communication file)

#### C Headers – API-C

- CFTAPI (CAPIUST): Selection area description, returned catalog area description, and so on

<!-- -->

- CFTD: Base API-C constants and type definitions 

<!-- -->

- XTYPE

#### C Headers – API2-C

- CFTAPI2 (CAPI2UST): Base2 API-C constants and type definitions.

The compile, link-edit, and run JCLs for these samples can be found in the CFTV2.INSTALL file and are called I91APICP, I92APILK and I93APIRN. The compile JOB must be customized to reflect your environment.

**Main delivered samples**


| Language  | Source file<br/> cftv2.SAMPLE* | Copy,<br/> Macro,<br/> Include used | API  | LINK EDIT command files (DLL) distlib.CNTL  | Load module  |
| --- | --- | --- | --- | --- | --- |
| COBOL NODYNAM  | OAPIC | OPAICST | CFTC | LINRDACO | OPAIC |
| - &quot; -  | OAPII | OAPIUST or OAPI24 | CFTI | LINRDAIO | OAPII |
| - &quot; -  | OAPIW | OAPIUST or OAPI24 | CFTI<br/> CFTU | LINRDAWO | OAPIW |
| - &quot; -  | OAPIX | OAPI24 + OAPICX4 or<br/> OAPIUST + AOPICX | cftaix | LINRDAXO | OAPIX |
| - &quot; -  | OAPI2A | OCFTAPI2 | API 2 | LINRDA2O | OAPI2A |
| - &quot; -  | OAPI2B | OCFTAPI2 | API 2 | LINRDA3O | OAPI2B |
| - &quot; -  | OAPISYN  | OAPICST OAPIINF OAPIMSG  | cftau or CFTU  | LINRDAYO  | OAPISYN  |
| **C** | CAPIC | CAPIUST | cftai<br/> cftau<br/> cftac | LINRDACC | CAPIC |
| - &quot; -  | CAPIW | CAPIUST | cftai<br/> cftau | LINRDAWC | CAPIW |
| - &quot; -  | CAPIX | CAPIUST | cftaix | LINRDAXC | CAPIX |
| - &quot; -  | CAPII | CAPIUST | cfti | LINRDAIC | CAPII |
| - &quot; -  | CAPI2A | CAPI2UST | API 2 | LINRDA2C | CAPI2A |
| - &quot; -  | CAPI2B | CAPI2UST | API 2 | LINRDA3C | CAPI2B |
| ASM  | AAPIDLL | - | cftai | LINKALE | AAPIDLL |
| - &quot; -  | AXPIDLL  |   | cftaix  | LINKALEX  | AXPIDLL  |

