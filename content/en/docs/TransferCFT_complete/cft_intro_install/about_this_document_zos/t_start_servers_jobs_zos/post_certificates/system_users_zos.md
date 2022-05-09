{
    "title": "About system users",
    "linkTitle": "About system users",
    "weight": "260"
}If the system does not have an APF, authorized program facility, the USERCTRL has no effect on the file actions. All file actions are done by the account that started Transfer CFT. Refer to [About system users](../../../c_about_zos/system_users_zos), and the *Transfer CFT User Guide* for more information on USERCTRL.

To enable user control for file actions, you require an APF.

> **Note**
>
> Note: An APF allows an installation to identify system or user programs that can use sensitive system functions.
