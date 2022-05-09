{
    "title": "Introduction to folder monitoring ",
    "linkTitle": "Folder monitoring",
    "weight": "160"
}This section describes how to set up folder monitoring for Transfer CFT directories. Transfer CFT supports two general types of folder monitoring:

- **File-system event monitoring**: Transfer CFT immediately detects newly available files in a defined set of directories and identifies them as transfer candidates. File-system event monitoring is available on [Linux and Windows](#Supporte) exclusively.  
    *  
    ![](/Images/TransferCFT/FILESYSTEM.png)  
    *

<!-- -->

- <span id="scheduled_folder"></span>**Scheduled folder monitoring**: Transfer CFT periodically checks the status of files that are in a defined set of directories to see if there are transfer candidates.  
      
    ![](/Images/TransferCFT/SCHEDULED.png)

A set of [configurable](#Configur) options allow you to define which files are monitored and the condition that can trigger an automatic SEND command for these files.

<span id="About"></span>

How folder monitoring works
---------------------------

This section describes the underlying concepts for folder monitoring with Transfer CFT.

******Checking the state of a monitored file******

For each monitored file, {{< TransferCFT/axwayvariablesComponentShortName  >}} tracks the file size and the date of its last modification. These two pieces of information constitute the ****state**** of the file. If the state does not change within a certain delay in seconds, the file is considered to be available to submit for transfer. A delay of zero seconds indicates that the files are immediately ready for submission.

******Folder monitoring directories******

For each directory that Transfer CFT monitors (scan_dir parameter), there is a second directory (work_dir parameter) for Transfer CFT to use for tracking. These two directories must be separate entities (the work_dir must differ from the scan_dir, and the work_dir cannot be a sub-directory of the scan_dir), and allow files to move from one directory to the other. While Transfer CFT will not create the scan_dir directory, the work_dir is automatically created if it does not already exist.

******Tracking files******

To track scan_dir files that have been submitted, Transfer CFT can either:

- Move these files to the work_dir (by renaming) prior to submitting them. This is called the MOVE method in Transfer CFT.
- Leave these files in place and create another file with the same name, which contains metadata, in the work_dir prior to submitting the transfer. This is called the FILE method in Transfer CFT.

See [Configuring file tracking options](#Configur2).

******SEND parameters******

In addition to the file path, the [IDF](../../c_intro_userinterfaces/command_summary/parameter_intro/idf) and [PART](../../c_intro_userinterfaces/command_summary/parameter_intro/part) name parameters are supplied in the SEND command. You can set these as fixed parameter values, or extract them from the first or second sub-directory names. {{< TransferCFT/axwayvariablesComponentShortName  >}} then automatically creates the corresponding sub-directories in the work_dir directory tree, as needed.

<span id="Prerequisites_foldermonitoring"></span>

Best practices
--------------

This section lists the best practices for folder monitoring. Before configuring folder monitoring, read the following:

- There are 2 options (FILE and MOVE methods); the MOVE method is recommended as explained [below](#Configur2).
- Do not define a scan_directory that is the same as the work_directory.
- Do not define a work_directory as a subdirectory of a scan_directory.
- When using the FILE method, do not delete the .met files that are automatically created in the working directory.

<span id="Limitati"></span>

Limitations
-----------

- Directory scanning does not allow transferring a file whose name contains a wildcard character, for example the asterisk character (\*) on UNIX-like platforms. Files containing wildcard characters in their file names are not be processed; they remain in the scanning directory, and an error message displays in the log. For a list of wildcard characters per platform, see [Platform-specific characters and functions](../../cft_intro_install/about_this_document_vms/c_cft_introduction_vms/installation/platform_specific_characters_and_functions).
- In an z/OS environment you can only use folder monitoring on UNIX file systems.
- The WILDMAT parameter is available on Unix/Windows/IBM i systems.
- When using the UCONF mode (deprecated mode) to enable folder monitoring, there is a limit to the number of folders due to the maximum length of the `folder_monitoring.folders` parameter being 512.
- Presently the folder monitoring CFTFOLDER option is only available in command line.
- Folder monitoring using a different user (USERID) is not available on Linux if the event mode is enabled (USEFSEVENTS=YES). Additionally, this feature is not supported in the obsolete UCONF folder configuration.
- For a multi-host installation, the METHOD=MOVE for folder monitoring is not supported on an NFS 3 shared disk due to synchronization delays. Instead, use NFS 4 or METHOD=FILE.
- The system user management feature ([USERCTRL](../../internal_a_m_start_here/user_rights_overview)) is not supported for Folder Monitoring configurations.

<span id="Supporte"></span>

### Supported OS for file-system event monitoring

The operating system services for *file-system event monitoring* functionality only work on local file systems. Transfer CFT supports this type of monitoring on:

- Linux: EXT family of file systems
- Windows: NTFS file systems

<span id="Configur"></span>

Configuration overview
----------------------

This section describes the key parameters that can alter the folder monitoring behavior. They relate to how you implement scheduling, and the method by which you move a file that is a ready transfer candidate.

### Recommendations

There are two ways to implement Transfer CFT folder monitoring, either using UCONF or Transfer CFT objects. We recommend the CFTFOLDER method of configuring folder monitoring. Users that presently are using UCONF to manage folder monitoring can migrate to a CFTFOLDER configuration as described in [Migrate to CFTFOLDER folder monitoring](migrate_uconf_cftfolder).

### Transfer CFT file monitoring scheduling

Two parameters define the timing for files that are managed by Transfer CFT folder monitoring. See the [parameter definition](../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftfolder) table for details.

- File idle delay: This defines the period in which the file becomes a candidate for submission if the state of a file has not changed within this delay (FILEIDLEDELAY).
- Interval: The interval between two scans of the directory files in seconds (INTERVAL).

<span id="Configur2"></span>

### Configuring file tracking options

Submitting the SEND command occurs differently depending on if the METHOD parameter value is set to MOVE or FILE as described in the following sections.

#### MOVE option

If the METHOD parameter is set to MOVE:

1. The file is moved (by renaming) to the work_dir in the same relative directory position as its original position in the scan_dir. By default, an optional timestamp is added to the name of the moved file.
1. A command is submitted internally that uses the following pattern:

```
CFTUTIL SEND=<part>, IDF=<idf>, FNAME=<pathname>
```

Where:

- &lt;part&gt; is the partner name as indicated in the configuration.
- &lt;idf&gt; is the idf name as indicated in the configuration.
- &lt;pathname&gt; is the absolute pathname of the file in the work_dir directory.

****Command that is submitted for a new file****

The following example demonstrates how the PART name is used for the first directory sub-level, and the IDF name for the second level sub-directory.

- Original file:     `/dir_c/scan/newyork/idf1/my_file.txt`
- Moved file: ` /dir_c/work/newyork/idf1/my_file.20131025.txt`

```
CFTUTIL SEND part=newyork, idf=idf1, fname=/dir_c/work/newyork/idf1/my_file.20131025.txt
```

#### FILE option

When the METHOD parameter is set to FILE, the original file is not renamed. However, to prevent the file from being sent again when {{< TransferCFT/axwayvariablesComponentShortName  >}} is restarted, and to keep track of persistent information about the file, a new file is created in the work_dir directory. This new file contains metadata about the file that {{< TransferCFT/axwayvariablesComponentShortName  >}} has generate.

Submitting the SEND command occurs as follows:

1. A new file with the same name as the original file, but suffixed with ****.met****, is created in the directory work_dir in the same relative position as the original file. Metadata generated by {{< TransferCFT/axwayvariablesComponentShortName  >}} are written to this .met file.

    > **Note**
    >
    > Caution  Do NOT delete the .met files as they are internal Transfer CFT files. In order to delete the .met files, you must delete the associated files in the SCAN directory (Transfer CFT must be running).

1. The same CFTUTIL command as is used in the MOVE method is submitted internally, but with an fname corresponding to the original file in the scan_dir.

**Command that is submitted for a new file**

This example uses the partner name in the first directory sub-level, and the IDF name in the second.

- Original file:   ` /dir_c/scan/newyork/idf1/my_file.txt`
- Metadata file: `/dir_c/work/newyork/idf1/my_file.txt.met`

```
CFTUTIL SEND part=newyork, idf=idf1, fname=/dir_c/scan/newyork/idf1/my_file.txt
```

Although both methods provide the same level of functionality, the MOVE method (renaming file) is recommended whenever file renaming prior to its submission is acceptable.

The FILE  method has some drawbacks that the MOVE method does not, especially related to performance. FILE method limitations include:

- For each file to send, a second file (the .met file) is created.
- Since all of the files remain in the directory, the directory to be scanned may contain a large number of files.
- All of the .met files must be checked by {{< TransferCFT/axwayvariablesComponentShortName  >}} at start up.
- {{< TransferCFT/axwayvariablesComponentShortName  >}} cannot differentiate between when a file has been purged and replaced by a new one, or if this same file has just been modified.

****Related topics****

- [Folder monitoring CFTFOLDER](../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftfolder)
- [Folder monitoring IBM i native files](folder_ibm_i_native)
- [Deprecated folder monitoring (UCONF)](folder_monitor_uconf)
- [Migrate to CFTFOLDER folder monitoring](migrate_uconf_cftfolder)
- [Create inclusion and exclusion filters](folder_customize)
