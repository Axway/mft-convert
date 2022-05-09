{
    "title": "Using Application programming interfaces (API)",
    "linkTitle": "Using APIs",
    "weight": "240"
}You perform Transfer CFT service calls differently depending on the programming language that you use (C or COBOL). For more information, refer to the programming topics in [About APIs](../../../../about_this_document_zos/using_apis)

The Transfer CFT service called (CFTI, CFTU or CFTC) executes the request, either with or without analyzing command syntax, and then initializes the response zone.

The client application receives from Transfer CFT:

- A return code

<!-- -->

- The requested data, if applicable

Creating an API application
---------------------------

The `cft-tcp.conf `configuration sample is located in` cft_scen `or` d$cft_run:[conf].`

Application components
----------------------

The `d$cft_run:[src.capi]` subdirectory contains:

- The sample source module,
    called `apixmp1.c,` which interacts with {{< TransferCFT/axwayvariablesComponentShortName  >}}. This program
    reads the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog and displays its contents in part or in
    full, depending on the restrictions set in the command line.
- The `makefile_capi.com`
    compilation procedure, which uses the` apixmp1.c `sample source module
    to generate the APIXMPI executable file.

Testing the configuration
-------------------------

To test the configuration:

1. Connect to the OpenVMS session with your Transfer CFT user.
1. Generate the {{< TransferCFT/axwayvariablesComponentShortName  >}} internal datafiles
    using `cftinit` with the configuration file:  
    cftinit cft_scen:cft-tcp.conf
1. When the` cftinit complete`
    message is displayed, run {{< TransferCFT/axwayvariablesComponentShortName  >}} using the command:  
    cft start
1. When the `CFTMAIN process   ID is xxxxx `message is displayed, perform a transfer:  
    CFTUTIL send part=BOSTON,idf=TXT
1. Check that the transfer is
    complete:  
    CFTUTIL LISTCAT
1. Run the sample program in the IFS environment:  
    set def d$cft_run
1. Run the sample program on NATIF environment:  
    run cft_exe:APIXMP1.exe  
    ****Results****  
    The result should correspond to the catalog contents:  
    PART=NEW YORK, IDT=&lt;dynamic identifier&gt;, IDF=TXT  
    PART=BOSTON, IDT=&lt;dynamic identifier&gt;, IDF=TXT  
    APIXMP1 _ 2 record(s) found
1. Stop {{< TransferCFT/axwayvariablesComponentShortName  >}}:  
    cft stop
