{
    "title": "Enable Sentinel and Event Router",
    "linkTitle": "Enable Sentinel and Event Router ",
    "weight": "260"
}Enabling the Axway Sentinel option for Transfer CFT is generally comprised of the following steps, which you must execute in the order listed:

- Install the Event Router

<!-- -->

- Set the Sentinel parameters in the unified configuration file

<!-- -->

- Create a LOGGER type padding file (optional)
    -   Use the LOGGER in a SYSPLEX

<!-- -->

- Shut down Transfer CFT

<!-- -->

- Restart Transfer CFT

> **Note**
>
> Note: Universal Agent installation is necessary only for end-to-end application monitoring.

Procedure
---------

This section provides step by step instructions for installing Sentinel for Transfer CFT z/OS.

<span id="Install the Event Router"></span>

### Install the Event Router

For detailed information about Event Router installation refer to the Sentinel Event Router information in the *Axway A 5 Suite Prerequisites and Installation Guide*.

- During the installation procedure, specify the creation of the BUFFER file as the type LOGGER (under SYSPLEX).
- During setup, a LOGGER type BUFFER file is created. Note the name of the file for use when setting values of the configuration file.

If the Event Router is not set up, you must create a LOGGER type file using the JCL: SN10CLGR.

<span id="Set the Sentinel parameters  A00CUSTO"></span>

### Set the Sentinel parameters A00CUSTO

You can set the Sentinel parameters in the unified configuration. If the Sentinel parameters are already customized, skip this section. Otherwise, customization parameters are located in the parameter file A03PARM of the installation library (sntlocad, sntladd, sntlp, sntlqueue, sntlgstr).

For a description of the general parameters refer to the Event Router information in the *Sentinel* documentation.

> **Note**
>
> Note: If the communication mode between Transfer CFT and the Event Router is an XCF type, and the TRKSVC is equal to 0, then you must define the Transfer CFT executable library as APF authorized.

<span id="Activate the unified configuration file parameters SN05CONF"></span>

### Activate the unified configuration file parameters SN05CONF

Before submitting the SN05CONF JCL, you can modify the command file. Enter:

****Trktname parameter****

- Using a buffer file is strongly encouraged, but not mandatory. If you prefer to not use a buffer file, modify the command UCONFSET as follows:

```
UCONFSET ID=sentinel.TRKTNAME,VALUE=
```

- The following message displays in the log when you restart Transfer CFT:

```
CFTS31W XTRK Warning No Buffer File(LOGGER file)defined Error Code = -1
```

To connect to the Event Router via XFC, manually modify the commands file to activate the appropriate communication mode and comment commands:

```
/\* UCONFSET ID=sentinel.TRKIPADDR,         VALUE=sntladdr                  
     UCONFSET ID=sentinel.TRKIPPORT,VALUE=sntlp\*/
```
<span id="Create_a_LOGGER_type_padding_file (optional)"></span>

### Create a LOGGER type padding file (optional)

If the Transfer CFT does not use the same padding file as the Event Router, you must create a padding file. This LOGGER type file should not be shared with other applications.  

The new LOGGER file is a DASD-ONLY type file. Use the JCL SN10CLGR, via the IBM IXCMIAPU utility, to create this file.

**Example**

```
DEFINE LOGSTREAM NAME(sntlgstr)
DASDONLY(YES)
LOWOFFLOAD(0)
LS_SIZE(180)
STG_SIZE(0)
MAXBUFSIZE(64000)
```

The keywords that are displayed in bold should be substituted by the JCL A00CUSTO. For more information refer to the IBM documentation on *Setting up a Sysplex*.

#### Use the LOGGER file in a SYSPLEX  

To create an overflow file (LOGGER) that is available for all SYSPLEX partitions, in the Coupling Facility Resource Manager (CFRM) add the following structure (`&userstr`) to the coupling data set.

****Example****

```
The following is an example of a structure definition:
STRUCTURE NAME(&userstr)
SIZE(6000)
INITSIZE(1200)
REBUILDPERCENT(30)
PRELIST(FACIL02,FACIL01)
 
The following is an example of a logstream definition:
DEFINE LOGSTREAM NAME(&LGRID)
STRUCTNAME(&userstr)
&Userstr is the structure name
&LGRID is the logger file name
```

> **Note**
>
> Note: You must be a system administrator to perform these operations.

<span id="Overflow file definition"></span>

