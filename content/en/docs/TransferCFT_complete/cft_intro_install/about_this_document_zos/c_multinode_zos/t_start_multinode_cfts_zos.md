{
    "title": "Start the multi-node Transfer CFT",
    "linkTitle": "Start multi-node Transfer CFTs",
    "weight": "220"
}After customizing your installation and executing the required z/OS commands, you are ready to start your multi-node Transfer CFTs. Remember that in a z/OS multi-node environment, Copilot acts as the node manager.

Start a node
------------

The node manager submits a JCL defined by the UCONF `cft.multi_node.start_node.proc_fname` parameter to start a node. (The default value is ..INSTALL(MNRMON)).

> **Note**
>
> Note: You can use the UCONF parameter cft.multi_node.start_node.user to define a specific node user, but in this case the LOAD must be APF defined. Otherwise, if not set, the node is submitted using the node manager owner by default.

> **Note**
>
> Note: Because the JCL MNRMON is an intermediate JCL, you must customize it.

### Select the start mode for the node

The following sections describe the two ways that you can start a node (customize MNRMAIN).

- STC: `          //STEP010 EXEC PGM=IKJEFT01,PARM='%RXSCFT SC'`
- JCL: `           //STEP010 EXEC PGM=IKJEFT01,PARM='%RXSCFT SJ'`

### STC

1. Extract the JCL ..INSTALL(MNRMAIN) information to create a specific STC (for example, CFTSTC).
1. In the in stream data set ‘//PARM DD \*’, you customize the start command using the STC name (MNRMON JCL).

```
set_ startcmd 's CFTSTC,JOBNAME='jobid
```

### JCL

The JCL to be submitted is defined by DDNAME CFTJCL (the JCL MNRMAIN was customized during the installation phase).

```
//CFTJCL DD DISP=SHR,
// DSN=&CFTENV..SAMPLE(MNRMAIN)
```

### How the JOBNAME is computed

Jobname is unique by LPAR, use the command DISPLAY JOBS to know the list of nodes started on the LPAR system. In the in stream data set ‘//PARM DD \*’, there are two variables that determine the JOBNAME, maskjob and masklist.

#### Maskjob

Prefix of jobname delimited by a ‘\*’.

**Example**

```
set maskjob ‘CFTNOD\*’
```

Or

Customize set maskjob using any valid REXX sentence.

**Example**

```
set maskjob strip(substr(USERID(),1,6))'t\*' (6 first characters of userid concatenated with ‘t\*’)
```

#### Masklist

This is a list of characters allowed to substitute the ‘\*’.

**Example**

```
set masklist '3456'
```

Result if maskjob=CFTNOD\*:

```
CFTNOD3,  CFTNOD4,  CFTNOD5, CFTNOD6
```

Or

**Example**

```
set masklist 'ABCD'
```

Result if maskjob= strip(substr(USERID(),1,6))'t\*' and userid is ‘A123456’:

```
A12345TA, A12345TB, A12345TC, A12345TD
```

### Start the Transfer CFT node manager

1. For each host that you configured (LPAR1, LPAR2, ...), start the node manager.
    -   Submit the JCL MNRMNG.
1. Start Transfer CFT.
    -   Submit the JCL MNSTART.
    -   You only need to perform this task once.
1. Check the status of the multi-node setup.
    -   Submit the JCL MNLNODE.
    -   Check the results and the sysout.

### Trace the node start

In addition to the standard log messages when starting a Transfer CFT node, the following display:

```
CFTI41I OMVS information for user=…..,uid uid=..,gid=0,home=(/home/cft/….)
CCFT00I Started XFB/CFT number is : 8.
CCFT04I Available storage for CFT: 24b=08408 …
```

For each start request an entry is created in the file …MONLOG, for example, when a node is started by STC.

```
Start request cftmain Jobname=SOZ113T4 25 Jun 2014 at 15:00:49 by Userid=SOP745

- Plex=PLEX1,Sysname=Z113 ,Version=z/OS 01.13.00 HBB7780
- Hostname=z-zos111b,Hostid=10.128.60.15
- Started by console command: S SOP7457A,JOBNAME=SOZ113T4

```

For example, when node started by JCL.

```
Start request cftmain Jobname=SOP745T4 13 Apr 2015 at 16:10:52 by Userid=SOP745

- Plex=PLEX1,Sysname=Z113 ,Version=z/OS 01.13.00 HBB7780
- Hostname=z-zos111b,Hostid=10.128.60.15
- Submitted by JCL IKJ56250I JOB SOP745T4(JOB03462) SUBMITTED

```
