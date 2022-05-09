{
    "title": "Link the Transfer CFT image directory",
    "linkTitle": "Link Transfer CFT image directory",
    "weight": "250"
}The D$CFT_INST:[OPT]=&gt; XMKT_BDIR:[OPT] directory contains procedure used to link Transfer CFT.


| File  | Contents  |
| --- | --- |
| CFT_LINK.COM | Procedure used to link the {{< TransferCFT/axwayvariablesComponentShortName  >}} images.<br /> The installation performs the link operation, but this procedure can be used if the VMS release is changed, for example. |
| COP_LINK.COM | Procedure used to link the COP images.<br /> The installation performs the link operation, but this procedure can be used if the VMS release is changed, for example. |
| MJGBLCFT.COM | Cf: CFT GLOBALS SECTION |
| *.OPT | Option files used to build the {{< TransferCFT/axwayvariablesComponentShortName  >}} images and associated utilities. |


{{< TransferCFT/axwayvariablesComponentShortName  >}} image directory
--------------------------------------------------------------------------

{{< TransferCFT/axwayvariablesComponentShortName  >}} image directory.

The logical name CFT_EXE points to the D$CFT_INST:[bin]directory.

### Example {{< TransferCFT/axwayvariablesComponentShortName  >}} directory

This directory contains program samples in C that describe how to use {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs and Exits.

There are two logical names:

- CFT_EXIT, which points to D$CFT_RUN:[SRC] directory
- CFT_API, which points to D$CFT_RUN:[SRC] directory

This directory is comprised of two subdirectories, CAPI and EXIT, as described in the next sections.

The subdirectory D$CFT_RUN:[SRC.CAPI] provides the following Transfer CFT files and samples.


| File  | Contents  |
| --- | --- |
| API2XMP1.C  | The catalog API sample program listing all catalog content.  |
| API2XMP2.C  | The catalog API sample program, which changes all Terminated (X) transfers to Ended.  |
| APIXMP1.C  | C API LIST for the catalog samples.  |
| APIXMP2.C  | C API commands: Send, Recv, Halt samples.  |
| EXACCT.C  | C samples for the account file.  |
| IPCAI2.C  | C API samples.  |
| TCFTAIX.C  | IPC test program.  |
| TCFTSYN.C  | The communication and catalog API sample program. For example, you can use this API program to display the CATALOG.  |


The subdirectory D$CFT_RUN:[SRC.EXIT] provides the following Transfer CFT files and samples.


| File  | Contents  |
| --- | --- |
| EXAMLDAP.C;1  | Access Management exit with LDAP sample.  |
| EXAMRBAC.C;1  | Access Management exit sample.  |
| EXAMRBAC.H;1  | Header used for sample exit.  |
| EXAMSMP1.C;1  | Access Management exit sample.  |
| EXAXMP1.C;1  | DIRECTORY TYPE EXIT TASK example.  |
| EXEXMP1.C;1  | END-OF-TRANSFER TYPE EXIT TASK V240 example.  |
| EXFXMP1.C;1  | EXAMPLE OF FILE TYPE EXIT TASK V240, which writes a message in the Transfer CFT log file for each transfer stage.  |
| EXFXMP2.C;1  | EXAMPLE OF FILE TYPE EXIT TASK V240.  |


### Example {{< TransferCFT/axwayvariablesComponentShortName  >}} exit directory

Directory containing program samples in C that describe how to use {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs.

Logical name CFT_EXIT points to D$CFT:[CFT.RUNTIME.SRC] directory. There are two subdirectories : CAPI and EXIT.


| File  | Contents  |
| --- | --- |
| *.OPT | File containing the link options for the various samples provided. |
| CFTEXITL.COM | Procedure submitted by an exit and used to query the remote catalog. |
| EXFXMP2.C | Sample FILE EXIT program written in C. |
| EXAXMPM.C<br /> EXAXMPP.C | Samples of DIRECTORY EXIT programs written in C. |
| EXEXMP1.C | Sample END OF TRANSFER EXIT program written in C. |
| EXEUS.H | C include file required to develop an END OF TRANSFER EXIT program. |
| EXFUS.H | C include file required to develop a FILE EXIT program. |
| EXAUS.H | C include file required to develop a DIRECTORY EXIT program. |
| EXEUS. | Declaration file required to develop an END OF TRANSFER EXIT program in COBOL. |

