---

    title: Creating  a directory exit - IBM i
    linkTitle: Creating a directory exit
    weight: 300

---
This page describes a delivered sample that is designed to create an example directory exit. You can use the <span class="code">`TCPPARAM `</span>configuration sample that is located in <span class="code">`CFTPROD/UTIN`</span>.

****Before you start****

Before running the directory exit test, you must make a few changes to the <span class="code">`(TCPPARAM)`</span> file depending on the type of network used. Edit the file using
your text editor (EDTF for example) as follows.

1. In the file, locate
    the *`cftprot`* command. The following lines are displayed:
1. Delete the comments (delimited
    by /\* at the beginning and \*/ at the end). The *`cftprot`* command should resemble the following:
1. Locate the *`cftexit`* command,
    commented as follows:
1. Locate the communication properties
    for your site, which appear at the end of the file. When
    modifying the (<span class="code">`TCPPARAM`</span>) file, you must also find every occurrence
    of the HOST string located in cfttcp-type commands and replace the X character
    strings with your system name or address.

## Prerequisites

You require the ICC and GMAKE utilities for IBM i platforms:

- gmake: GNU compiler (make)
- icc: Intel C/C++ compiler (calls ILE)

## Application components

The *`<installdir>/runtime/src/exit/`* subdirectory contains several sample source
modules. This program is used to check the following:

- Activation
    of a transfer to a partner known to the directory EXIT but not to Transfer
    CFT
- Activation
    of a transfer to a partner not known to either {{< TransferCFT/axwayvariablesComponentShortName >}} or the directory
    EXIT

The *`<installdir>/lib`* subdirectory contains:

- The<span class="code">` libcftexa.a`</span>
    module required to use the {{< TransferCFT/axwayvariablesComponentShortName >}} directory EXITs

## Generating the exit

To generate the sample CFTEXITA application:

1. Access the <span class="code">`<installdir>/runtime/src/exit/ `</span>directory.
1. Enter the command: <span class="code">`gmake`</span>

## Running the test

1. Access the <span class="code">`<installdir>/runtime/conf `</span>directory.
1. Generate the {{< TransferCFT/axwayvariablesComponentShortName >}} databases
    using <span class="code">`cftinit`</span> the configuration file provided
    and modified for this EXIT:<span class="code">` cft-tcp.conf`</span>
1. When the<span class="code">` cftinit complete `</span>
    message is displayed, run {{< TransferCFT/axwayvariablesComponentShortName >}} using the <span class="code">`cftstart `</span>utility: <span class="code">`cftstart`</span>
1. When the<span class="code">` CFTMAIN process   ID is xxxxx`</span> message is displayed, perform an initial standard transfer
    using the command:  
    <span class="code">`CFTUTIL send part=BOSTON, idf=TXT`</span>
1. Now submit a second transfer
    to the <span class="code">`NCFT_OK`</span> partner:  
    <span class="code">`CFTUTIL send part=NCFT_OK,idf=TXT`</span>
1. After a few seconds, you can
    check the transfer state by entering the command: <span class="code">`cftcatab`</span>
1. The transfer is successful
    because NRPART01 is defined in the DIRECTORY EXIT as being the EXTPTN01
    non- {{< TransferCFT/axwayvariablesComponentShortName >}} partner.
1. Now submit a third transfer
    to the NCFT\_OK partner:  
    <span class="code">`CFTUTIL send part=NCFT_NOK,idf=TXT`</span>
1. After a few seconds, you can
    check the transfer state by entering the command: <span class="code">`cftcatab`</span>

The transfer fails because the password is invalid, even though NRPART02
is defined in the DIRECTORY EXIT.

1. Stop {{< TransferCFT/axwayvariablesComponentShortName >}}: <span class="code">`cftstop`</span>
