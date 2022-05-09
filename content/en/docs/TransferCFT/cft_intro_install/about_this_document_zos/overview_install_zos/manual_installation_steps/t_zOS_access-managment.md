{
    "title": "Define internal access management",
    "linkTitle": "Define internal access management",
    "weight": "280"
}This section describes the OS specific settings required to enable internal access management. As a prerequisite, you must read and be familiar with the [Internal access management](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/internal_access_mgt/internal_a_m_start_here.htm) information in the *Transfer CFT User Guide*.

About internal AM
-----------------

There are three methods available to define internal access management when using Transfer CFT z/OS - a system service, SAF class, or a file.

The are two delivered JCLs, H81$AMIN and H81$AMSF, to use to implement internal access management.

- H81$AMSF: This JCL activates access management for the group database for the SAF class method
- H81$AMIN: This JCL activates access management for the group database for either the system or file method

Alternatively, you can use UCONF to define the internal access management. See the Internal access management information in the Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} {{< TransferCFT/suitevariablesDocTypeUser  >}} for more information.

### Using system as the internal access management

With **system** access management, you define the mapping between predefined roles and groups in UCONF, and the user groups assignment in RACF.

When the access management type is **system**, you must give the STC userID the SPECIAL attribute AUDITOR to be able to read the RACF user profiles.

Define the following facilities for the USERID owner of the STC Copilot CFTMAIN and API batches:

```
UCONFSET ID=am.internal.group_database,value=system
SETROPTS GENERIC(FACILITY)
RDEFINE FACILITY IRR.RADMIN.\* UACC(READ)
RDEFINE FACILITY IRR.RADMIN.LISTUSER UACC(READ)
RDEFINE FACILITY IRR.RADMIN.LISTGRP UACC(READ)
RDEFINE FACILITY IRR.RADMIN.RLIST UACC(READ)
RDEFINE FACILITY IRR.RADMIN.SETROPTS.LIST UACC(READ)
```

If you defined the previous facilities as UACC(NONE), you must set READ rights each Transfer CFT user.

```
PERMIT IRR.RADMIN.LISTUSER -CLASS(FACILITY) ACCESS(READ) ID(user)
PERMIT IRR.RADMIN.LISTGRP -CLASS(FACILITY) ACCESS(READ) ID(user)
PERMIT IRR.RADMIN.RLIST -CLASS(FACILITY) ACCESS(READ) ID(user)
PERMIT IRR.RADMIN.SETROPTS.LIST -CLASS(FACILITY) ACCESS(READ) ID(user)
```

In Transfer CFT, set the `group_database` to `system`.

```
UCONFSET ID=am.internal.group_database,value=system
```

### Using SAF class as the internal access management

With the **SAF class** method of access management, you define the mapping between predefined roles and resources in UCONF, and you define the user classes at the system level using a SAF (System Authorization Facility) class.

When the access management method is **SAF class**, each user role is associated with a security resource (RACF, TSS, ACF2). You can set the name of the resource in the corresponding UCONF variable using the user norms in the table below.


| Example resource name  | UCONF variable/role  | Access to resource  |
| --- | --- | --- |
| CFT.ROLE.ADMIN  | am.internal.role.admin  | READ  |
| CFT.ROLE.OPERATOR  | am.internal.role.helpdesk  | READ  |
| CFT.ROLE.PARTNER  | am.internal.role.partnermanager  | READ  |
| CFT.ROLE.DESIGNER  | am.internal.role.designer  | READ  |
| CFT.ROLE.TRANSFER  | am.internal.role.application  | READ  |


```
UCONFSET ID=am.internal.group_database,value=safclass
UCONFSET ID=am.internal.safclass,value='Class
' (the class resource must be available)
 
For each ROLE (the following are examples, replace with your own values):

> RDEFINE Class CFT.ROLE.ADMIN UACC(NONE) OWNER(...)
> RDEFINE Class CFT.ROLE.OPERATOR UACC(NONE) OWNER(...)
> RDEFINE Class CFT.ROLE.PARTNER UACC(NONE) OWNER(...)
> RDEFINE Class CFT.ROLE.DESIGNER UACC(NONE) OWNER(...)
> RDEFINE Class CFT.ROLE.TRANSFER UACC(NONE) OWNER(...)

 
Authorize users or groups: (RACF sample)

> PERMIT CFT.ROLE.ADMIN CLASS(Class) ACCESS(READ) ID(USER001)
> PERMIT CFT.ROLE.TRANSFER CLASS(Class) ACCESS(READ) ID(USER002)

 
NOTE: ACCESS must be set to READ
.
```

> **Note**
>
> Note: The load libraries must be APF.

### Using file as the internal access management

With **file** type access management, you define the mapping between predefined roles and groups in UCONF, and assign the user groups in an external file.

When using this type of access management, the file format must be VB, where the maximum number of `lrecl `is 1024. Enter the character \* in column 1 to allow comments.

#### User/Group record description

Start with the `UserID `in column 1 using blanks as separators. For example, to assign users rights, use the format:

****Format****

```
USER001 OPERATOR PARTNER .....
USER002 ADMIN ..... 
```

In Transfer CFT, set the `group_database` to file and specify the path to the file defined above.

```
UCONFSET ID=am.internal.group_database,value=file
UCONFSET ID=am.internal.group_database.fname,value=filename
```
