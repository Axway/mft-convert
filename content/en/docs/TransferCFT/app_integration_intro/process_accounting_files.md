{
    "title": "Processing  accounting files",
    "linkTitle": "Processing accounting files",
    "weight": "240"
}You can write the statistical data for successful transfers to a file by defining the CFTACCNT type=file command as described in [Recording mode for statistical data](../../admin_intro/admin_config_commands/cftaccnt_concepts). You can then extract this data to use with other applications. The extracted data is available in C language for all platforms and in COBOL for z/OS and IBM i systems.

Delivered files
---------------

### Include file

{{< TransferCFT/suitevariablesTransferCFTName  >}} provides an include file, `cftcnt.h`, that provides the account file record structure to be used by the program. Depending on the operating system, the file is located in:

- UNIX, Windows, HP NonStop, and IBM i (IFS): &lt;installdir&gt;/home/inc
- z/OS: Distrib..H (CFTCNT)
- Openvms: D$CFT_INST:[INC]

### Source samples

Transfer CFT delivers a sample source written in C language called `exacct.c` as well as a compilation procedure, which is system dependent.


| System  | File location  | Build command  |
| --- | --- | --- |
| UNIX  | &lt;install_dir&gt;/home/distrib/template/src/exit  | makefile  |
| Windows  | &lt;install_dir&gt;\home\distrib\template\src\exit  | exit.mak  |
| z/OS  | Distrib..SAMPLEC(EXACCT)  | Compilation: Runtime..INSTALL(I91APICP)<br/> Link-edit: Runtime..INSTALL(I92APILK) |
| IBM i (IFS)  | &lt;install_dir&gt;/home/distrib/template/src/exit  | gmake  |
| HP NonStop  | &lt;install_dir&gt;/home/distrib/template/src/exit  | makefile  |
| OpenVMS  | D$CFT_INST:[DISTRIB.TEMPLATE.SRC.EXIT]  | Compilation and link-edit  |


> **Note**
>
> Note: The exacct.c sample only supports an account file in V24 format. However, if the UCONF cft.cftlog.fname.atts and cft.cftaccnt.fname.attsparameters are not defined, CFTINIT will create the account and log file using the V23 format. (These UCONF values are only available on OpenVMS, z/OS, and IBM i systems.)Otherwise, use the following commands to generate the file in the correct format:CFTUTIL CFTFILE type=accnt,fname=&lt;CFTACCNT file&gt;,format=V24,mode=replaceCFTUTIL CFTFILE type=accnt,fname=&lt;CFTACCNTA file&gt;,format=V24,mode=replace

Executing the sample file
-------------------------

You can use the following steps for all supported operating systems except z/OS:

To generate a sample file for example on UNIX:

1. Copy the two files from `<install_dir>/home/distrib/template/src/exit` to `<install_dir>/runtime/src/exit`.
1. Enter the system appropriate compile command.

The EXACCT executable file is automatically stored in:

- UNIX, HP NonStop, IBM i (IFS): &lt;installdir&gt;/runtime/bin/EXACCT &lt;account file name&gt;
- Windows: &lt;installdir&gt;\\runtime\\bin\\EXACCT &lt;account file name&gt;
- OpenVMS: d$cft_run:[bin]
- IBM i (OS/400): CFTPGM library

objects are created

the executable program is copied to the runtime\\bin

you can use this program to display the account file as a display

if you have a program that you want to use the program in an application that extracts the information from the account, use the sample to adapt. to manage data for an application. transfer related information for applications to use.

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

### VMS example

****Syntax****

```
mcr D$CFT_RUN:[BIN]EXACCT.EXE CFTACCNT
```

Results
-------

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
