---

    title: Processing  accounting files
    linkTitle: Processing accounting files
    weight: 240

---
You can write the statistical data for successful transfers to a file by defining the CFTACCNT type=file command as described in [Recording mode for statistical data](../../admin_intro/admin_config_commands/cftaccnt_concepts). You can then extract this data to use with other applications. The extracted data is available in C language for all platforms and in COBOL for z/OS and IBM i systems.

## Delivered files

### Include file

{{< TransferCFT/suitevariablesTransferCFTName  >}} provides an include file, <span class="code">`cftcnt.h`</span>, that provides the account file record structure to be used by the program. Depending on the operating system, the file is located in:

- UNIX, Windows, HP NonStop, and IBM i (IFS): &lt;installdir>/home/inc
- z/OS: Distrib..H (CFTCNT)

### Sample source

Transfer CFT delivers a sample source written in C language called <span class="code">`exacct.c`</span> as well as a compilation procedure, which is system dependent:


| System  | File location  | Build command  |
| --- | --- | --- |
| UNIX  | &lt;install_dir&gt;/home/distrib/template/src/exit  | makefile  |
| Windows  | &lt;install_dir&gt;\home\distrib\template\src\exit  | exit.mak  |
| z/OS  | Distrib..SAMPLEC(EXACCT)  | Compilation: Runtime..INSTALL(I91APICP)<br/> Link-edit: Runtime..INSTALL(I92APILK) |
| IBM i (IFS)  | &lt;install_dir&gt;/home/distrib/template/src/exit  | gmake  |
| HP NonStop  | &lt;install_dir&gt;/home/distrib/template/src/exit  | makefile  |


## Executing the sample file

You can use the following steps for all supported operating systems except z/OS:

To generate a sample file for example on UNIX:

1. Copy the two files from <span class="code">`<install_dir>/home/distrib/template/src/exit`</span> to <span class="code">`<install_dir>/runtime/src/exit`</span>.
1. Enter the system appropriate compile command.

The EXACCT executable file is automatically stored in:

- UNIX, HP NonStop, IBM i (IFS): &lt;installdir>/runtime/bin/EXACCT &lt;account file name>
- Windows: &lt;installdir>\\runtime\\bin\\EXACCT &lt;account file name>

Perform the following steps if you are running on a z/OS system.

1. Copy the Runtime..SAMPLEC(EXACCT) to Distrib..SAMPLEC(EXACCT)
1. Compile.
1. Perform the link edit.

### UNIX, HP NonStop, and IBM i (IFS) example

****Syntax****

```
EXACCT <install_dir>/runtime/accnt/cftaccnt
```

### Windows example

****Syntax****

```
EXACCT <install_dir>\\home\\distrib\\template\\src\\exit
```

### z/OS example

```
//S1 EXEC PGM=EXACCT,PARM='acount_file_name'
//SYSPRINT DD SYSOUT=\*
//SYSOUT DD SYSOUT=\*
```

## Results

All operating systems should have a resulting file similar to the following:

```
ACCOUNT FILE
------------
Transfer id. IDT
B2508304
Direct DIRECT = RECV
Mode MODE = REQUESTER
Type TYPE = FILE
 
Item type DIFTYP = SINGLE
 
Logic file identifier IDF = TEST
Logic file network id NIDF = TEST
Protocol identifier PROT = PESITSSL
File Name FNAME =
\*recv/FTEST
...
\*
 
Receiver application RAPPL =
\*
Record format FRECFM = U
length FLRECL = 04096
File compression NCOMP = 00
Bytes FCARS = 0000007104
```
