---
title: "Folder monitoring CFTFOLDER"
linkTitle: "Folder monitoring - CFTFOLDER"
weight: 260
---This section provides a description of how to use Transfer CFT objects to manage folder monitoring.

> **Note**
>
> There are two ways to implement Transfer CFT folder monitoring, either using UCONF or Transfer CFT objects. We recommend the CFTFOLDER method of configuring folder monitoring. Users that presently are using UCONF to manage folder monitoring can migrate to a CFTFOLDER configuration as described in Migrate to CFTFOLDER folder monitoring.

## CFTFOLDER object

The following table describes the parameters you can use to define the CFTFOLDER object. Additionally the table lists the UCONF equivalent for users who have opted for a polling type of folder monitoring.

### Folder monitoring parameters

Use the following CFTFOLDER parameters to configure folder monitoring for each directory as needed.


| <span id="Paramete"></span>Parameter  | Type  | Default  | Description  |
| --- | --- | --- | --- |
| same as in UCONF<br/> &lt;folder_monitoring.enable&gt; | Boolean  | No  |  • No: No folder monitoring occurs.<br/> • Yes: Enable {{< TransferCFT/axwayvariablesComponentShortName  >}} folder monitoring. |
| ID<br/> **Mandatory** | node  | None  | Add the logical folders to monitor (list of logical identifiers).<br/> You should provide a unique name to identify the set of configuration parameters corresponding to this directory. If you have more than one Folder to monitor, use a space between each logical value. |
| STATE  | Boolean  | Active  | Enables a scan of the folder.<br/> <blockquote> **Note**<br/> NO = NOACTIVE.<br/> </blockquote>  |
| SCANDIR<br/> *Mandatory*  | string  | None  | Absolute path name of the top level directory to scan. You can use an environmental variable with the syntax <code>%env:&lt;variable&gt;%</code> however note that this variable is replaced with the actual value at the time you define the object.<br/> This directory must exist before restarting Transfer CFT.<br/> *See [NOTE](#*char_note). |
| WORKDIR<br/> *Mandatory*  | string  | None  | Absolute path name of the top level directory available for file state information. You can use an environmental variable with the syntax <code>%env:&lt;variable&gt;%</code> however note that this variable is replaced with the actual value at the time you define the object.<br/> • If you are using the MOVE method, files that are ready to be submitted are available in the work_dir.<br/> • If you are using the FILE method, the .met files are stored in the work_dir.<br/> <blockquote> **Note**<br/> Caution Never delete any .met files.<br/> </blockquote> *See [NOTE](#*char_note). |
| ENABLESUBDIR  | Boolean  | Yes  | Values:<br/> • Yes: The entire scan_dir sub-directory tree is monitored.<br/> • No: No scan is performed. |
| <span id="METHOD"></span>METHOD  | enum  | MOVE  | Values:<br/> • MOVE: Files are moved to the work_dir prior to being submitted.<br/> • FILE: Files are left in the scan_dir, and a state file with the same name is created in work_dir prior to submitting the file.<br/> <blockquote> **Note**<br/> Before changing the method from FILE to MOVE, you should remove all files (metadata .met files) located in the associated working directory.<br/> </blockquote> <blockquote> **Note**<br/> Changing the method from MOVE to FILE, deletes all files located in the associated working directory. Therefore, we recommend removing all files from the scan and working directory before changing the METHOD type.<br/> </blockquote> Please see the [Limitations](../../../../app_integration_intro/intro_folder_monitor#Limitati) for multi-host system recommendations. |
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
| FILTERTYPE  | enum  | WILDMAT  | Defines the pattern matching algorithm to use for file name filtering.<br/> Values:<br/> • STRJCMP: The Transfer CFT pattern matching algorithm.<br/> • WILDMAT: A well known public domain algorithm, and is the default. **Unix/Windows only**<br/> • EREGEX: Extended regular expression syntax.<br/> See [Create inclusion and exclusion filters](../../../../app_integration_intro/intro_folder_monitor/folder_customize#top) for details. |
| RENAMEMETHOD  | Enum  | TIMESTAMP  | This parameter applies only to the MOVE method. When set to TIMESTAMP, a timestamp of the pattern YYYYMMDDHHMMSS is added at the end of the name of the renamed file but before the last '.'.<br/> For example, using timestamp_separators=".": • myfile is renamed myfile.20131025<br/> • myfile.txt is renamed myfile.20131025.txt<br/> <blockquote> **Note**<br/> Unset the default value and use " " to MOVE without adding a timestamp.<br/> </blockquote>  |
| RENAMESEPARATOR  | string  | OpenVMS:<br/> " _ "<br/> All other OS:<br/> "." | This parameter only applies to the MOVE method.<br/> You can use no more than two characters from among the following:<br/> .[]()_-<br/> The first character defines the separator before the timestamp. The second one, when present, defines the separator after the timestamp.<br/> For example, using timestamp_separators "[]": - myfile is renamed myfile.[20131025] - myfile.txt is renamed myfile.[20131025].txt |
| N/A in this version  | string  |   | Metadata used to control user changes.  |
| USEFSEVENTS<br/> <br/> [More information](#) | Boolean  | No  | Set to YES to enable the file system events monitoring service to detect newly available files.  |
| [Folder monitoring using USERCTRL](../../../command_summary/parameter_intro/userid). |


> **Note**
>
> \*You cannot use the following characters in the SCANDIR or WORKDIR definition. Additionally you cannot use a comma (,) in the CFTFOLDER SCANDIR or WORKDIR definition.

- {{< TransferCFT/PrimaryForunix >}} /
- For {{< TransferCFT/PrimaryforWindows >}} \\ / : \* ? " &lt; > &#124;

****Related topics****

- [Introduction to folder monitoring](../../../../app_integration_intro/intro_folder_monitor)
- [Deprecated folder monitoring (UCONF)](../../../../app_integration_intro/intro_folder_monitor/folder_monitor_uconf)
- [Migrate to CFTFOLDER folder monitoring](../../../../app_integration_intro/intro_folder_monitor/migrate_uconf_cftfolder)
- [Create inclusion and exclusion filters](../../../../app_integration_intro/intro_folder_monitor/folder_customize)
