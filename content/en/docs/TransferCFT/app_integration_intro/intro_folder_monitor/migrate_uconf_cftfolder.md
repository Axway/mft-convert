---
title: "Migrate to CFTFOLDER folder monitoring"
linkTitle: "Migrate to CFTFOLDER folder monitoring"
weight: 210
---You can use the ****Folder monitoring migration**** **utility** to help you migrate from the UCONF method of folder monitoring to the CFTFOLDER method. This procedure may be useful if you have migrated or upgraded from Transfer CFT 3.1.2 to Transfer CFT 3.2.4.

Reasons to switch to the CFTFOLDER method include:

* Folder monitoring is easier to manage via the CFTFOLDER object
* The UCONF method has a limitation on the number of logical folders that you can monitor

## Procedure

Using the `cftmifm` migration command creates a text file that the CFTUTIL utility can use to migrate the folder definitions, where the CFTFOLDER definitions are an extraction from the UCONF definitions.

You must stop and start, or restart, Transfer CFT for the modifications to be taken into account.

### Migration command

The folder migration utility is supplied as an executable file, and displays help text when invoked without arguments or options.

****Actions****

Two actions are available, you can extract the folder definitions to migrate, or purge the UCONF folder definitions after migrating.

`cftmifm migrate &#124; purge `

****Options****


| Option  | Description  |
| --- | --- |
| -s  | Simulate. No action is taken, simply displays all actions that should be taken.  |
| -f  | Force. Ignores detected errors.<br/> By default, processing is canceled when error conditions are encountered.<br/> When the -f option is set, only fatal errors prevent processing. |
| -p  | Pattern. The folder selection pattern.<br/> Searches for corresponding folder candidates, allowing you to specify a subset of the folders that must be processed.<br/> The pattern syntax is the same as the STRJCMP filter type used by CFTFOLDER. |
| -t  | Level. The trace level from 0..4. The lower the level, the fewer the messages that are displayed. The default value is 1.  |
| -w  | Width. The maximum line width for displayed screen messages, between 80..500, where the default is 130.  |
| -o  | Fname. The output file for the CFTFOLDER object containing the definitions required for migration. To create the migrated folders in Transfer CFT configuration, you must interpret the generated output file using CFTUTIL.  |


> **Note**

* You must enter each option or argument separately.
* The fname is ignored when using the purge command, or when using the (-s) simulation option.

### How the migration utility works

The Transfer CFT migration utility performs the following steps:

1. Builds a list all the folder names defined in uconf.
1. If a pattern was supplied, it restricts the list to nicknames that match this pattern.
1. Checks that all names have the correct syntax and are all different.
1. Removes any previously migrated or moved folders from the list.
1. For each folder of the list, check if it can be migrated to Transfer CFT configuration.
1. Optionally when using -s (simulate), it stops here and does not process.

### How the purge works

The Transfer CFT purge utility performs the following steps:

1. Builds a list all the folder names defined in uconf.
1. If a pattern was supplied, restricts the list to nicknames that match this pattern.
1. Checks that all names have the correct syntax and are all different.
1. When folders in the list also exist as CFTFOLDER objects in the Transfer CFT configuration, and STATE=ACTIVE, these folders are targeted to be purged.
1. At this point the purge is performed unless you have specified - s (simulate).

* The purge removes all targeted folders from the folder_monitoring.folders listed in UCONF. As opposed to the migration, this action directly alters the UCONF configuration.
* If you are using -s (simulate), the tool only displays folders targeted by the purge.

## Examples of how to use the migrate and purge commands

### Examples on Unix or Windows systems

****Example 1****

Migrate all folders from UCONF to CFTFOLDER objects.

1. Create CFTFOLDER objects in fm1.cfg file:  
    `cftmifm migrate -o fm1.cfg`
1. Save uconf folder configuration:  
    `CFTUTIL CFTEXT type=uconf,id=folder_monitoring.*,fout=fm_uconf_save.cfg`
1. Interpret fm1.cfg file:  
    `CFTUTIL config type=input,fname=fm1.cfg`
1. Purge uconf folder configuration:  
    `cftmifm purge`

****Example 2****

Only migrate specific folders from the UCONF configuration to the CFTFOLDER option, for example, select all logical folders starting with the letter "A".

1. Create the CFTFOLDER objects in a file called fm2.cfg, for example:  
    `cftmifm migrate -p A* -o fm2.cfg`
1. Make a backup save of the UCONF folder configuration:  
    `CFTUTIL CFTEXT type=uconf,id=folder_monitoring.*,fout=fm_uconf_save.cfg`
1. Interpret the generated fm2.cfg file:  
    `CFTUTIL config type=input,fname=fm2.cfg`
1. Purge the UCONF folder configuration:  
    `cftmifm purge -p A*`

### Examples on an IBM i system

****Example 1****

Call the following programs to migrate from UCONF to CFTFOLDER objects.

