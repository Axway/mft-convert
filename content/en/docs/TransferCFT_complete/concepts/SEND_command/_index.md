{
    "title": "Send a file",
    "linkTitle": "Using the SEND command",
    "weight": "160"
}How to use the SEND FILE command
--------------------------------

This section describes how to use the SEND command to perform a file transfer. It begins with the simple examples and builds in complexity. However, there are many Transfer CFT parameters that can help you customize your flows, which are described in feature specific topics. Examples in this page are divided into the following umbrella categories:

- [Use or override default values](#Use)
- [Use file name parameters](#Use2)
- [Use scheduling features](#Use3)
- [Execute pre and post transfer activities](#Execute)
- [Use visibility features to link an application to a transfer](#Use4)
- [Miscellaneous send features](#Miscella)

See also the details on using [symbolic variables](../../c_intro_userinterfaces/command_summary/symbolic_variables).

<span id="Use"></span>

Use or override default values
------------------------------

### Send a file using only the partner definition

You can perform a basic SEND command by specifying only the partner where the IDF (model file) in the partner definition is used by default. If the partner definition does not include an IDF, the global default model file value is used.

In this example, the `store1 `partner values including the `dailysales `(IDF) are used.

```
cftpart id=store1, idf=dailysales
send part=store1
```

> **Note**
>
> Note: When using Central Governance, be sure to use a SEND command syntax that includes an IDF corresponding to a flow created in Central Governance.

### Override the model file default values

You can create your own model files to use with an existing partner. When specified in the send command, the new model file overrides the partner and global default model file value.

```
send part=store1, idf=newmodel
```

> **Note**
>
> Note: If you enter the name of a model file identifier that does not exist, the partner's default model file is used. If no IDF is defined for the partner then BIN, the global model file default, is used.

### Override the model file values

Using additional send command parameters, such as the file name, you can override values in the model file.

```
send part=store1, idf=model, fname=newfile
```

### Blocking the override functionality

In some cases you may not want to allow the model file values to be overridden. To block the override functionality set the force parameter to yes. In this example, the fname=oldfile and not newfile is used.

```
cftsend id=newmodel1, fname=oldfile.... force=yes
send part=store1, idf=newmodel1, fname=newfile
```
<span id="Use2"></span>

Use file name parameters
------------------------

There are several ways for you to provide the file name in the SEND command to achieve the actions described in this section, such as sending files based on a list.

### Send a file using only the file name

```
send part=store1, idf=newmodel, fname=report
```

### Send a group of files

#### Send files that are recorded in a list

Use the appropriate indirection character (@ or \#) to send all files that are listed in a specified file. Add the indirection character at the beginning of the complete file name, including path as shown in the following syntax.

```
send part=store1, idf=newmodel, fname=@pub/sourcefiles/list
```

> **Note**
>
> Note: The indirection character means that you are reading the file as if it contains file. Be careful that the file you are using contains a list of files.

For more information, see [Use an indirection file](send_group_of_files_cl#Sending_files_designated_by_an_indirection_file).

#### Send files that match a mask

You can use mask characters to send groups of files by adding either the ? character to indicate one character, or the \* to indicate multiple characters. Be sure to prefix the file name with the appropriate indirection character.

```
send part=store1, idf=newmodel, fname=@reports\*
```

#### Reference a folder

Specify the folder name and add the indirection character to send all files that are contained in a folder, where `daily_reports` is a folder.

```
send part=store1, idf=newmodel, fname=@daily_reports
```

#### Use selfname

You can use the `selfname `option to send only certain files from a given folder to a particular partner. Begin by creating a file that lists the specific files to send, where you use only the relative name.

Using the `send `command, include the `selfname `parameter, which is the name of the file you created. You then use the name of the folder containing the files (preceded by the indirection character) as the `fname`.

**Example**

In this example, the `report_folder` contains the following files A1, A2, A3, B1, and B2, and the `report_limited` file lists A1 and B1.

```
send part=store1, idf=newmodel, selfname=report_limited, fname=\#report_folder
```

Using the syntax in this example, only the files A1 and B1 are transferred.

See [SELFNAME](../../c_intro_userinterfaces/command_summary/parameter_intro/selfname).

### Send a list of the folder contents

This section describes how to send a list of files that are contained in a folder or folders. Note that it does not send the files themselves.

#### Use the folder name

If you specify only the folder name, this sends a list of the files contained in the folder.

```
send part=store1, idf=newmodel, fname=folder
```

#### Use file or folder name plus a mask

Add a mask character to the file or folder name to send a list of the files contained in the folder.

```
send part=store1, idf=newmodel, fname=folder/reports\*
```

### How to define the remote file name

#### Use the network file name

Use the NFNAME, network file name, to indicate the remote file name to be downloaded on the remote side. This feature is available only in open mode, as described in Read transfer implicit
send open mode.

```
send part=store1, idf=newmodel, nfname=remotefile
```

> **Note**
>
> Note: If you precede the NFNAME with an asterisk \*, the receiver can choose
> to keep the current transmitted name (in open mode) or rename the file.

<span id="Use3"></span>

Use scheduling features
-----------------------

### Trigger a send according to the transfer status

You can use the send command with STATE parameters so that the send occurs once the transfer has a predefined status.


| D = disp  | Transfers are carried out immediately. (default) |
| --- | --- |
| H = hold | A practical application for this method is to make files available for a partner to download when the partner is ready. This can be started by the START command or a RECV command. |
| K = keep | You can use this status to store several transfers, for example, until all are ready to go. A manual START command would trigger the transfers. |


```
send part=store1, idf=newmodel, state=hold
```

### Perform a send at a specific time

You can use the mintime and mindate parameters to schedule a transfer for a given date and time. These can be relative or absolute values; for more information see Delayed transfers in [Transfer scheduling](../transfer_command_overview/delayed_transfers).

```
send part=store1, idf=newmodel, mindate= 20150924, mintime=204500
```

### How to define a periodic send

You can use the cycle and tcycle parameters to define a periodic send. See [Cyclic transfer requests](../transfer_command_overview/cyclic_transfer_requests).

```
send part=store1, idf=newmodel, cycle=7, tcycle=day
```

### Define a send based on a transfer timeout

You can define that beyond a specified time in minutes, the transfer is canceled. See also [maxduration]().

```
send part=store1, idf=newmodel, maxduration=10
```

See also [Transfer
scheduling](../transfer_command_overview/delayed_transfers).

### Send a group of files in a single transfer

**Prerequisites**

- You can only use this mode between Transfer CFTs running the same platforms (Windows to Windows, for example)
- Set the uconf cft.server.force_heterogeneous_mode parameter value to yes if not already done
- Use the group of files syntax

Use the fname and wfname parameters to use this mode, also referred to as homogeneous mode.

```
send part=store1, idf=endofdayresults, fname=@myfolder/\*, wfname=results.zip
```

See also [WFNAME](../../c_intro_userinterfaces/command_summary/parameter_intro/wfname).
&lt;/p&gt;

> **Note**
>
> Note: Not managed by Central Unified Flow Management.

<span id="Execute"></span>

Execute pre and post transfer activities
----------------------------------------

For details transfer activities and scheduling, see [Processing execution policy](../about_transfer_processing/processing_exec_policy).

### How to trigger pre execution scripts

```
send part=store1, idf=model, preexec=exec/preexec.cmd
```

### How to trigger post execution scripts

```
send part=store1, idf=model, exec=exec/postexec.cmd
```

### How to trigger a script when a transfer fails

See [Transfer
error procedures](../about_transfer_processing/transfer_related_procedures) for more information.

```
send part=store1, idf=model, exece=exec/errorexec.cmd
```

> **Note**
>
> Note: You can use the _NONE_ keyword in send and receive commands to disable the EXECSF, EXECRF, EXECSE, EXECRE and EXECSFA procedures that are defined in the general Transfer CFT environment parameter settings (CFTPARM).

<span id="Use4"></span>

Use visibility features to link an application to a transfer
------------------------------------------------------------

When using visibility features to link an application to a transfer, or to link a transfer to other transfers, you can use the appobjid and the appcycid parameters. These parameters refer to tracked object and the cycle id.

```
send part=store1, idf=model, appobjid=appliobject, appcycid=Appl1
```
<span id="Miscella"></span>

Miscellaneous send features
---------------------------

### How to override defined transfer routing

Use the ipart parameter to override either the default or defined routing partner in the partner definition.

```
send part=store1, idf=model, ipart=siteB
```

See also [Store and forward](../transfer_command_overview/store_and_forward_mode_routing).

### How to toggle the protocol used for a send

In the partner definition create a choice of protocols, for example prot=(nossl, ssl), sap=(1761, 1762). Use the PROT parameter with the SEND command to indicate for that particular transfer it should use the specified protocol.

```
send part=store1, idf=model, prot=ssl
```

### How to provide passwords when required by the remote partner

You must provide both the user and corresponding password in send command as defined on the remote partner in the receive file template.

```
send part=store1, idf=model, ruser=guest, rpasswd=guestpassword
```

### Wait for a synchronous transfer to complete before continuing

After configuring the [synchronous communication](../../app_integration_intro/synch_comm_tcpip_intro), add the wstate and wtimeout to the send command.

```
config type=com, mediacom=tcpip, fname=xhttp://localhost:1765
send part=store1, idf=model, wstates=TX, wtimeout=120
```

### How to send additional information to the remote partner

Use the following parameters to further refine the send command and provide more information to the remote partner concerning a transfer

- parm = a free parameter that allows you to add information to send with your transfer (512 characters)
- sappl, rappl = send information about the applications sending the transfer
- suser, ruser = send information related to the transfer users

```
send part=store1, idf=model, parm='contains all daily reports'
```

Use the archive features
------------------------

When FACTION=ARCHIVE and ARCHIVEFNAME is set, the source file is moved to the file name specified in the ARCHIVEFNAME parameter when the transfer is completed.

```
send part=store1, idf=model, faction=archive, archivefname=&FNAME_&FDATE_&IDTU
```

****Limitations****

- The fname and archivefname must be on the same volume (all platforms)
- Faction=archive is not supported for:
    -   Implicit send (CFTSEND IMPL=YES)
    -   Homogeneous group of files
    -   Broadcasting
