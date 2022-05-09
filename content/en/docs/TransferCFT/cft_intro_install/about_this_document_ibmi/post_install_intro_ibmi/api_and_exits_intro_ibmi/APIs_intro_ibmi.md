{
    "title": "Using Application programming interfaces (API)",
    "linkTitle": "Using APIs",
    "weight": "280"
}> **Note**
>
> Note: Only ILE (Integrated Language Environment) is supported.

You perform Transfer CFT service calls differently depending on the programming language that you use (C, COBOL, or RPG). For more information, refer to the programming topics in [About APIs](../../../../about_this_document_zos/using_apis)

The Transfer CFT service called (CFTI, CFTU or CFTC) executes the request, either with or without analyzing command syntax, and then initializes the response zone.

The client application receives from Transfer CFT:

- A return code

<!-- -->

- The requested data, if applicable

Prerequisites
-------------

You require the ICC and GMAKE utilities for IBM i platforms:

- gmake: GNU compiler (make)
- icc: Intel C/C++ compiler (calls ILE)

Call from a COBOL/ILE or RPG/ILE program
----------------------------------------

### Querying the Catalog: CFTI function

Available commands include OPEN, SELECT, NEXT, MODIFY and CLOSE.

### Requesting a transfer: CFTU and CFTC functions

Available parameters include: F-SEND, F-RECV, F-START, F-HALT, F-KEEP, F-DELETE, F-END and F-COM.

****COBOL/ILE Programming samples****

All COBOL sample files are available in CFTPGM/CFTSRC. However, you should not directly modify them as this library is proprietary to Axway, and updating the product may modify these samples. Therefore, we recommend that you copy the samples you need into CFTPROD/UTIN and configure them according to your production requirements.

> **Note**
>
> Note: The product installation copies and configures some samples.

****RPG/ILE programming examples****

Refer to the programming examples, RPG COPY clauses, and procedures, which are supplied in the Transfer CFT library CFTPGM/CFTSRC (CPYRPGCILE, CPYRPGIILE, CPYRPGCIL4, CPYRPGIIL4):

- TCFTI2_RPG (API structure V24)
- TCFTU_RP1 (API asynchronous)
- TCFTU_RP2 (API synchronous)
- I_TCFTI_RP (executes CFTI or CFTIX functions)
- I_TCFTU_RP (executes CFTU function), etc.

Creating an API application
---------------------------

The `TCPPARAM` configuration sample is located in `CFTPROD/UTIN`.

Application components
----------------------

The `<installdir>/runtime/src/capi `subdirectory contains the:

- Sample source module,
    called `apixmp1.c,` which interacts with {{< TransferCFT/axwayvariablesComponentShortName  >}}. This program
    reads the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog and displays its contents in part or in
    full, depending on the restrictions set in the command line.
- `makefile`
    compilation procedure, which uses the` apixmp1.c `sample source module
    to generate the APIXMPI executable file.

The `CFTPGM `library subdirectory contains the `libapisrv1.srvpgm`
module required to use {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs.

Generating the application
--------------------------

To generate the *`APIXMP1`* sample program:

1. Access the `<installdir>/runtime/src/capi `directory.
1. Enter the command:  `gmake`

Testing the configuration
-------------------------

To test the configuration:

1. Connect to the IBM session with your Transfer CFT user.
1. Generate the {{< TransferCFT/axwayvariablesComponentShortName  >}} internal datafiles
    using `cftinit` with the configuration file:  
    CALL PGM(CFTINIT) PARM('CFTPROD/UTIN(TCPPARAM)')
1. When the` cftinit complete`
    message is displayed, run {{< TransferCFT/axwayvariablesComponentShortName  >}} using the command:  
    cftstart
1. When the `CFTMAIN process   ID is xxxxx `message is displayed, perform a transfer:  
    CALL PGM(CFTUTIL) PARM(SEND 'part=boston,idf=txt')
1. Check that the transfer is
    complete:  
    CALL PGM(CFTUTIL) PARM(LISTCAT)
1. Run the sample program in the IFS environment:  
    cd &lt;installdir&gt;/runtime; . ./profile; APIXMP1
1. Run the sample program on NATIF environment:  
    CALL PGM(APIXMP1)

****Results****

The result should correspond to the catalog contents:

> `PART=NEW YORK, IDT=<dynamic identifier>,IDF=TXTPART=BOSTON ,IDT=<dynamic identifier>,IDF=TXTAPIXMP1 _ 2 record(s) found`

Stop {{< TransferCFT/axwayvariablesComponentShortName  >}}:

`     cftstop`