```
CALL PGM(CFTMIFM) PARM('migrate' '-o' 'CFTPROD/fm1cfg')
CALL PGM(CFTUTIL) PARM(CFTEXT 'type=uconf,id=folder_monitoring.\*,fout=CFTPROD/fm_uconf')
CALL PGM(CFTUTIL) PARM(CONFIG 'type=input,fname=fm1cfg')
CALL PGM(CFT324CI/CFTMIFM) PARM('purge')
```

****Example 2****

Call the following programs to migrate specific folders from the UCONF configuration to the CFTFOLDER option. For example, select all logical folders starting with the letter "A".

```
CALL PGM(CFTMIFM) PARM('migrate' '-p' 'A\*' '-o' 'CFTPROD/fm2cfg')
CALL PGM(CFTUTIL) PARM(CFTEXT 'type=uconf,id=folder_monitoring.\*,fout=CFTPROD/fm_uconf')
CALL PGM(CFTUTIL) PARM(CONFIG 'type=input,fname=fm2cfg')
CALL PGM(CFTMIFM) PARM('purge' '-p' 'A\*')
```

### Examples on a z/OS system

Submit the CFTMIFM JCL located in the INSTALL library to migrate from UCONF to CFTFOLDER objects. Next submit the CFTMIFMP JCL, also located in the INSTALL library, to purge the UCONF configuration.

1. Extracts all folder definitions: `cftmifm migrate -p * -o <temp_file>`
1. Print the output.
1. Apply the extracted definitions: `CFTUTIL config type=input,fname=<temp_file>`
1. Purges the all existing UCONF folder definitions: `cftmifm purge -p *`

To  migrate or purge using a specific pattern you can modify the CFTMIFM and CFTMIFMP JCLs, otherwise all logical folders are affected.

### Rollback

A prerequisite to performing a rollback is that you must have made a backup of the UCONF folder configuration prior to having migrated. Using the example above, this is the step: `CFTUTIL CFTEXT type=uconf,id=folder_monitoring.*,fout=fm_uconf_save.cfg`

1. Interpret the backup uconf folder file, for example `fm_uconf_save.cfg`.  
    `CFTUTIL config type=input,fname=fm_uconf_save.cfg`
1. Manually remove all CFTFOLDER objects:  
    `CFTFOLDER ID=<folder>,mode=delete`

### Limitations and notes

