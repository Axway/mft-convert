---
    "title": "Migration prerequisites",
    "linkTitle": "Perform a manual migration",
    "weight": "210"
---
After performing a {{< TransferCFT/axwayvariablesComponentShortName  >}} {{< TransferCFT/PrimaryTransferCFTversionlong  >}} installation, you should update to the most recent service pack.

{{% TransferCFT/snippets/r_license_2_version_to_3%}}

Check the impact of new features
--------------------------------

Before starting the migration, we highly recommend that you review the features changes listed in [Migration or upgrade impact and considerations](../../../#Migratio)

{{% TransferCFT/snippets/Window_x86_DLL%}}

Install {{< TransferCFT/axwayvariablesComponentShortName  >}} {{< TransferCFT/axwayvariablesComponentVersion  >}}
---------------------------------------------------------------------------------------------------------------------------

Perform a {{< TransferCFT/axwayvariablesComponentShortName  >}} installation, as described in the OS-specific installation section.

Load the environment
--------------------

Before beginning a standard migration procedure, you must load the old {{< TransferCFT/axwayvariablesComponentShortName  >}} environment.

### Windows procedure

#### Transfer CFT 2.4

There is no profile file for Transfer CFT 2.4 in Windows.

To execute a command you must be in the correct directory. Therefore, before starting the migration, change the directory to the version-appropriate {{< TransferCFT/axwayvariablesComponentShortName  >}} installation directory.

#### Transfer CFT 2.5 and higher

From the console, change the directory to the Transfer CFT runtime directory and execute the profile file using the command: profile.bat

After loading the profile, you can execute commands from anywhere.

### UNIX procedure

#### Transfer CFT 2.4

From the console, execute the profile file for your version of Transfer CFT, which is by default located in the home directory. Enter: `. ./ENV_CFT`

#### Transfer CFT 2.5 and higher

From the console, change directory to the Transfer CFT runtime directory and execute the profile file using the command: `. ./profile`

After loading the profile, you can execute commands from anywhere.
