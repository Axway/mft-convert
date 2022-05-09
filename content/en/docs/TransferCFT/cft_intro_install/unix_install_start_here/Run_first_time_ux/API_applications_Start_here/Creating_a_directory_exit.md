{
    "title": "Creating  a directory exit",
    "linkTitle": "Creating a directory exit",
    "weight": "240"
}The following example was designed from a modified version of the ****cft-tcp.conf**** configuration example, located in ****&lt;installdir&gt;/runtime/conf****. For this example, you should have customized
at least one of these files, using the instructions in [Running Transfer
CFT for the first time.]()

Prerequisites
-------------

Before running the directory exit test, you must make a few changes
to the cft-tcp.conf file, depending on the
type of network used.

1. Edit the relevant file using
    your text editor (*vi* for example) and perform the following operations.
1. In the edited file, locate
    the *cftprot* command. The following lines are displayed:

```
cftprot
id      = PeSITCFT,  
``type      = PESIT,
prof      = CFT,
...
/\*\*\* exita      = EXIT_A, \*\* See Operations Guide \*\*/
mode      = replace
```

1. Delete the comments (delimited
    by /\* at the beginning and \*/ at the end).

When the operation is complete, you should obtain
the following *cftprot* command:

```
cftprot id      = PeSITCFT,
type      = PESIT,  
prof
     = CFT,
...
exita= EXIT_A,
mode      = replace
```

1. Locate the *cftexit* command,
    commented as follows:

```
CFTEXIT ID      = EXIT_A,
PARM = EXAPARM1,
LANGUAGE = C,
PROG = 'CFTEXITA',
TYPE = ACCESS,
MODE = REPLACE \*\*\*/
```

You must remove the comments to obtain the following
command:

```
CFTEXIT ID     
= EXIT_A,
PARM      = EXAPARM1,
LANGUAGE      = C,
PROG      = 'CFTEXITA',
TYPE      = ACCESS,
MODE      = REPLACE
```

1. Locate the communication properties
    of your site, which appear at the end of the file.

<!-- -->

- If you are
    modifying the cft-tcp.conf file, you must also find every occurrence
    of the HOST string located in cfttcp-type commands and replace the X character
    strings with your system name or address

### Application components

The *&lt;installdir&gt;/runtime/src/exit/* subdirectory contains:

- A sample source
    module, called *exaxmpm.c*, with its associated include file (*exaus.h*),
    and an additional file called *exaxmpp.h*  
    This program is used to check the following features:
- Activation
    of a transfer to a partner known to the directory EXIT but not to Transfer
    CFT
- Activation
    of a transfer to a partner not known to either {{< TransferCFT/axwayvariablesComponentShortName  >}} or the directory
    EXIT

<!-- -->

- The *mk_cftexita*
    compilation procedure used to generate the CFTEXITA program

The *&lt;installdir&gt;/lib* subdirectory contains:

- The *libcftexa*.*a*
    module required to use the {{< TransferCFT/axwayvariablesComponentShortName  >}} directory EXITs

To generate the sample CFTEXITA application, proceed as follows.

1. Access the *&lt;installdir&gt;/runtime/src/exit/* directory.
1. Enter the command:

`       make -f mk_cftexita`

Running the test
----------------

1. Access the *&lt;installdir&gt;/runtime/conf* directory.
1. Generate the {{< TransferCFT/axwayvariablesComponentShortName  >}} databases
    using *cftinit* the configuration file provided
    and modified for this EXIT:` cft-tcp.conf`
1. When the *cftinit complete*
    message is displayed, run {{< TransferCFT/axwayvariablesComponentShortName  >}} using the *cftstart* utility: `cftstart`
1. When the *CFTMAIN process
    ID is xxxxx* message is displayed, perform an initial standard transfer
    using the command:  
    `CFTUTIL send part=BOSTON, idf=TXT`
1. Now submit a second transfer
    to the NCFT_OK partner.  
    `CFTUTIL send part=NCFT_OK,idf=TXT`
1. After a few seconds, you can
    check the transfer state by entering the  
    command: `cftcatab`
1. The transfer is successful
    because NRPART01 is defined in the DIRECTORY EXIT as being the EXTPTN01
    non- {{< TransferCFT/axwayvariablesComponentShortName  >}} partner.
1. Now submit a third transfer
    to the NCFT_OK partner.  
    `CFTUTIL send part=NCFT_NOK,idf=TXT`
1. After a few seconds, you can
    check the transfer state by entering the  
    command:

`cftcatab`

The transfer fails because the password is invalid, even though NRPART02
is defined in the DIRECTORY EXIT.

1. Stop {{< TransferCFT/axwayvariablesComponentShortName  >}}:

`cftstop`
