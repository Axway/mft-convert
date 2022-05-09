{
    "title": "Diagnostic commands ",
    "linkTitle": "Diagnostic commands",
    "weight": "260"
}The diagnostic commands search for operating errors that occur in Transfer CFT, and should be used under Axway customer support supervision. The commands syntax usage are described in the following sections.

Diagnostic command descriptions
-------------------------------

### SGTRACE command

The SGTRACE command allows an external trace file to be processed. The possible values for the SGTRACE command are:

- ON: Activates the trace

<!-- -->

- OFF: Deactivates the trace

<!-- -->

- CLOSE: Closes the trace file

<!-- -->

- OPEN: Opens the trace file

<!-- -->

- nnnnnn: Numeric value between 0 and 511, which allows the events to be selected when the corresponding value is true

Values

- 1: Trace of network messages

<!-- -->

- 2: Trace of non-0 return codes and other errors

<!-- -->

- 4: Trace of DASDM/CATALOG/SVC99 file operations

<!-- -->

- 8: Trace of file read/write operations

<!-- -->

- 16: Trace of calls to C functions

<!-- -->

- 64: Trace of communications between Transfer CFT tasks

<!-- -->

- 128: Trace of program calls

<!-- -->

- 256: Trace of the display interface actions

<!-- -->

- 512: Trace of calls to Transfer CFT exits

The SGTRACE command allows for a dynamic modification of the default option defined in SGINSTAL, or set in PARM.

#### Using the SGTRACE file

The SGTRACE file opens automatically when Transfer CFT initializes, and the trace is marked as logically active if the opening is correct. The trace value used is the one defined in the SGINSTAL MACRO installation step (A12OPTS).

Events are recorded if the following conditions are satisfied:

- The event is selected.

<!-- -->

- The trace is logically active.

<!-- -->

- The file is open.

The file has the FIXED BLOCKED, LRECL=132 format. You can change the value of the trace used by the PARM field of each Transfer CFT.

**Example**

```
// UTIL EXEC PGM=CFTUTIL,
// PARM=‘SGTRACE nnn’
```

SGTRACE nnn must be the first and second parameters of the PARM field.

If the DD SGTRACE card is absent, Transfer CFT OS/390 allocates the file dynamically with SYSOUT = A.

### ITRACE command

The ITRACE command enables the internal trace of Transfer CFT and the associated file SGSTAE to be managed. The possible values of the ITRACE command are:

- CLOSE: Closes the file trace

<!-- -->

- OPEN: Opens the file trace

<!-- -->

- nnnnnn: Numeric value from 4 to 4096 allowing the size of the internal trace buffer (in Kbytes) to be modified

The internal trace is automatically activated when you start Transfer CFT. The size of the trace buffer used is the size defined during installation, in the MACRO SGINSTALL.

#### Using the SGSTAE file

The SGSTAE file is automatically opened when you start Transfer CFT. This file receives a formatted version of the internal trace for each ABEND of Transfer CFT. This file is of the format FIXED BLOCKED, LRECL=132.

If the DD SGSTAE card is not present, Transfer CFT dynamically allocates a file with SYSOUT = A.

### DEBUG command

The DEBUG command enables you to request a formatted version of the internal trace of Transfer CFT, in the file SGSTAE. This command does not have any parameters.

### ABEND command

The ABEND command causes an ABEND 0C1 in the operator module. It causes a DUMP of Transfer CFT and generation of a formatted version of the internal trace of Transfer CFT, in the file SGSTAE. This command does not have any parameters. The command decrements the counters MAXABEND and MAXDUMP defined in SGINSTAL.

### ECHO command

The ECHO command has no effect, and ends with messages such as the following, which let you check that Transfer CFT is in operating condition:

`'SGOP00I MVSv32x-B102162-2017/03/17'‘SGOP02I Command Complete 18/05/2017, 15:52:03 User=xxxxxx'`

### CACHE command

The cache command is used to control the Transfer CFT catalog cache. There are two options available:

- CACHE QUERY: to query all cache settings. For example:

    DDIS30i Cache Dsn=cftv2..CATALOG Common/Private

    DDIS31i Cache Alet=alet Sid=plex Ori=1000 N/p=4 Rec=rrr

    DDIS32i Cache Pall=total In=recin nbr Users nbds D/S

    Common/Private: The cache is shared (common) or not.

> -   Alet: The ALET of the first data space
> -   Plex: The SYSPLEX member running the cache
> -   Ori: The system offset in the data space
> -   N/P: the number of 4K pages needed to save one CFTCAT record
> -   Rrr: the total number of records in the cache
> -   Total : the total number of allocated pages
> -   Recin : the numbers of cached records in the cache
> -   Nbr : the numbers of users sharing this cache, valid only with COMMON status
> -   Nbds : the number of dataspace used.

