{
    "title": "1. Create new users",
    "linkTitle": "1. Create new users ",
    "weight": "200"
}Begin by creating a new user in {{< TransferCFT/suitevariablesCentralGovernanceName  >}}. You can refer to the {{< TransferCFT/PrimaryCGorUM  >}} {{< TransferCFT/suitevariablesDocTypeUser  >}} for detailed descriptions of user roles and privileges.

To create new systems users:

1. In {{< TransferCFT/PrimaryCGorUM  >}}, create your users and assign the appropriate roles and privileges as described in the {{< TransferCFT/suitevariablesCentralGovernanceName  >}} {{< TransferCFT/suitevariablesDocTypeUser  >}}.  
    For example, create the system users Flow Manager, Partner Manager, and Monitoring Assistant.
1. Define the user rights for actions on files (USERCTRL) for these same new users for the local system where Transfer CFT is installed.

> **Note**
>
> Superusers have all rights on Transfer CFT (you can check the uconf am.passport.superuser parameter, providing a list of superusers). It is important to remember that in UNIX/Windows, the user that installs Transfer CFT is the superuser. This means that even if you restrict a user's roles in Central Governance, if that user is the superuser it can still perform all actions on files. Additionally, if you define a system user during installation, that user is automatically added to the am.superuser list of users.

For users that have not yet implemented {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, either create user permissions in Passport, or continue on to Step 2.

Parameter mapping for {{< TransferCFT/suitevariablesCentralGovernanceName  >}}
-----------------------------------------------------------------------------------

The following parameters are now managed in {{< TransferCFT/suitevariablesCentralGovernanceName  >}}. This table maps the existing {{< TransferCFT/axwayvariablesComponentLongName  >}} defaults and values.


| CG field  | CG values  | CFTUTIL parameter  | Description  |
| --- | --- | --- | --- |
| User for file access  | <span >Transfer CFT system account</span> &#124; USERID variable  | CFTPARM - USERCTRL = <span >NO</span> &#124; YES  | Specifies the account that is used to read/write transferred files.  |
| User for script execution  | <span >Transfer CFT system account</span> &#124; USERID variable  | UCONF - cft.server.exec_as_ user = <span >NO</span> &#124; YES  | Specifies the account that is used to execute scripts. This parameter is not supported on Transfer CFTs running on z/OS and IBM i systems.  |
| Check permission for transfer execution  | YES &#124; <span >NO</span>  | am.passport.userctrl.check_ permissions_on_transfer_ execution  | Checks whether the user has permissions to execute transfers.  |
| Create process as user  | YES &#124; <span >NO</span>  | copilot.misc.createprocessasuser  | Specifies whether Transfer CFT Copilot user must have system rights.  |


****Related topics****

- [Recommendations and troubleshooting](../user_rights_tips)
