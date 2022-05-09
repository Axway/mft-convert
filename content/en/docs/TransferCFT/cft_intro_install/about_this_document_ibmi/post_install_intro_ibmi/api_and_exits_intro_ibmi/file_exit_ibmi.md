{
    "title": "Creating  a file exit - IBM i",
    "linkTitle": "Creating a file exit",
    "weight": "290"
}This page describes a delivered sample that is designed to create an example file exit. You can use the `TCPPARAM `configuration sample that is located in `CFTPROD/UTIN`.

Prerequisites
-------------

You require the ICC and GMAKE utilities for IBM i platforms:

- gmake: GNU compiler (make)
- icc: Intel C/C++ compiler (calls ILE)

Application components
----------------------

The `<installdir>/runtime/src/exit/` subdirectory contains:

- The `exfxmp1.c` sample source module with its associated `exfus.h` include file. This program
    demonstrates the various user functions:
    -   ALLOC_TYP:
        the EXIT allocates the file
    -   OPEN_TYP: the
        EXIT opens the file
    -   DATA_TYP: the
        EXIT writes or reads the file, and so on

<!-- -->

- The `makefile `
    compilation procedure, which uses `exfxmp2.c` to generate the CFTEXITF
    program

The CFTPGM library contains the `libcftexf.srvpgm`
module. This library allows you to use the {{< TransferCFT/axwayvariablesComponentShortName  >}} file EXITs.

Generating the exit
-------------------

To generate the sample CFTEXITF application:

1. Access the `<installdir>/runtime/src/exit/` directory.
1. Enter the command: `gmake`

Testing the exit
----------------

1. Connect to the IBM session with your Transfer CFT user.
1. Generate the {{< TransferCFT/axwayvariablesComponentShortName  >}} internal datafiles
    using the `cftinit `utility with the configuration file:

    CALL PGM(CFTINIT) PARM('CFTPROD/UTIN(TCPPARAM)')

1. When the `cftinit complete`
    message is displayed, run {{< TransferCFT/axwayvariablesComponentShortName  >}} using the `cftstart `utility:

    `cftstart`

1. When the `CFTMAIN process   ID is xxxxx `message is displayed, run a transfer using the command:

    CALL PGM(CFTUTIL) PARM(SEND 'part=BOSTON, idf=fic1')

1. After a few seconds, you can
    check the transfer state by entering the following command. If the transfers have not terminated, repeat thecommand.

    `CALL PGM(CFTUTIL) PARM(LISTCAT)`

1. Stop {{< TransferCFT/axwayvariablesComponentShortName  >}} using the `cftstop`
    utility:

    `cftstop`

1. Examine the contents of the `CFTPROD/LOG. sav` file in the `CFTPROD `library and locate the messages inserted by the EXIT.  
      
    The files created in the `CFTPROD `library are empty, as the sample EXIT is
    only a simulation.
