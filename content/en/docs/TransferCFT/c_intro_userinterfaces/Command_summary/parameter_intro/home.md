{
    "title": "&HOME",
    "linkTitle": "_HOME_",
    "weight": "1450"
}### &HOME

A keyword to use with the [WORKINGDIR](../workingdir) parameter.

You can use the &HOME keyword in CFTSEND and CFTRECV command file names, script names, and directory parameters. This keyword functions differently depending on the system user setting:

- When system users are enabled, &HOME represents the home folder ($HOME on UNIX or %USERPROFILE% on Windows) of the user that requested the transfer.
- When system users are disabled, &HOME represents the home folder of the user that started Transfer CFT.

> **Note**
>
> Note: For more information on system users, refer to the platform specific guide, UNIX:Using system users or Windows: Using system users.
