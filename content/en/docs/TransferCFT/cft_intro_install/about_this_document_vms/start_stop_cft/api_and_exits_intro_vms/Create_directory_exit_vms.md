{
    "title": "Creating  a directory exit - OpenVMS",
    "linkTitle": "Creating a directory exit",
    "weight": "260"
}This page describes a delivered sample, `cft-tcp.conf` configuration sample located in `cft_scen or d$cft_run:[conf]`, that you can use to create an example directory exit.

> **Note**
>
> Note: Please check the prerequisites if you have not already done so.

Before you start
----------------

Before running the directory exit test, modify the `cft-tcp.conf` file depending on the type of network used. Edit the file using a text editor (`EDIT, `for example) as follows.

1. Locate
    the *`cftprot`* command in the file and the following lines:
1. Delete the comments (delimited
    by /\* at the beginning and \*/ at the end). The *`cftprot`* command should resemble the following:
1. Locate the *`cftexit`* command,
    commented as follows:
1. Locate the communication properties
    for your site, which appear at the end of the file. When
    modifying the `cft-tcp.conf` file, you must also find every occurrence
    of the HOST string located in cfttcp-type commands and replace the` X` character
    strings with your system name or address.

Application components
----------------------

The `d$cft_run:[src.exit] `subdirectory contains several sample source
modules used to check the following:

- Activation
    of a transfer to a partner known to the directory EXIT but not to Transfer
    CFT
- Activation
    of a transfer to a partner not known to either {{< TransferCFT/axwayvariablesComponentShortName  >}} or the directory
    EXIT

Generating the exit
-------------------

To generate the sample CFTEXITA application:

1. Access the d$cft_run:[src.exit]` `directory.
1. Enter the command: `makefile exit`

Running the test
----------------

1. Access the `cft_scen` directory.
1. Generate the {{< TransferCFT/axwayvariablesComponentShortName  >}} databases
    using `cftinit` the configuration file provided
    and modified for this EXIT:` cft-tcp.conf`
1. After the` cftinit complete `
    message is displayed, start {{< TransferCFT/axwayvariablesComponentShortName  >}}: `cft start`
1. When the` CFTMAIN process   ID is xxxxx` message displays, perform an initial transfer
    using the command:  
    `CFTUTIL send part=BOSTON, idf=TXT`
1. Now submit a second transfer
    to the `NCFT_OK` partner:  
    `CFTUTIL send part=NCFT_OK,idf=TXT`
1. After a few seconds,
    check the transfer state by entering the command: `CFTUTIL listcat`  
    The transfer is successful
    because the `NRPART01 `is defined in the DIRECTORY EXIT as being the `EXTPTN01 ` non- {{< TransferCFT/axwayvariablesComponentShortName  >}} partner.
1. Now submit a third transfer
    to the `NCFT_OK` partner:  
    `CFTUTIL send part=NCFT_NOK,idf=TXT`
1. After a few seconds,
    check the transfer state by entering the command: `CFTUTIL listcat`  
    The transfer fails because the password is invalid, even though NRPART02
    is defined in the DIRECTORY EXIT.
1. Stop {{< TransferCFT/axwayvariablesComponentShortName  >}}: `cft stop`
