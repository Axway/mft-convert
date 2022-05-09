{
    "title": "Uninstall a service pack or patch",
    "linkTitle": "Uninstall a service pack or patch",
    "weight": "220"
}Uninstall a service pack
------------------------

When you apply a service pack, a backup of your previous version is saved in a SAVF object (named, for example, SPXSAV), which is in your production library. You can use the UNINSTALL command, delivered in the program library, to uninstall a service pack and roll back to the previous version.

Follow these steps to uninstall a service pack:

1. Stop Transfer CFT and Copilot.
1. Call the UNINSTALL command, press F4, and complete the following fields:

```
CFT Uninstall (UNINSTALL)
 
Type choices, press Enter.
Silent installation . . . . . . '2
'         1:Yes/2:No
CFT Program library . . . . . . CFTPGM Name
CFT Production library . . . . . CFTPROD Name
SAVF name of previous version . SPXSAV Name
```

- Where:
    -   Parameter 1: Name of your program library
    -   Parameter 2: Name of your production library
    -   Parameter 3: Name of the SAVF where you saved your previous library when you updated Transfer CFT

1. Press Enter. The patch or service pack is removed, and you are returned to the previous version.

> **Note**
>
> Note: As with other Transfer CFT commands, ensure that your program library (CFTPGM) and production library are present in your EDTLIBL before calling the UNINSTALL command.

Uninstall a patch
-----------------

When you apply a patch, a backup of your previous version is saved in a `SAVF `object (for example, `PATCHSAV`). This SAVF file is located in your program library.

If you need to uninstall a patch, please restore the previous SAVF to your ****CFT program library**** (`PATCHSAV` by default).

****Example****

```
RSTOBJ OBJ(\*ALL) SAVLIB(CFTPGM) DEV(\*SAVF) OBJTYPE(\*ALL) SAVF(CFTPGM/PATCHSAV) OPTION(\*ALL) RSTLIB(CFTPGM)
```
