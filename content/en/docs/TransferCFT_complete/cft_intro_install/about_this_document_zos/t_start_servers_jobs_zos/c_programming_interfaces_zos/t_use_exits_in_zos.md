{
    "title": "Specific z/OS exits in Transfer CFT",
    "linkTitle": "Specific z/OS exits in Transfer CFT",
    "weight": "290"
}This section describes exits in Transfer CFT z/OS, and includes information about:

- Exit lists
- File-type Exits
- Calling APIs in Exits

Exit lists
----------

The exit list is installed automatically as CFTEXILI. For more information, refer to the Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} documentation on *[*Exit Lists*](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/Prog/Exits/EXIT_list/exit_lists_start_here.htm)*.

File type exits
---------------

For more information on exits in Transfer CFT, refer to the Transfer CFT documentation on [File Exits](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/Prog/Exits/File_exit/File_exit_Start_here.htm).

This section provides additional information required to construct exits under z/OS.

### Calling Transfer CFT exits in z/OS

Transfer CFT provides the following types exits:

- Directory (\*)=A
- File (\*)=F
- Beginning-of-transfer (\*)=B

<!-- -->

- End-of-transfer (\*)=E

(\*) In the following descriptions, the ‘\*’ character refers to the four exits, and is replaced with one of the four characters (A, E, F, or B) as applicable.

An exit is a program comprising 3 parts:

- The exit monitor, which is delivered with Transfer CFT

<!-- -->

- The ‘EX\*INI’ program

<!-- -->

- The main program whose address is returned by the EX\*INI program

The ‘EX\*INI’ program and the main program are written by the Transfer CFT user, and then LINK EDITED with the exit server.

The 'EX\*INI' INIT function name is determined by the exit type:

- EXAINI: directory type

<!-- -->

- EXFINI: file type
- EXBINI: beginning-of-transfer type

<!-- -->

- EXEINI: end of transfer type

The exit program is dynamically loaded by Transfer CFT on the first request. It is executed in the same way as an OS monitor. You may write the programs in ASSEMBLER 370, IBM C or COBOL, *or* C and COBOL together.

The programs can be called repeatedly for different transfers, meaning the programs must be serially reusable. Each data area in the program is reset each time it is called and NOT declared with an initial value.

Several different exits can be executed concurrently, each of them being a separate task. It is impossible for 2 exits to write to the same file, particularly if it is a ‘SYSPRINT’, ‘SYSDBOUT’ or ‘STDOUT’ file used to debug C or COBOL languages.

The exit interface dialogs with the monitor using 31-bit memory fields.

The names of the exit load-modules must always begin with the 6-character ‘CFTEXI’ string.

Exits are called in problem mode and with a user key. They have the same privileges as the Transfer CFT, which in most cases is an authorized program.

The SGTRACE 512 debug option allows you to obtain the following information in the SGTRACE file, before each call to the processing function:

- ‘EX\*3RUN-PARM1-5’: 5 words in hexadecimal characters:

<!-- -->

- Word 1:  processing address

<!-- -->

- Words 2 to 5: addresses of the 4 received parameters

<!-- -->

- ‘EX\*3RUN-DUMPCTX: 32 or nnn bytes in hexadecimal characters

The first 32 bytes (SGTRACE=512) or the whole parameter field (SGTRACE=544) submitted to the exit are listed. If an ABEND occurs during the exit, the Transfer CFT error processing function takes control. A brief diagnosis of the error is displayed on the z/OS console.

Example of a Transfer CFT diagnosis:

```
SGAB00E: Jobname Stepname SITMOS   Date Time Version
ABEND = 84sssuuu ,   PSW =xxxxxxxx xxxxxxxx ,  EPA  =xxxxxxxx
SGAB01E: REG 0-7 =xxxxxxxx .......
SGAB02E: REG 8-15=xxxxxxxx ......
SGAB03E:   SGNUC =xxxxxxxx , TRACE =xxxxxxxx ...
SGAB08E: INSTRUCTIONS :    xxxxxxxx:-6           xxxxxxx
SGAB08E: \*\* USER EXIT ABEND DETECTED \*\* L=l EPA= x NAME=Mod
SGAB30E: xxxxxxxx CFTEXInn TCB:  Tcbaddr SGNUC: xxx EPA : Epa
SGAB09E: TASK-AC =CFTEXInn , KCB =xx , PRV =xx ,SGSAVE =xxSAVE = Saveaddr MODULE = EXITASK  date Version  EPA= xx RET= xx
```

Additionally, the following are valid for the example:

- The Transfer CFT internal trace is edited in the SGSTAE file
- A dump is made, depending on the MAXDUMP value
- If Transfer CFT detects a lack of response from the exit, then the transfer is placed in the HOLD status

For an exit in COBOL or C, the diagnostics are also available in the 'CEEDUMP' file.

****Related topics****

- [Managing exits]()
