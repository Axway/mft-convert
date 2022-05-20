---
title: "Deprecated folder monitoring (UCONF)"
linkTitle: "Deprecated folder monitoring (UCONF)"
weight: 220
---This section provides a description of how to use Transfer CFT UCONF values to manage folder monitoring. This method is no longer recommended; you should use the CFTFOLDER object method.

- [Configure folder monitoring using UCONF](#Configur)
- [How {{< TransferCFT/axwayvariablesComponentShortName >}} handles monitored files](#How2)
- [Modify and apply configuration changes](#Modifying_existing_configuration)
- [Directory configuration examples](#Director)
- [File-system event monitoring](#File-sys)

> **Note**
>
> There are two ways to implement Transfer CFT folder monitoring, either using UCONF or Transfer CFT objects. We recommend the CFTFOLDER method of configuring folder monitoring. Users that presently are using UCONF to manage folder monitoring can migrate to a CFTFOLDER configuration as described in Migrate to CFTFOLDER folder monitoring.

<span id="Configur"></span>

## Configure folder monitoring using UCONF

For each monitored directory you must provide a unique name to identify the set of configuration parameters corresponding to this directory. This enables you to individually configure each directory to be monitored.

### Step overview

1. Activate the folder monitoring option.
    -   Set uconf parameter folder_monitoring.enable to Yes.
1. Declare your logical directories to monitor.
    -   Add to your uconf parameter folder_monitoring.folders 1 logical name by root directory you want to monitor.
1. For each logical directory defined, configure the specific options you want to use for each:
    -   File management method

    <!-- -->

    -   Used sub-directories

    <!-- -->

    -   Set the IDF

    <!-- -->

    -   Set the partner name

    <!-- -->

    -   Define the delay to take into account the file

    <!-- -->

    -   Define other uconf Folder Monitoring parameters (described in the following sections)

<span id="Monitori"></span>

### Folder monitoring parameters

Use the following UCONF parameters to configure folder monitoring for each directory as needed. See the section [UCONF](../../../admin_intro/uconf) if you are not familiar with unified configuration settings.

****Parameter descriptions****


| UCONF parameter  | Type  | Default | Description  |
| --- | --- | --- | --- |
| folder_monitoring.enable  | Boolean  | No  |  • No: No folder monitoring occurs.<br/> • Yes: Enable {{< TransferCFT/axwayvariablesComponentShortName  >}} folder monitoring. |
| folder_monitoring.folders  | node  | None  | Add the logical folders to monitor (list of logical identifiers).<br/> You should provide a unique name to identify the set of configuration parameters corresponding to this directory. If you have more than one Folder to monitor, use a space between each logical value.<br/> See the **Comment***** below this table for additional information. |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.enable | Boolean  | Yes  | Enables a scan of the folder, where NO deactivates folder monitoring. |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.scan_dir | string  | None  | Absolute path name of the top level directory to scan.<br/> This directory must exist before restarting CFT.<br/> *See NOTE. |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.work_dir | string  | None  | Absolute path name of the top level directory available for file state information.<br/> • If you are using the MOVE method, files that are ready to be submitted are available in the work_dir.<br/> • If you are using the FILE method, the .met files are stored in the work_dir.<br/> <blockquote> **Note**<br/> Caution Never delete any .met files.<br/> </blockquote> *See NOTE. |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.enable_subdir | Boolean  | Yes  | Values:<br/> • Yes: The entire scan_dir sub-directory tree is monitored.<br/> • No: No scan is performed. |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.method | enum  | MOVE  | Values:<br/> • MOVE: Files are moved (by renaming), to the work_dir prior to being submitted.<br/> • FILE: Files are left in the scan_dir, and a state file with the same name is created in work_dir prior to submitting the file.<br/> See also [Configuring file tracking options (MOVE option)](../#Configur2). |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_idle_delay | integer  | 5  | If the state of a file has not changed within this delay in seconds, the file becomes a candidate for submission.  |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.idf | string  | ""  | The IDF name to use in the SEND command. Use one of the following:<br/> • A fixed name.<br/> • "(0)": The name of the first directory sub-level is used.<br/> • "(1)": The name of the second directory sub-level is used. |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.part | string  | ""  | The PART name to use in the SEND command. Use one of the following:<br/> • A fixed name.<br/> • "(0)": The name of the first directory sub-level is used.<br/> • "(1)": The name of the second directory sub-level is used. |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.interval | int  | 60  | The interval between two scans of the directory files in seconds.  |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_count | int  | -1  | Maximum number of file submissions for each scan. Using the default value indicates that there is no maximum. |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_size_min | int  | -1  | Files shorter than this value, in bytes, are not candidates for submission. Using the default value indicates that there is no lower limit on the file size.  |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_size_max | int  | -1  | Files larger than this value, in bytes, are not candidates for submission. Using the default value indicates that there is no upper limit on the file size.  |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_include_filter | string  | ""  | If this parameter is defined, only files whose names match this pattern are monitored.  |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_exclude_filter | string  | ""  | If this parameter is defined, files whose names match this pattern are not monitored.  |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.resubmit_changed_file | Boolean  | Yes  | This parameter has no effect when the configured method is MOVE.<br/> When the method parameter value is set to FILE:<br/> • Yes: When the state of a previously submitted file is seen as having changed, the file is submitted again.<br/> • No: Files are not resubmitted, regardless of changes.<br/> <blockquote> **Note**<br/> The file is resubmitted after any change regardless of if the modification is a small change, or purging and replacing the file with another file having the same name.<br/> </blockquote>  |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.filter_type | enum  | WILDMAT  | Defines the pattern matching algorithm to use for file name filtering. Values:<br/> • STRJCMP: The Transfer CFT pattern matching algorithm.<br/> • WILDMAT: A well known public domain algorithm, and is the default. **Unix/Windows only**<br/> See [Create inclusion and exclusion filters](../folder_customize#Defining) for details. |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.renaming_method | Enum  | TIMESTAMP  | This parameter applies only to the MOVE method.<br/> • NONE or " ": The filename is unchanged (no timestamp is added). If the file already exists in the work directory, the MOVE process fails.<br/> • TIMESTAMP: A timestamp having the format YYYYMMDDHHMMSS is added at the end of the name of the renamed file but before the last '.'.<br/> For example, using timestamp_separators=".": • myfile is renamed myfile.20131025<br/> • myfile.txt is renamed myfile.20131025.txt |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.renaming_separators | string  |   | This parameter only applies to the MOVE method. It must contain at most 2 characters from among the following:<br/> .[]()i_-<br/> The first character defines the separator before the timestamp. The second one, when present, defines the separator after the timestamp.<br/> For example, using timestamp_separators "[]": - myfile is renamed myfile.[20131025] - myfile.txt is renamed myfile.[20131025].txt |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.control | string  |   | Metadata used to control user changes.  |
| folder_monitoring.folders.<br/> &lt;logical_name&gt;.<br/> use_file_system_events<br/> [More information](#File-sys) | Boolean  | No  | Set to YES to enable the file system events monitoring service to detect newly available files.  |


> **Note**
>
> \*You cannot use the following characters in the SCANDIR or WORKDIR definition. Additionally you cannot use a comma (,) in the CFTFOLDER SCANDIR or WORKDIR definition.

- {{< TransferCFT/PrimaryForunix >}} /
- For {{< TransferCFT/PrimaryforWindows >}} \\ / : \* ? " &lt; > &#124;

******Comment\*\*\***: You can use CFTUTIL to create the list of folders &lt;logical_names>. When using CFTUTIL, be careful to correctly enter the command. For example, where FM1, FM2 and FM3 are 3 logical folders to be managed by {{< TransferCFT/axwayvariablesComponentShortName  >}}, enter:****

`CFTUTIL uconfset id= folder_monitoring.folders, value= "'FM1 FM2 FM3 '"         </code></p>`

## How {{< TransferCFT/axwayvariablesComponentShortName  >}} handles monitored files

This section describes how the various file monitoring parameters work.

### Parameter settings and actions

- The delay between scans of a given directory is defined by its interval parameter value.
- By default the ENABLESUBDIR [enable_subdir] parameter is set to YES, and the directory and all its sub-directories are scanned.
- For each file detected, the name is checked against the configured parameters values in the include and exclude file filters. Files that match the combined criteria are monitored, all others are ignored.

For a file to become a candidate to be submitted, the following conditions must be met:

- File size: If these values are configured, the following rules apply.
    -   FILESIZEMIN [file_size_min]: The current size must not be less than this value.
    -   FILESIZEMAX [file_size_max]: The current size must not be greater than this value.
- The last modification time and duration must not have changed within a number of seconds as defined in the FILEIDLEDELAY [file_idle_delay] parameter value.

<span id="Modifying_existing_configuration"></span>

## Create or modify a monitored folder and apply configuration changes

The act of starting Transfer CFT causes Transfer CFT to check for and reload configuration changes. Alternatively, you can dynamically execute the `CFTUTIL RECONFIG type=FOLDER` command to check and reload the configuration.

Upon reloading, if there are any modified configuration parameters or detected errors in the new configuration, Transfer CFT records these in the log. Additionally, Transfer CFT verifies that the updated configuration is compatible with the contents of the current directories.

In particular, if you change the METHOD parameter from FILE to MOVE without modifying the scan_dir and work_dir parameters, and if the work_dir directory is not empty, Transfer CFT displays an error message in the log and will not monitor the corresponding directory.

To deactivate compatibility checks of a folder’s new configuration, unset the value of the `folder_monitoring.folders.<logical_name>.control `parameter using the `uconfunset `command.

**Example**

If the folder's logical name is **A**, execute the following command prior to the reconfiguration (or start) command:

```
CFTUTIL UCONFUNSET id = folder_monitoring.folders.A
.control
```

When you then reconfigure (or start) Transfer CFT, the **A** folder is not checked.

```
CFTUCONF RECONFIG type=folder
```
<span id="Director"></span>

## Directory configuration examples

This section presents an example that consists of configuring 3 directories for monitoring, each having a different set of configuration parameter values. In this example, the three different directories are called A, B, and C.

Note that the configuration parameter folder_monitoring must contain a list with these directory names, separated by blanks. Additionally, you must enable the folder monitoring functionality.

> **Note**
>
> In all of the examples in this topic, you must enter CFTUTIL in upper case.

For this example, you would execute the following command:

```
CFTUTIL uconfset id=folder_monitoring.enable , value='Yes'
CFTUTIL uconfset id=folder_monitoring.folders , value= 'A B C'
\*Note that the "' '"characters are used to protect the spaces between each folder monitoring nodes declarations.
```

> **Note**
>
> All of the examples in this section were written for a UNIX platform. Modify to suit your environment accordingly.

#### Directory A requirements

The first directory presents the simplest possible configuration, leaving most parameters set to their default values.

- All of the files in the directory sub-tree are candidates for the SEND submission.
- The files are sent to a given partner, newyork, using an IDF name of IDFA.

The following commands create the configuration defined for directory A.

```
#
# Create all of the needed directories (UNIX platform example)
#
mkdir /home/CFT/fm/dir_a
mkdir /home/CFT/fm/dir_a/scan
mkdir /home/CFT/fm/dir_a/work
#
# Define the needed Transfer CFT configuration parameters leaving all others set to their default value.
#
CFTUTIL uconfset id=folder_monitoring.folders.A.scan_dir , value='/home/CFT/fm/dir_a/scan'
CFTUTIL uconfset id=folder_monitoring.folders.A.work_dir , value='/home/CFT/fm/dir_a/work'
CFTUTIL uconfset id=folder_monitoring.folders.A.part , value='NEWYORK'
CFTUTIL uconfset id=folder_monitoring.folders.A.idf , value='IDFA'
```

#### Directory B requirements

For the second directory, directory B, we want to:

- Be able to send files to the following partners, newyork, berlin, london, rome, brussels, and paris.
- Use the id given as the IDF, in this example TXT.
- Send only files suffixed by .txt.

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
# Define all of the needed Transfer CFT configuration parameters, while leaving the others set to their default value.
#
CFTUTIL uconfset id=folder_monitoring.folders.B.scan_dir , value='/home/CFT/fm/dir_b/scan'
CFTUTIL uconfset id=folder_monitoring.folders.B.work_dir , value='/home/CFT/fm/dir_b/work'
CFTUTIL uconfset id=folder_monitoring.folders.B.part , value='(0)'
CFTUTIL uconfset id=folder_monitoring.folders.B.idf , value='TXT'
CFTUTIL uconfset id=folder_monitoring.folders.B.file_include_filter , value='\*.txt'
```

The files to be sent must be moved to the directory that corresponds to the destination partner name, for example `/home/CFT/fm/dir_b/newyork` for the partner named newyork.

#### Directory C requirements

For the third directory, directory C, we want to:

- Be able to send files to multiple partners, newyork and paris.
- Use idf1, idf2, or idf3 as the newyork partner IDF.
- Use idfa, idfb, idfc, or idfd as the paris partner IDF.
- Not send files suffixed by .tmp.
- Automatically move the files to be sent to the scan_dir, so the file_idle_delay parameter value is set to zero.
- Submit files within a delay of approximately 10 seconds (interval).
- Limit the number of send submissions per interval to 4 (file_count).

The following commands create the described directory C configuration.

```
#
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
# Define all necessary Transfer CFT configuration parameters leaving others set to their default value.
#
CFTUTIL uconfset id=folder_monitoring.folders.C.file_idle_delay , value='0'
CFTUTIL uconfset id=folder_monitoring.folders.C.idf , value='(1)'
CFTUTIL uconfset id=folder_monitoring.folders.C.part , value='(0)'
CFTUTIL uconfset id=folder_monitoring.folders.C.scan_dir , value='/home/CFT/fm/dir_c/scan'
CFTUTIL uconfset id=folder_monitoring.folders.C.work_dir , value='/home/CFT/fm/dir_c/work'
CFTUTIL uconfset id=folder_monitoring.folders.C.interval , value='10'
CFTUTIL uconfset id=folder_monitoring.folders.C.file_count , value='4'
CFTUTIL uconfset id=folder_monitoring.folders.C.file_exclude_filter , value='\*.tmp'
```

The files to be sent must be moved to the directory that corresponds to the destination partner and idf names, for example /home/CFT/fm/dir_c/newyork/idf1 for the partner newyork and idf idf1.

<span id="Folder"></span>

## Folder monitoring using the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI

From the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI, select the ****Unified Configuration**** icon. From the Unified configuration dialog box, select folder_monitoring. To display all possible parameters, first create at least one folder (folder_monitoring.folders) for {{< TransferCFT/axwayvariablesComponentShortName  >}} to monitor, and click **Apply**.

For more information on setting unified configuration parameters, refer to [Using UCONF in CFTUTIL](../../../admin_intro/uconf/uconf_w_cftutil) or [About the unified configuration](../../../admin_intro/uconf) topics.

<span id="File-sys"></span>

## File-system event monitoring

This feature enables you to use file-system events monitoring to detect newly available files for an immediate Transfer CFT action.

****Available on Linux/Windows only****

See [Supported OS for file-system event monitoring](../#Supporte).

### Configure file-system event monitoring

Set the following UCONF parameters as shown below. When you set this option for a specific folder, Transfer CFT immediately treats any events that occur in this folder's SCAN directory.

```
CFTUTIL uconfset id=folder_monitoring.folders.MyFolder.use_file_system_events, value=YES
```

#### Attention

This feature can be resource intensive for Transfer CFT and the system in general in the following situations:

- You have a large number of directories and sub-directories monitored using file-system events.
- The activity in terms of file additions, removals, changes of files in those directories is high.

We recommended that you only use file-system event monitoring when immediate attention by Transfer CFT is a functional requirement.

****Related topics****

- [Introduction to folder monitoring](../)
- [Folder monitoring CFTFOLDER](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftfolder)
- [Migrate to CFTFOLDER folder monitoring](../migrate_uconf_cftfolder)
- [Create inclusion and exclusion filters](../folder_customize)
