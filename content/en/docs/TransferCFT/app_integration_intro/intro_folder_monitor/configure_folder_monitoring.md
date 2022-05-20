---
title: "Folder monitoring CFTFOLDER"
linkTitle: "Folder monitoring CFTFOLDER"
weight: 180
---This section provides a description of how to use Transfer CFT objects to manage folder monitoring.

> **Note**
>
> There are two ways to implement Transfer CFT folder monitoring, either using UCONF or Transfer CFT objects. We recommend the CFTFOLDER method of configuring folder monitoring. Users that presently are using UCONF to manage folder monitoring can migrate to a CFTFOLDER configuration as described in Migrate to CFTFOLDER folder monitoring.

## Folder monitoring set up

1. Activate the folder monitoring option.
    *   Set uconf parameter folder_monitoring.enable to ****Yes****.
1. Declare your logical directories to monitor.
    *   Add CFTFOLDER objects.
1. For each logical directory defined, configure the specific options you want to use for each:
    *   File management method

    <!-- -->

    *   Used sub-directories

    <!-- -->

    *   Set the IDF

    <!-- -->

    *   Set the partner name

    <!-- -->

    *   Define the delay to take into account the file

    <!-- -->

    *   Define other Folder Monitoring parameters
1. Optionally, to specify file-system event monitoring you additionally must set this in [CFTFOLDER object.](#Enable)

<span id="CFTFOLDE"></span>

## CFTFOLDER object

The following table describes the parameters you can use to define the CFTFOLDER object. Additionally the table lists the UCONF equivalent for users who have opted for a polling type of folder monitoring.

<span id="Folder"></span>

### Folder monitoring parameters

Use the following CFTFOLDER parameters to configure folder monitoring for each directory as needed.

****Parameter descriptions****


| <span id="Paramete"></span>Parameter  | Type  | Default  | Description  |
| --- | --- | --- | --- |
| same as in UCONF<br/> &lt;folder_monitoring.enable&gt; | Boolean  | No  |  • No: No folder monitoring occurs.<br/> • Yes: Enable {{< TransferCFT/axwayvariablesComponentShortName  >}} folder monitoring. |
| ID<br/> **Mandatory** | node  | None  | Add the logical folders to monitor (list of logical identifiers).<br/> You should provide a unique name to identify the set of configuration parameters corresponding to this directory. If you have more than one Folder to monitor, use a space between each logical value. |
| STATE  | Boolean  | Active  | Enables a scan of the folder.<br/> <blockquote> **Note**<br/> NO = NOACTIVE.<br/> </blockquote>  |
| SCANDIR<br/> *Mandatory*  | string  | None  | Absolute path name of the top level directory to scan. You can use an environmental variable with the syntax <code>%env:&lt;variable&gt;%</code> however note that this variable is replaced with the actual value at the time you define the object.<br/> This directory must exist before restarting Transfer CFT.<br/> *See [NOTE](#*char_note). |
| WORKDIR<br/> *Mandatory*  | string  | None  | Absolute path name of the top level directory available for file state information. You can use an environmental variable with the syntax <code>%env:&lt;variable&gt;%</code> however note that this variable is replaced with the actual value at the time you define the object.<br/> • If you are using the MOVE method, files that are ready to be submitted are available in the work_dir.<br/> • If you are using the FILE method, the .met files are stored in the work_dir.<br/> <blockquote> **Note**<br/> Caution Never delete any .met files.<br/> </blockquote> *See [NOTE](#*char_note). |
| ENABLESUBDIR  | Boolean  | Yes  | Values:<br/> • Yes: The entire scan_dir sub-directory tree is monitored.<br/> • No: No scan is performed. |
| <span id="METHOD"></span>METHOD  | enum  | MOVE  | Values:<br/> • MOVE: Files are moved to the work_dir prior to being submitted.<br/> • FILE: Files are left in the scan_dir, and a state file with the same name is created in work_dir prior to submitting the file.<br/> <blockquote> **Note**<br/> Before changing the method from FILE to MOVE, you should remove all files (metadata .met files) located in the associated working directory.<br/> </blockquote> <blockquote> **Note**<br/> Changing the method from MOVE to FILE, deletes all files located in the associated working directory. Therefore, we recommend removing all files from the scan and working directory before changing the METHOD type.<br/> </blockquote> Please see the [Limitations](../#Limitati) for multi-host system recommendations. |
| ARCHIVEDIR  | String  |   | Archive directory where a source file is moved to after a successful transfer. This means the source file is moved from the WORKDIR for METHOD=MOVE or SCANDIR for METHOD=FILE to the ARCHIVEDIR.  |
| FILEIDLEDELAY  | integer  | 5  | If the state of a file has not changed within this delay in seconds, the file becomes a candidate for submission.  |
| GROUPID  | String  |   | Complementary information for the USERID. Maximum length 32 characters.  |
| IDF<br/> *Mandatory* | string  | ""  | The IDF name to use in the SEND command. Use one of the following:<br/> • A fixed name.<br/> • "(0)": The name of the first directory sub-level is used.<br/> • "(1)": The name of the second directory sub-level is used.<br/> <blockquote> **Note**<br/> In the Directory C example/home/CFT/fr/dir_c/scan/newyork/idf1, the (0) represents newyork, and (1) represents idf1.<br/> </blockquote>  |
| PART<br/> *Mandatory*  | string  | ""  | The PART name to use in the SEND command. Use one of the following:<br/> • A fixed name.<br/> • "(0)": The name of the first directory sub-level is used.<br/> • "(1)": The name of the second directory sub-level is used.<br/> <blockquote> **Note**<br/> In the Directory C example/home/CFT/fr/dir_c/scan/newyork/idf1, the (0) represents newyork, and (1) represents idf1.<br/> </blockquote>  |
| INTERVAL  | int  | 60  | The interval between two scans of the directory files in seconds.  |
| FILECOUNT  | int  | 0  | Maximum number of file submissions for each scan. Using the default value indicates that there is no maximum. |
| FILESIZEMIN  | int  | 0  | Files shorter than this value, in bytes, are not candidates for submission. Using the default value indicates that there is no lower limit on the file size. |
| FILESIZEMAX  | int  | 0  | Files larger than this value, in bytes, are not candidates for submission. Using the default value indicates that there is no upper limit on the file size. |
| INCLUDEFILTER  | string  | ""  | If this parameter is defined, only files whose names match this pattern are monitored.  |
| EXCLUDEFILTER  | string  | ""  | If this parameter is defined, files whose names match this pattern are not monitored.  |
| RESUBMITCHANGED  | Boolean  | Yes  | This parameter has no effect when the configured method is MOVE.<br/> When the method parameter value is set to FILE:<br/> • Yes: When the state of a previously submitted file is seen as having changed, the file is submitted again.<br/> • No: Files are not resubmitted, regardless of changes.<br/> <blockquote> **Note**<br/> The file is resubmitted after any change regardless of if the modification is a small change, or purging and replacing the file with another file having the same name.<br/> </blockquote>  |
| FILTERTYPE  | enum  | WILDMAT  | Defines the pattern matching algorithm to use for file name filtering.<br/> Values:<br/> • STRJCMP: The Transfer CFT pattern matching algorithm.<br/> • WILDMAT: A well known public domain algorithm, and is the default. **Unix/Windows only**<br/> • EREGEX: Extended regular expression syntax.<br/> See [Create inclusion and exclusion filters](../folder_customize#top) for details. |
| RENAMEMETHOD  | Enum  | TIMESTAMP  | This parameter applies only to the MOVE method. When set to TIMESTAMP, a timestamp of the pattern YYYYMMDDHHMMSS is added at the end of the name of the renamed file but before the last '.'.<br/> For example, using timestamp_separators=".": • myfile is renamed myfile.20131025<br/> • myfile.txt is renamed myfile.20131025.txt<br/> <blockquote> **Note**<br/> Unset the default value and use " " to MOVE without adding a timestamp.<br/> </blockquote>  |
| RENAMESEPARATOR  | string  | OpenVMS:<br/> " _ "<br/> All other OS:<br/> "." | This parameter only applies to the MOVE method.<br/> You can use no more than two characters from among the following:<br/> .[]()_-<br/> The first character defines the separator before the timestamp. The second one, when present, defines the separator after the timestamp.<br/> For example, using timestamp_separators "[]": - myfile is renamed myfile.[20131025] - myfile.txt is renamed myfile.[20131025].txt |
| N/A in this version  | string  |   | Metadata used to control user changes.  |
| USEFSEVENTS<br/> <br/> [More information](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftfolder) | Boolean  | No  | Set to YES to enable the file system events monitoring service to detect newly available files.  |
| [Folder monitoring using USERCTRL](../../../c_intro_userinterfaces/command_summary/parameter_intro/userid). |


> **Note**
>
> \*You cannot use the following characters in the SCANDIR or WORKDIR definition. Additionally you cannot use a comma (,) in the CFTFOLDER SCANDIR or WORKDIR definition.

* {{< TransferCFT/PrimaryForunix >}} /
* For {{< TransferCFT/PrimaryforWindows >}} \\ / : \* ? " &lt; > &#124;

### Parameter settings and actions

* The delay between scans of a given directory is defined by its interval parameter value.
* By default the ENABLESUBDIR [enable_subdir] parameter is set to YES, and the directory and all its sub-directories are scanned.
* For each file detected, the name is checked against the configured parameters values in the include and exclude file filters. Files that match the combined criteria are monitored, all others are ignored.

For a file to become a candidate to be submitted, the following conditions must be met:

* File size: If these values are configured, the following rules apply.
    *   FILESIZEMIN [file_size_min]: The current size must not be less than this value.
    *   FILESIZEMAX [file_size_max]: The current size must not be greater than this value.
* The last modification time and duration must not have changed within a number of seconds as defined in the FILEIDLEDELAY [file_idle_delay] parameter value.

## Create or modify a CFTFOLDER object

The act of starting Transfer CFT causes Transfer CFT to check for and reload configuration changes. Alternatively, you can dynamically execute the `CFTUTIL RECONFIG type=FOLDER` command to check and reload the configuration.

Upon reloading, if there are any modified configuration parameters or detected errors in the new configuration, Transfer CFT records these in the log. Additionally, Transfer CFT verifies that the updated configuration is compatible with the contents of the current directories.

In particular, if you change the METHOD parameter from FILE to MOVE without modifying the scan_dir and work_dir parameters, and if the work_dir directory is not empty, Transfer CFT displays an error message in the log and will not monitor the corresponding directory.

If you create or modify a folder while Transfer CFT is running, you must execute the ACT command to reload the configuration.

****Example****

The following command reloads the FM40 configuration.

```
ACT ID=FM40, type=FOLDER
```
<span id="Enable"></span>

## Enable the file-system event monitoring

This feature enables you to use file-system events monitoring to detect newly available files for an immediate Transfer CFT action.

****Available on Linux/Windows only****

See [Supported OS for file-system event monitoring](../#Supporte).

To enable file-system event monitoring modify as follows:

```
CFTFOLDER ID=<myfolderobject>, USEFSEVENTS=YES, ...
```

#### Attention

This feature can be resource intensive for Transfer CFT and the system in general in the following situations:

* You have a large number of directories and sub-directories monitored using file-system events.
* The activity in terms of file additions, removals, changes of files in those directories is high.

We recommended that you only use file-system event monitoring when immediate attention by Transfer CFT is a functional requirement.

<span id="Activate"></span>

## Activate and deactivate folder monitoring

To turn the file-system event monitoring off for a given folder object, use the command:

```
INACT TYPE=FOLDER,ID=<myfolderobject>
```

To turn on the file-system event monitoring for a given folder object, use the command:

```
ACT TYPE=FOLDER,ID=<myfolderobject>
```

To view all of the CFTFOLDER objects, use the command:

```
LISTPARM TYPE=FOLDER
```

To extract the folder objects, use the command:

```
CFTEXT TYPE=FOLDER
```

## Remove or change a folder

### Change a folder

If Transfer CFT is running and you create or change a folder or multiple folders, you can execute RECONFIG TYPE=FOLDER so that all changes are taken into account.

### Delete a folder

Prior to deleting a folder object, check that it is inactive. You can execute INACT on this folder if unsure.

```
INACT TYPE=FOLDER, ID=<myfolder>
CFTFOLDER ID=<myfolder>, MODE=DELETE
```

> **Note**
>
> If you delete an active folder object while Transfer CFT is running without first deactivating the folder (INACT), you must execute a RECONFIG TYPE=FOLDER. If you do not, the folder remains active.

## Directory configuration examples

This section presents an example that consists of configuring 3 directories for monitoring, each having a different set of configuration parameter values. In this example, the three different directories are called A, B, and C.

> **Note**
>
> All of the examples in this section were written for a UNIX platform. Modify to suit your environment accordingly.

### Directory A requirements

The first directory presents the simplest possible configuration, leaving most parameters set to their default values.

* All of the files in the directory sub-tree are candidates for the SEND submission.
* The files are sent to a given partner, `newyork`, using an IDF name of IDFA.

The following commands create the configuration defined for directory A.

```
#
# Create all of the needed directories (UNIX platform example)
#
mkdir /home/CFT/fm/dir_a
mkdir /home/CFT/fm/dir_a/scan
mkdir /home/CFT/fm/dir_a/work
#
CFTUTIL CFTFOLDER ID=A, SCANDIR='/home/CFT/fm/dir_a/scan', WORKDIR='/home/CFT/fm/dir_a/work', PART='NEWYORK', IDF='IDFA'
```

### Directory B requirements

For the second directory, directory B, we want to:

* Be able to send files to the following partners, newyork, berlin, london, rome, brussels, and paris.
* Use the id given as the IDF, in this example TXT.
* Send only files suffixed by .txt.

The following commands create the required directory B configuration.

```
#
# Create all needed directories (example for UNIX platforms)
#
mkdir /home/CFT/fm/dir_b
mkdir /home/CFT/fm/dir_b/scan
mkdir /home/CFT/fm/dir_b/work
mkdir /home/CFT/fm/dir_b/scan/newyork
mkdir /home/CFT/fm/dir_b/scan/berlin
mkdir /home/CFT/fm/dir_b/scan/london
mkdir /home/CFT/fm/dir_b/scan/rome
mkdir /home/CFT/fm/dir_b/scan/brussels
mkdir /home/CFT/fm/dir_b/scan/paris
#
CFTUTIL CFTFOLDER ID=B, SCANDIR='/home/CFT/fm/dir_b/scan', WORKDIR='/home/CFT/fm/dir_b/work', PART='(0)', IDF='TXT', INCLUDEFILTER='\*.txt'
```

The files to be sent must be moved to the directory that corresponds to the destination partner name, for example `/home/CFT/fm/dir_b/newyork `for the partner named `newyork`.

### Directory C requirements

For directory C we want to:

* Send files to multiple partners, newyork and paris.
* Use idf1, idf2, or idf3 as the newyork partner IDF.
* Use idfa, idfb, idfc, or idfd as the paris partner IDF.
* Not send files suffixed by .tmp.
* Automatically move the files to be sent to the SCANDIR, so the FILEIDLEDELAY parameter value is set to zero.
* Submit files within a delay of approximately 10 seconds (INTERVAL).
* Limit the number of send submissions per interval to 4 (FILECOUNT).

The following commands create the described directory C configuration.

```
#<span id="#example_for_description"></span>
# Create all needed directories (example for UNIX platforms)
#
mkdir /home/CFT/fm/dir_c
mkdir /home/CFT/fm/dir_c/scan
mkdir /home/CFT/fm/dir_c/work
mkdir /home/CFT/fm/dir_c/scan/newyork/idf1
mkdir /home/CFT/fm/dir_c/scan/newyork/idf2
mkdir /home/CFT/fm/dir_c/scan/newyork/idf3
mkdir /home/CFT/fm/dir_c/scan/paris/idfa
mkdir /home/CFT/fm/dir_c/scan/paris/idfb
mkdir /home/CFT/fm/dir_c/scan/paris/idfc
mkdir /home/CFT/fm/dir_c/scan/paris/idfd
#
CFTUTIL CFTFOLDER ID=C, FILEIDLEDELAY='0', PART='(0)', IDF='(1)', SCANDIR='/home/CFT/fm/dir_c/scan',
WORKDIR='/home/CFT/fm/dir_c/work', INTERVAL='10', FILECOUNT='4', FILEEXCLUDEFILTER='\*.tmp'
```

The files to be sent must be moved to the directory that corresponds to the destination partner and idf names, for example /home/CFT/fm/dir_c/newyork/idf1 for the partner `newyork `and idf `idf1`.

<span id="Customiz"></span>

## Customize SEND with folder monitoring

You can customize the transfer related metadata, such as the IDA, PARM, SUSER, etc. using the file name value. This allows folder monitoring to provide a functionality that resembles a SEND command.

For example, you have a file named `A0001.appli1.cft.XXX` in the `scan1 `folder, and you have the following two objects in your configuration:

```
CFTFOLDER id=folder1,scandir=scan1,idf=idf1,par=part1,...CFTSEND id=idf1,ida=&%.1froot,sappl="&%.2froot",suser="&%.3froot",...
```

Consequently, Transfer CFT automatically creates a request based on the above syntax:

```
SEND part=part1,idf=idf1,ida=A0001,sappl="appli1",suser="cft"
```

* For more information on how to effectively use separators with symbolic variables, please see [Separate fields in symbolic variables.](../../../c_intro_userinterfaces/command_summary/symbolic_variables#Separate)
* For more information on the various symbolic variables to use in CFTSEND with CFTFOLDER, see [List of symbolic variables.](../../../c_intro_userinterfaces/command_summary/symbolic_variables#List_of_symbolic_variables)

## Archiving with folder monitoring

Archiving is a way to store files after they have been moved from the scanned folder to the receiving folder.

1. A file is deposited in the folder to be scanned.  
    ![](/Images/TransferCFT/folder5.png)
1. Depending on the folder monitoring method you have enabled, the file is picked up and moved to the working directory.  
    ![](/Images/TransferCFT/folder6.png)
1. The file transfer and any related processing occur and the file is then moved to the storage folder.  
    ![](/Images/TransferCFT/folder7.png)

### Use case scenario

The following example describes how to scan a folder, send any new file, and then store a backup of the file. Additionally, the scenario provides instructions on how to then rename the transferred file as it is stored in the backup folder.

> **Note**
>
> If you create a new transfer with the same name as a previous file, it overwrites the existing file in the archive folder.

### Prerequisites

Create the following:

* A folder to scan, for example: `mkdir myScanFolder`
* A working folder, for example: `mkdir myWorkFolder`
* An archive folder, for example: `mkdir myArchiveFolder`
* A test file, for example: `myFile.txt`
* Folder monitoring must be enabled: `uconfset id=folder_monitoring.enable, value=yes`
* Stop {{< TransferCFT/suitevariablesTransferCFTName >}} prior to start the procedure, enter: `cft stop `

In the **Steps** below, we use the absolute paths, that is, the folders are located in the runtime directory.

### Archive with no file renaming afterward

1. Create the CFTFOLDER object.  
1. ```
    cftfolder id=app1, PART=paris, idf=myfile, scandir=MyScanFolder, workdir=MyWorkFolder, renamemethod=none, archivedir=MyArchiveFolder,method=move,interval=1,fileidledelay=0
    ```
1. Activate the new CFTFOLDER object:  
1. ```
    ACT type=folder, id=app1
    ```
1. Start Transfer CFT: `cft start`
1. Put the test file in the `runtime/MyScanFolder` folder.
1. Navigate to the `runtime/MyArchivedFolder` and check that the `MyFile.txt` is stored there.
1. Check in the log for a message similar to the following:  
    ```
    CFTT89I Faction on FNAME=MyWorkFolder\\MyFile.txt archived as MyArchivedFolder\\MyFile.txt <IDTU=A000000F PART=PARIS IDF=MYFILE IDT=C1918185>
    ```

### Archive and rename in the archive folder

1. Create the CFTFOLDER object.  

1. ```
    cftfolder id=app1, PART=paris, idf=MyFile, SCANDIR=MyScanFolder, workdir=MyWorkFolder, renamemethod=none, archivedir=MyArchivedFolder,method=move,interval=1,fileidledelay=0
    ```

1. Create a SEND model using `archivefname`. In this example the transfer's IDTU is appended on the filename:

    ```
    CFTSEND id=MYFILE, archivefname=&FROOT&(-.)FSUF_&IDTU, faction=ARCHIVE
    ```

1. Activate the new CFTFOLDER object:  

1. ```
    ACT type=folder, id=app1
    ```

1. Start Transfer CFT: `cft start`

1. Put the test file in the `runtime/MyScanFolder` folder.

1. Navigate to the `runtime/MyArchiveFolder` and check that the `MyFile.txt` is stored there.

1. Check in the log for a message similar to the following:  
    ```
    CFTT89I Faction on FNAME=MyWorkFolder\\MyFile.txt archived as MyArchiveFolder\\MyFile.txt_A000000F <IDTU=A000000F PART=PARIS IDF=MYFILE IDT=C1918185>
    ```

<span id="Folder2"></span>

### Folder monitoring using USERCTRL

The following example demonstrates how to use the [USERCTRL](../../../c_intro_userinterfaces/command_summary/parameter_intro/userctrl) parameter to define access control for another user. See the sections on user rights in [How to enable system users - Windows](../../../cft_intro_install/unix_install_start_here/run_first_time_ux/run_first_time_ux/t_adding_system_user_unix) for details.

> **Note**
>
> USEFSEVENTS=YES is not supported on UNIX systems in this use case.

1. Enable USERCTRL in the CFTPARM command:  
    ```
    CFTPARM ID=IDPARM0,…,USERCTRL=YES
    ```
1. Start Transfer CFT with `usercft `as the user.
1. User1 creates the `/home/user1/scandir_app1` and `/home/user1/workdir` folders:  
    ```
    mkdir /home/user1/scandir_app1
    mkdir /home/user1/workdir
    ```
1. `App1 `writes files to this specific scandir folder, e.g. `/home/user1/scandir_app1`, which belongs to user1. Notice that the usercft user does not have access to`  scandir_app1`.
1. From CFTUTIL, create a CFTFOLDER:  
    ```
    cftfolder id=app1, PART=paris, idf=app1, SCANDIR=/home/user1/scandir_app1 ,userid=user1, workdir=/home/user1/workdir, method=move, interval=10,fileidledelay=0
    ```
1. Activate the CFTFOLDER object:  
    ```
    act type=folder,id=app1
    ```
1. Create a file called` Myfile.txt`, and copy it to the `/home/user1/scandir_app1` folder.

****Results****

The  transfer is executed on the behalf of user1. Notice that there is a message indicating that the folder is activated on behalf of the specified user.

```
CFTR20I folder "/home/user1/scandir_app1" registered as nickname <APP1>
CFTR20I on behalf of "user1" user
```