* It is recommended, but you are not obliged, to purge the migrated UCONF folders. Therefore, if both CFTFOLDER and UCONF folders exists, the CFTFOLDER definitions takes precedence.
* Case sensitivity: Unlike UCONF, CFTFOLDER identifiers are not case sensitive - for example, a folder called "SamPle" is migrated as SAMPLE.
* Special characters: If folder names contain special characters (&”#{$€ …) or accents (éàù …), migration fails as the utility cannot read these. However, theses are correctly rewritten if they are part of the SCANDIR and WORKDIR parameters.
* Folder name length: The length of the folder name in CFTFOLDER cannot exceed 32 characters. If a UCONF defined folder name is too long, it cannot be migrated.

Parameter mapping and descriptions


| Parameter  | UCONF  | Type  | Default<br/> UCONF | Default <br/> CFTFOLDER | Description  |
| --- | --- | --- | --- | --- | --- |
| same as in UCONF | folder_monitoring.enable  | Boolean  | No  | No  |  • No: No folder monitoring occurs.<br/> • Yes: Enable {{< TransferCFT/axwayvariablesComponentShortName  >}} folder monitoring. |
| ID  | folder_monitoring.folders  | node  | None  | None  | Add the logical folders to monitor (list of logical identifiers).<br/> You should provide a unique name to identify the set of configuration parameters corresponding to this directory. If you have more than one Folder to monitor, use a space between each logical value.<br/> See the **Comment***** below this table for additional information. |
| STATE  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.enable | Boolean  | Yes  | Active  | Enables a scan of the folder.<br/> Note: NO = NOACTIVE. |
| SCANDIR  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.scan_dir | string  | None  | None  | Absolute path name of the top level directory to scan.<br/> This directory must exist before restarting Transfer CFT.<br/> *See NOTE. |
| WORKDIR  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.work_dir | string  | None  | None  | Absolute path name of the top level directory available for file state information. Do not use the same name as you are using for the corresponding SCANDIR.<br/> • If you are using the MOVE method, files that are ready to be submitted are available in the work_dir.<br/> • If you are using the FILE method, the .met files are stored in the work_dir.<br/> <blockquote> **Note**<br/> Caution Never delete any .met files.<br/> </blockquote> *See NOTE. |
| ENABLESUBDIR  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.enable_subdir | Boolean  | Yes  | Yes  | Values:<br/> • Yes: The entire scan_dir sub-directory tree is monitored.<br/> • No: No scan is performed. |
| METHOD  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.method | enum  | MOVE  | MOVE  | Values:<br/> • MOVE: Files are moved (by renaming), to the work_dir prior to being submitted.<br/> • FILE: Files are left in the scan_dir, and a state file with the same name is created in work_dir prior to submitting the file. |
| FILEIDLEDELAY  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_idle_delay | integer  | 5  | 5  | If the state of a file has not changed within this delay in seconds, the file becomes a candidate for submission.  |
| IDF  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.idf | string  | ""  | ""  | The IDF name to use in the SEND command. Use one of the following:<br/> • A fixed name.<br/> • "(0)": The name of the first directory sub-level is used.<br/> • "(1)": The name of the second directory sub-level is used. |
| PART  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.part | string  | ""  | ""  | The PART name to use in the SEND command. Use one of the following:<br/> • A fixed name.<br/> • "(0)": The name of the first directory sub-level is used.<br/> • "(1)": The name of the second directory sub-level is used. |
| INTERVAL  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.interval | int  | 60  | 60  | The interval between two scans of the directory files in seconds.  |
| FILECOUNT  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_count | int  | -1  | 0  | Maximum number of file submissions for each scan. Using the default value indicates that there is no maximum. |
| FILESIZEMIN  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_size_min | int  | -1  | 0  | Files shorter than this value, in bytes, are not candidates for submission. Using the default value indicates that there is no lower limit on the file size. |
| FILESIZEMAX  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_size_max | int  | -1  | 0  | Files larger than this value, in bytes, are not candidates for submission. Using the default value indicates that there is no upper limit on the file size.  |
| INCLUDEFILTER  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_include_filter | string  | ""  | ""  | If this parameter is defined, only files whose names match this pattern are monitored.  |
| EXCLUDEFILTER  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.file_exclude_filter | string  | ""  | ""  | If this parameter is defined, files whose names match this pattern are not monitored.  |
| RESUBMITCHANGED  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.resubmit_changed_file | Boolean  | Yes  | Yes  | This parameter has no effect when the configured method is MOVE.<br/> When the method parameter value is set to FILE:<br/> • Yes: When the state of a previously submitted file is seen as having changed, the file is submitted again.<br/> • No: Files are not resubmitted, regardless of changes.<br/> <blockquote> **Note**<br/> The file is resubmitted after any change regardless of if the modification is a small change, or purging and replacing the file with another file having the same name.<br/> </blockquote>  |
| FILTERTYPE  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.filter_type | enum  | WILDMAT  | WILDMAT  | Defines the pattern matching algorithm to use for file name filtering. Values:<br/> • STRJCMP: The Transfer CFT pattern matching algorithm.<br/> • WILDMAT: A well known public domain algorithm, and is the default. **Unix/Windows only**<br/> See [Create inclusion and exclusion filters](#Defining) for details. |
| RENAMEMETHOD  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.renaming_method | Enum  | TIMESTAMP  | TIMESTAMP  | This parameter applies only to the MOVE method.<br/> • NONE or " ": The filename is unchanged (no timestamp is added). If the file already exists in the work directory, the MOVE process fails.<br/> • TIMESTAMP, a timestamp of the pattern YYYYMMDDHHMMSS is added at the end of the name of the renamed file but before the last '.'.<br/> For example, using timestamp_separators=".": • myfile is renamed myfile.20131025<br/> • myfile.txt is renamed myfile.20131025.txt |
| RENAMESEPARATOR  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.renaming_separators | string  |   |   | This parameter only applies to the MOVE method. It must contain at most 2 characters from among the following:<br/> .[]()i_-<br/> The first character defines the separator before the timestamp. The second one, when present, defines the separator after the timestamp.<br/> For example, using timestamp_separators "[]": - myfile is renamed myfile.[20131025] - myfile.txt is renamed myfile.[20131025].txt |
| N/A in this version  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.control | string  |   |   | Metadata used to control user changes.  |
| USEFSEVENTS<br/>  | folder_monitoring.folders.<br/> &lt;logical_name&gt;.<br/> use_file_system_events<br/> [More information](../folder_monitor_uconf#File-sys) | Boolean  | No  | No  | Set to YES to enable the file system events monitoring service to detect newly available files.  |
| USERID  | N/A  | N/A  | N/A  | N/A  | This feature is not available in UCONF folder monitoring.  |


> **Note**
>
> \*You cannot use the following characters in the SCANDIR or WORKDIR definition. Additionally you cannot use a comma (,) in the CFTFOLDER SCANDIR or WORKDIR definition.

* {{< TransferCFT/PrimaryForunix >}} /
* For {{< TransferCFT/PrimaryforWindows >}} \\ / : \* ? " &lt; > &#124;

****Related topics****

* [Introduction to folder monitoring](../)
* [Folder monitoring CFTFOLDER](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftfolder)
* [Migrate to CFTFOLDER folder monitoring](#)
* [Create inclusion and exclusion filters](../folder_customize)
