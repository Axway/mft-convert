{
    "title": "Setting up the TCP/IP layer ",
    "linkTitle": "Setting protocol parameters",
    "weight": "220"
}Before starting {{< TransferCFT/axwayvariablesComponentShortName  >}} with TCP/IP for the first time, you must setup your TCP/IP.

****Procedure****

1. Install and configure
    the TCP/IP layer.
1. In the SAMPLE folder, configure
    the PARMTCP.SMP sample file, with the ****key****
    parameter in CFTPARM.
1. In the SAMPLE folder, configure
    the sample file PARMTCP.SMP. Configure the ****nspart****
    and ****nrpart**** parameters in CFTPARM,
    and the ****host**** parameter of the
    CFTTCP command.
1. Start the batch file `..\CFT\SAMPLE\RESETTCP`.
1. Start CFTMAIN.
1. Check that CFTMAIN
    started correctly.
1. From a command prompt on a different Windows, and in the {{< TransferCFT/axwayvariablesComponentShortName  >}} root folder, enter the
    command: `> CFTUTIL SEND PART=PART1, IDF=TEST`
