{
    "title": "Creating  a directory exit - IBM i",
    "linkTitle": "Creating a directory exit",
    "weight": "300"
}This page describes a delivered sample that is designed to create an example directory exit. You can use the `TCPPARAM `configuration sample that is located in `CFTPROD/UTIN`.

****Before you start****

Before running the directory exit test, you must make a few changes to the `(TCPPARAM)` file depending on the type of network used. Edit the file using
your text editor (EDTF for example) as follows.

1. In the file, locate
    the *`cftprot`* command. The following lines are displayed:
1. Delete the comments (delimited
    by /\* at the beginning and \*/ at the end). The *`cftprot`* command should resemble the following:
1. Locate the *`cftexit`* command,
    commented as follows:
1. Locate the communication properties
    for your site, which appear at the end of the file. When
    modifying the (`TCPPARAM`) file, you must also find every occurrence
    of the HOST string located in cfttcp-type commands and replace the X character
    strings with your system name or address.

Prerequisites
-------------

You require the ICC and GMAKE utilities for IBM i platforms:

- gmake: GNU compiler (make)
- icc: Intel C/C++ compiler (calls ILE)

Application components
----------------------

The *`<installdir>/runtime/src/exit/`* subdirectory contains several sample source
modules. This program is used to check the following:

- Activation
    of a transfer to a partner known to the directory EXIT but not to Transfer
    CFT
- Activation
    of a transfer to a partner not known to either {{< TransferCFT/axwayvariablesComponentShortName  >}} or the directory
    EXIT

The *`<installdir>/lib`* subdirectory contains:

- The` libcftexa.a`
    module required to use the {{< TransferCFT/axwayvariablesComponentShortName  >}} directory EXITs

Generating the exit
-------------------

To generate the sample CFTEXITA application:

1. Access the `<installdir>/runtime/src/exit/ `directory.
1. Enter the command: `gmake`

Running the test
----------------

1. Access the `<installdir>/runtime/conf `directory.
1. Generate the {{< TransferCFT/axwayvariablesComponentShortName  >}} databases
    using `cftinit` the configuration file provided
    and modified for this EXIT:` cft-tcp.conf`
1. When the` cftinit complete `
    message is displayed, run {{< TransferCFT/axwayvariablesComponentShortName  >}} using the `cftstart `utility: `cftstart`
1. When the` CFTMAIN process   ID is xxxxx` message is displayed, perform an initial standard transfer
    using the command:  
    `CFTUTIL send part=BOSTON, idf=TXT`
1. Now submit a second transfer
    to the `NCFT_OK` partner:  
    `CFTUTIL send part=NCFT_OK,idf=TXT`
1. After a few seconds, you can
    check the transfer state by entering the command: `cftcatab`
1. The transfer is successful
    because NRPART01 is defined in the DIRECTORY EXIT as being the EXTPTN01
    non- {{< TransferCFT/axwayvariablesComponentShortName  >}} partner.
1. Now submit a third transfer
    to the NCFT_OK partner:  
    `CFTUTIL send part=NCFT_NOK,idf=TXT`
1. After a few seconds, you can
    check the transfer state by entering the command: `cftcatab`

The transfer fails because the password is invalid, even though NRPART02
is defined in the DIRECTORY EXIT.

1. Stop {{< TransferCFT/axwayvariablesComponentShortName  >}}: `cftstop`
