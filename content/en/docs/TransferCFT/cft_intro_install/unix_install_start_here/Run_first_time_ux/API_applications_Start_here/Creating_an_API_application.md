{
    "title": "Creating  an API application",
    "linkTitle": "Creating an API application",
    "weight": "220"
}The example provided below was designed for the ****cft-tcp.conf**** configuration example located in *&lt;installdir&gt;/runtime/conf/*. For
this example, you should have already customized this file using the method described in [*Running
{{< TransferCFT/axwayvariablesComponentShortName  >}} for the first time*]().

Application components
----------------------

The *&lt;installdir&gt;/runtime/src/capi* subdirectory contains the:

- Sample source module,
    called *apixmp1.c*, which interacts with {{< TransferCFT/axwayvariablesComponentShortName  >}}. This program
    reads the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog and displays its contents in part or in
    full, depending on the restrictions set in the command line
- *makefile*
    compilation procedure, which uses the *apixmp1.c* sample source module
    to generate the APIXMPI executable file

The *&lt;installdir&gt;/lib* subdirectory contains the:

- *libcftapi.a*
    module required to use {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs

To generate the *APIXMP1* sample program, proceed as follows.

1. Access the *&lt;installdir&gt;/runtime/src/capi* directory.
1. Enter the command:   ****`make`****

Testing the configuration
-------------------------

To test the configuration, proceed as follows:

1. Access the *&lt;installdir&gt;/runtime/conf/* directory.
1. Generate the {{< TransferCFT/axwayvariablesComponentShortName  >}} internal datafiles
    using *cftinit* with one of the two proposed configuration files:

`     cftinit cft-tcp.conf`

1. When the *cftinit complete*
    message is displayed, run {{< TransferCFT/axwayvariablesComponentShortName  >}} using the command:

`     cft start`

1. When the *CFTMAIN process
    ID is xxxxx* message is displayed, perform one or more transfers:

`     CFTUTIL send part=BOSTON,idf=TXT`

1. Check that the transfers are
    complete:

`     cftcatab`

1. Run the sample program:

`     cd <installdir>/runtime/src/capi ; ./APIXMP1`

****Results****: The result should correspond to the catalog contents:

> `PART=NEW YORK, IDT=<dynamic identifier>,IDF=TXTPART=BOSTON ,IDT=<dynamic identifier>,IDF=TXTAPIXMP1 _ 2 record(s) found`

1. Stop {{< TransferCFT/axwayvariablesComponentShortName  >}}:

`     cft stop`
