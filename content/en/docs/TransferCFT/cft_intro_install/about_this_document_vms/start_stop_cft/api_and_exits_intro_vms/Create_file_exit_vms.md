{
    "title": "Creating  a file exit - OpenVMS",
    "linkTitle": "Creating a file exit",
    "weight": "250"
}This page describes a delivered sample that you can use as the basis for a file exit.

> **Note**
>
> Note: Please check the prerequisites if you have not already done so.

Application components
----------------------

The `d$cft_run:[src.exit]` subdirectory contains:

- The `exfxmp1.c` sample source module with its associated `exfus.h` include file. This program
    demonstrates the various user functions:
    -   ALLOC_TYP:
        the EXIT allocates the file
    -   OPEN_TYP: the
        EXIT opens the file
    -   DATA_TYP: the
        EXIT writes or reads the file, and so on

<!-- -->

- The `makefile_exit.com`
    compilation procedure, which uses `exfxmp2.c` to generate the CFTEXITF
    program

Generating the exit
-------------------

To generate the sample CFTEXITF application:

1. Access the `d$cft_run:[src.exit]` directory.
1. Enter the command: `@makefile_exit.com`

Testing the exit
----------------

1. Connect to the OpenVMS session with your Transfer CFT user.
1. Generate the {{< TransferCFT/axwayvariablesComponentShortName  >}} internal datafiles
    using the `cftinit `utility with the configuration file:

    `cftinit cft_scen:cft-tcp.conf`

1. When the `cftinit complete`
    message is displayed, start {{< TransferCFT/axwayvariablesComponentLongName  >}}:

    `cft start`

1. When the `CFTMAIN process   ID is xxxxx `message is displayed, run a transfer using the command:

    `CFTUTIL SEND part=BOSTON,idf=fic1`

1. After a few seconds, you can
    check the transfer state by entering the following command. If the transfers have not terminated, repeat the command.

    `CFTUTIL listcat`

1. Stop {{< TransferCFT/axwayvariablesComponentShortName  >}}:

    `cft stop`

1. Examine the contents of the `cftlog` file in the `d$cft_run:[log]`library and locate the messages inserted by the EXIT.
