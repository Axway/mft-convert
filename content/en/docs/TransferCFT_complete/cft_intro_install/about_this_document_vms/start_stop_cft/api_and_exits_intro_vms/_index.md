{
    "title": "About  APIs and exits - OpenVMS",
    "linkTitle": "Build APIs and exits",
    "weight": "220"
}This section introduces the two application families that can be developed using
two interfaces and the development kit contents for building APIs.

- Applications communicating
    with {{< TransferCFT/axwayvariablesComponentShortName  >}} to submit and monitor transfers or query the catalog,
    for example.
- Exits enabling
    user programs to take control during a send operation.

{{< TransferCFT/axwayvariablesComponentShortName  >}} provides a programming interface in C, which require that you have the development kit, C compiler, and associated tools installed on your system.

This section then describes how to
create the following applications:

- An API
    application
- File exit
- Directory
    exit

<span id="Prerequi"></span>

Prerequisites
-------------

You require a C compiler and the Linker Utility to perform the operations in this section.

C API and EXIT executables
--------------------------

All executables are available in the `'D$CFT_RUN:[bin]'` directory.

To run an executable, use the commands:

`run D$CFT_RUN:[bin]TCFTSYN.exe`

`MCR D$CFT_RUN:[bin]TCFTSYN.exe ARG1 AGR2`

Compile overview
----------------

After installing Transfer CFT and loading the profile, you can compile as follows:

### Exit overview

To compile an EXIT sample, enter:

`set def D$CFT_RUN:[SRC.EXIT]`

`@MAKEFILE_EXIT.COM`

If the script is not available following an update, retrieve it using the command:

`COPY D$CFT_INST:[DISTRIB.TEMPLATE.SRC.EXIT]makefile_*.*;*  D$CFT_RUN:[SRC.EXIT]`

### API overview

To compile a C API sample, enter:

`set def D$CFT_RUN:[SRC.CAPI]`

`@MAKEFILE_CAPI.COM`

If the script is not available following an update, retrieve it using the command:

`COPY D$CFT_INST:[DISTRIB.TEMPLATE.SRC.CAPI]makefile_*.*;*  D$CFT_RUN:[SRC.CAPI]`