- CACHE LOAD: to force the complete load of the Transfer CFT catalog into the cache. Use extreme care with his command! At end of processing, messages DDIS30I to DDIS32I are displayed.

The ? command
-------------

The ? command enables you to find the status of certain Transfer CFT components. Available options include:

- ? TASK: Lists Transfer CFT tasks

<!-- -->

- ? FILES: Lists the open files

<!-- -->

- ? ENQ: Lists the shared resources and conflicts

<!-- -->

- ? SCB: Lists the CFT queues and their status

<!-- -->

- ? TCP: Lists the status of the TCP/IP sub-task

<!-- -->

- ? APF: Lists the APF ON/OFF status and the default user

<!-- -->

- ? MEM: Lists the memory status in the SGTRACE file

<!-- -->

- ? Thhhhhhhh: Displays the memory at hexadecimal address ‘hhhhhhhh’ in the Transfer CFT area, 16 bytes aligned on a double word

<!-- -->

- ? TRK: Lists the sub-task status

<!-- -->

- ? SSL: Lists the CFTTSSL sub-task status

<!-- -->

- ? TCOM, COS, LOG...: Lists the state under the task CFTTCOM, CFTTCOMS (Synchronous), CFTLOG, and so on

<!-- -->

- ? EXITS: Lists the state of the CFTEXIT subtask(s)

<!-- -->

- ? ABTCP: Lists the TCP/IP sub-task status, and forces an ABEND-S0C6 in this sub-task. Use only under Axway support supervision

<!-- -->

- ? ABTRK: Lists the CFTTRK sub-task status, and forces an ABEND-S0C6 in this sub-task. Use only under Axway support supervision

<!-- -->

- ? ABSSL: Lists the CFTTSSL sub-task status, and forces an ABEND-S0C6 in this sub-task. Use only under Axway support supervision 

<!-- -->

- ? ABCOM, ABCOS, and ABLOG: Lists the CFTCOM, CFTCOMS, CFTLOG task status and launches an ABEND-S0C6 in the sub-task. Use only under Axway support supervision
- ? TIOT: Lists the active entries in the TIOT (task I/O table).

### DISPLAY command response

#### TASK

```
DTSK01I TASK SUMMARY (SGNUC=001EA000):
DTSK02I 002BFA20 " L62RCAK" EPA=07602480 OWN=CFTR324 PRIVATE.
Addr task block, EPA, user name, type-private/ OS
DTOD03I TOD=16:11:50:155444.
TOD of the last DISPATCH
DTSK03I 0022F000 "CFTTPRO " EPA=00000000 TCB=006F6388 OWN=CFTR223. OS Task
```

#### FILES

```
DFIL01I FILE SUMMARY:
DFIL02I 0022EE90 FIL00007 SOP$CFT.REF.C324.COM UPDATE
Addr FCB, DDNAME, DSNAME, Access type
DFIL04I Read=2550 Write=145 TOD=14:01:27:985867 GU SGFVSDIR 240517 B102162Read count, write count, TOD of the last access, operation, module/date/revision
```

#### ENQ

Follow-up:                                       

```
DENQ01I ENQ SUMMARY :
DENQ02I 0725DEA0 "CFTPARM " "90.SOP$CFT.REF.C324.PARM
" EXC SYSTEM .
Addr block, QNAME RNAME Type Scope
DTOD04I TOD=16:11:46:351245 tas=00065900.
ENQ TODof the list of waiting requesters
```

#### SCB

```
DSCB01I SCB SUMMARY: DSCB02I:ADR=002BF520 RF=1BB20069 FL=110000E8 OW=001FEA20(CFTINTV ) PC=0
 
TM=8640001. Addr File, Reference, flags, Creative tasks, Number of posted messages, Waiting time, 1/100ths
 
DTOD03I TOD=12:07:57:156629. TOD of hold
```

#### APF

```
CFAU02I JOB "CFTR223 " USERID "CFTR223 " IS APF-AUTH.
Jobname, Userid RACF, Indication APF ON/OFF
CARM01I Automatic restart for CFTR223 (STC05479) started, Element="XIDPARM
```

If APF is ON, the name of the ARM element used is recorded.

#### MEM

Real space used by CFT 0116BEA0 freed on pages 0003A160 allocated below 002B6000 freed below 0000C7A0

The formatting is found in the SGTRACE file, such as in a z/OS dump, and calls the same module as the dump.

****Related topics****

****[The ? command]()****
