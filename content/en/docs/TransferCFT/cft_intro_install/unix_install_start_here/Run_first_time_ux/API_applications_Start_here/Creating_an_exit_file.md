---

    title: Creating  an exit file
    linkTitle: Creating an exit file
    weight: 220

---
The example described in this topic was designed to operate using the <span class="bold_in_para">****cft-tcp.conf****</span> configuration example located in <span class="bold_in_para">****&lt;installdir>/runtime/conf****</span>.
For this example, you should have already customized the
file using the instructions in [*Running Transfer
CFT for the First Time*]().

## Application components

The *&lt;installdir>/runtime/src/exit/* subdirectory contains:

- A sample source
    module, called *exfxmp1.c*, with its associated include file *exfus.h*
- This program
    demonstrates the various user functions:
- ALLOC\_TYP:
    the EXIT allocates the file
- OPEN\_TYP: the
    EXIT opens the file
- DATA\_TYP: the
    EXIT writes or reads the file

<!-- -->

- And so on

<!-- -->

- The *mk\_cftexitf*
    compilation procedure, which uses *exfxmp2.c* to generate the *CFTEXITF*
    program

The *&lt;installdir>/lib* subdirectory contains the:

- *libcftexf.a*
    module; this library allows you to use the {{< TransferCFT/axwayvariablesComponentShortName >}} file EXITs

To generate the sample CFTEXITF application, proceed as follows:

1. Access the *&lt;installdir>/runtime/src/exit/* directory.
1. Enter the command:

`     make   -f mk_cftexitf`

## Testing the exit

1. Access the *&lt;installdir>/runtime/conf/* directory.

1. Generate the {{< TransferCFT/axwayvariablesComponentShortName >}} internal datafiles
    using the *cftinit* utility with the configuration file:

    `cftinit cft-tcp.conf`

1. When the *cftinit complete*
    message is displayed, run {{< TransferCFT/axwayvariablesComponentShortName >}} using the cftstart utility:

    `cftstart`

1. When the *CFTMAIN process
    ID is xxxxx* message is displayed, run a transfer using the command:

    `CFTUTIL send part=BOSTON, idf=fic1`

1. After a few seconds, you can
    check the transfer state by entering the following command. If the transfers have not terminated, repeat the *cftcatab* command.

    `cftcatab`

1. Stop {{< TransferCFT/axwayvariablesComponentShortName >}} using the *cftstop*
    utility:

    `cftstop`

1. Examine the contents of the
    *cft\_log. sav* file in the *&lt;installdir>/runtime/log/* directory and locate the
    messages inserted by the EXIT.  
      
    The files created in *&lt;installdir>/runtime/* are empty, as the sample EXIT is
    only a simulation.