### Overflow file definition

The following table describes the overflow file definition for the Logger file, where:

- TRKSHAREDFILE=YES is MANDATORY when the logger file is shared between the Event Router and applications. Set this to NO if the applications are sending messages directly to the Sentinel server without going through a ER
- The log structure is ONLY used to define a logger file shared between the partitions of the SYSPLEX, and is NOT referenced in any parameters


|   | Event Router  | TRKUTIL  | Transfer CFT 2.7 and later  |
| --- | --- | --- | --- |
| Configuration file  | USEPARIN  | TRKCONF  | UCONF  |
| Logger file  | (AGENT)<br/> api_file= | TRKTNAME=  | UCONFSET ID=sentinel.TRKTNAME, VALUE=xxxx.xxxx.xxx  |
| - &quot; -  | - &quot; -  | TRKSHAREDFILE=YES  | UCONFSET ID=sentinel.TRKSHAREDFILE,VALUE=YES  |


<span id="Communication with the Event Router"></span>

###  Communication with the Event Router

The following parameters define communication with the Event Router via XCF. In this setup:

- The XCF definition (queue=xxxx) is the XCF member name representing the ER server
- The XCF group is PELISCOP by default. You can modify this default by setting queue = “member group”


|   | Event Router  | TRKUTIL  | Transfer CFT 2.6.x  |
| --- | --- | --- | --- |
| Configuration file  | USEPARIN  | TRKCONF  | UCONF  |
| SVC  | (SYSTEM)<br/> svc_nb=nnn | TRKSVC=nnn  | UCONFSET ID=sentinel.TRKSVC,VALUE=nnn  |
| XCF definition  | (AGENT)queue=  | TRKQUEUE=  | UCONFSET ID=sentinel.TRKQUEUE,VALUE=xxxx  |
| XCF definition  | (AGENT)queue=  | TRKTYPE=XCF  | UCONFSET ID=sentinel.TRKTYPE,VALUE=XCF  |


<span id="Modify the Transfer CFT start-up procedure"></span>

### Modify the Transfer CFT start-up procedure

During the start-up procedure, the task dedicated to managing messages towards Sentinel allocates a sysout (ddname=TRKOUT) for any generated error messages or traces.

<span id="Register configuration file values"></span>

### Register configuration file values

For configuration file modifications to be taken into account, including updates, stop and then restart Transfer CFT.

<span id="Set message sending mode"></span>

### Set message sending mode

Use the TRKTMODE parameter of the configuration file to manage the sending mode for messages emitted towards Sentinel.

Possible values:

- TRKTMODE=I: Immediate, default value

<!-- -->

- TRKTMODE=D: Differed, the messages are stored to the buffer file

<!-- -->

- TRKTMODE=R: Retry, enables the management of a sending delay via a parameter TRKTCONNRETRY=nn in case of a connection problem with SENTINEL

### TCP/IP source port conflict

Sentinel notifications can get the source port from the system when the variable sentinel.trk_min_port is set to '0' (zero).

The system will assign one of the Ephemeral Port range (default is 1024 - 65535)

Command: uconfset id='sentinel.trk_min_port',value=0

### End-to-End monitoring examples

The following JCLs are available as end-to-end monitoring examples:

- SNTLCFT: The deposit of a file transfer command via the CFTUTIL utility, with the same CycleID as generated by the TRKUTIL SENDEVENT command. This is available in the INSTALL library.

<!-- -->

- SNTLEXEC: The end of transfer procedure, associating an acknowledgement via the CFTUTIL utility and the ‘COMPUTEIDENT’, ‘SENDEVENT’, and ‘SENDCYCLE’ TRKUTIL command. This is available in the EXEC library.

<span id="Heartbeats zOS"></span><span id="kanchor49"></span>

Implement Heartbeat functionality
---------------------------------

The CFTHEART JCL sets the unified configuration heartbeat values as follows:

- sentinel.heartbeat.enable = YES
- sentinel.heartbeat.periodicity = 300 (This recommended value has a direct correlation with the value defined in the Tracked Object.)
- sentinel.heartbeat.script = &lt;install_dir&gt;/extras/sentinel/MFTheartbeat.\*
- sentinel.trkipaddr = sentinel.server.address
- sentinel.trkipport = Sentinel.qlt/auto.port value (default = 1305)
- sentinel.xfb.enable = YES
    &lt;/li&gt;
