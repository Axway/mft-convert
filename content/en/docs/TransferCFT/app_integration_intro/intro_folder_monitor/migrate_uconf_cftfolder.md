---
    "title": "Migrate to CFTFOLDER folder monitoring",
    "linkTitle": "Migrate to CFTFOLDER folder monitoring",
    "weight": "210"
---
You can use the ****Folder monitoring migration**** **utility** to help you migrate from the UCONF method of folder monitoring to the CFTFOLDER method. This procedure may be useful if you have migrated or upgraded from Transfer CFT 3.1.2 to Transfer CFT 3.2.4.

Reasons to switch to the CFTFOLDER method include:

- Folder monitoring is easier to manage via the CFTFOLDER object
- The UCONF method has a limitation on the number of logical folders that you can monitor

Procedure
---------

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

- You must enter each option or argument separately.
- The fname is ignored when using the purge command, or when using the (-s) simulation option.

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

- The purge removes all targeted folders from the folder_monitoring.folders listed in UCONF. As opposed to the migration, this action directly alters the UCONF configuration.
- If you are using -s (simulate), the tool only displays folders targeted by the purge.

Examples of how to use the migrate and purge commands
-----------------------------------------------------

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

- It is recommended, but you are not obliged, to purge the migrated UCONF folders. Therefore, if both CFTFOLDER and UCONF folders exists, the CFTFOLDER definitions takes precedence.
- Case sensitivity: Unlike UCONF, CFTFOLDER identifiers are not case sensitive - for example, a folder called "SamPle" is migrated as SAMPLE.
- Special characters: If folder names contain special characters (&”\#{$€ …) or accents (éàù …), migration fails as the utility cannot read these. However, theses are correctly rewritten if they are part of the SCANDIR and WORKDIR parameters.
- Folder name length: The length of the folder name in CFTFOLDER cannot exceed 32 characters. If a UCONF defined folder name is too long, it cannot be migrated.

Parameter mapping and descriptions

{{% TransferCFT/snippets/foldermonitoring%}}

****Related topics****

- [Introduction to folder monitoring](../)
- [Folder monitoring CFTFOLDER](../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftfolder)
- [Migrate to CFTFOLDER folder monitoring](#)
- [Create inclusion and exclusion filters](../folder_customize)
