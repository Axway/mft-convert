---
    "title": "Folder monitoring CFTFOLDER",
    "linkTitle": "Folder monitoring - CFTFOLDER",
    "weight": "260"
---
This section provides a description of how to use Transfer CFT objects to manage folder monitoring.

> **Note**
>
> Note: There are two ways to implement Transfer CFT folder monitoring, either using UCONF or Transfer CFT objects. We recommend the CFTFOLDER method of configuring folder monitoring. Users that presently are using UCONF to manage folder monitoring can migrate to a CFTFOLDER configuration as described in Migrate to CFTFOLDER folder monitoring.

CFTFOLDER object
----------------

The following table describes the parameters you can use to define the CFTFOLDER object. Additionally the table lists the UCONF equivalent for users who have opted for a polling type of folder monitoring.

### Folder monitoring parameters

Use the following CFTFOLDER parameters to configure folder monitoring for each directory as needed.

{{% TransferCFT/snippets/cftfolder_parms%}}

> **Note**
>
> Note: \*You cannot use the following characters in the SCANDIR or WORKDIR definition. Additionally you cannot use a comma (,) in the CFTFOLDER SCANDIR or WORKDIR definition.

- {{< TransferCFT/PrimaryForunix  >}} /
- For {{< TransferCFT/PrimaryforWindows  >}} \\ / : \* ? " &lt; &gt; &#124;

****Related topics****

- [Introduction to folder monitoring](../../../../app_integration_intro/intro_folder_monitor)
- [Deprecated folder monitoring (UCONF)](../../../../app_integration_intro/intro_folder_monitor/folder_monitor_uconf)
- [Migrate to CFTFOLDER folder monitoring](../../../../app_integration_intro/intro_folder_monitor/migrate_uconf_cftfolder)
- [Create inclusion and exclusion filters](../../../../app_integration_intro/intro_folder_monitor/folder_customize)
