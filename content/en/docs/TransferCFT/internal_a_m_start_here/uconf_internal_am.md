{
    "title": "Internal access management",
    "linkTitle": "Internal access management",
    "weight": "150"
}This section describes how to configure {{< TransferCFT/axwayvariablesComponentLongName  >}}‘s *internal* access management. Internal access management is an out-of-the-box access management based on roles and privileges and a internal group datafile, where groups and their members are defined in a supplied database. You can use this type of access management with or without Central Governance or Flow Manager.

{{< TransferCFT/axwayvariablesComponentLongName  >}} supports two types of internal access management. However, using CFTROLE objects is the implementation of choice as opposed to the previously-used, hard-coded roles.

Overview
--------

### About internal access management

Internal AM requires a database, which can be the `system`, `xfbadm`, `file`, `safclass` containing defined group members. For each group, {{< TransferCFT/axwayvariablesComponentLongName  >}} uses an alias or identifier to map a CFTROLE object in the Transfer CFT configuration to a group defined in the database.

A user belongs to a group that is mapped to a role. So when this user provides credentials when logging on {{< TransferCFT/axwayvariablesComponentLongName  >}}, they will have the privileges that correspond with this role.

### Internal access management example


| Action  | Details  |
| --- | --- |
| <code>am.internal.group_database</code> is set to <code>system</code>  | System users (local or LDAP users) belong to a group, for example <code>USER1 </code>is in the group <code>USERS_CFTUI_GROUP</code>. |
| <code>USER1 </code>connects to the user interface  | Transfer CFT retrieves the corresponding system group and checks the configuration for an existing CFTROLE (using an ID or ALIASES). |
| {{< TransferCFT/axwayvariablesComponentLongName  >}} locates the CFTROLE  | {{< TransferCFT/axwayvariablesComponentLongName  >}} loads the privileges associated with <code>USERS_CFTUI_GROUP</code> and allows <code>USER1</code> to connect. Additionally, {{< TransferCFT/axwayvariablesComponentLongName  >}} applies the appropriate rights and privileges for <code>USER1</code>.<br/> The same check occurs for each user who is part of a group that is defined in <code>am.internal.group_database</code>. |


### Previous internal access management option

Prior to {{< TransferCFT/axwayvariablesComponentLongName  >}} 3.8, internal access management used predefined, hard-coded, unmodifiable roles. You can still use this method, however the CFTROLE object, described above, is the method of choice.

The predefined roles privileges include:

- **Administrator**: Full user access
- **Helpdesk**: View the catalog and log
- **Partner Manager**: Manage partners
- **Designer**: Manage application flows
- **Application**: Applications can request and manage transfers, and view the catalog

<!-- -->

- **Custom**: Create new custom roles

Please refer to the [*Transfer CFT *{{< TransferCFT/axwayvariablesReleaseNumber  >}} *Security Guide*](https://docs.axway.com/bundle/TransferCFT_36_SecurityGuide_allOS_en_HTML5/page/Content/security_guide/predefined_privileges.htm) for a complete list of privileges and roles. Log in is required. See also , which describes use cases and their configuration.

### Internal access management parameters

Deprecated parameters are gray and noted as (deprecated).


| Internal AM parameters  | Default  | Description  |
| --- | --- | --- |
| am.internal.group_database  | file (z/OS)<br/> system (all other platforms) | Group database where group members are defined.<br/> • system (UNIX, Windows, and IBM i): the groups are defined in the OS group database (Unix, Windows, IBM i - see [Transfer CFT control utilities](../../cft_intro_install/about_this_document_ibmi/install_intro_ibmi/access-managment_ibmi) |
| am.internal.group_database.fname  |   | If you set<code> am.internal.group_database=file</code>, you must define this file name, which is a variable file containing the groups associated with each user.<br/> For example:<br/> • USER001 group01 group02 group04<br/> • USER002 group04 group05<br/> Where the groups are mapped as shown in the example [mapping](#Mapping) table below. |
| am.internal.persistence_timeout  | 300  | Delay in seconds between updating the list of group that a user belongs to. |
| am.internal.role.admin <sup>(deprecated)</sup>  |   | Admin role and groups mapping. This role enables you to perform all administrative tasks. List of groups (blank separator) |
| am.internal.role.helpdesk <sup>(deprecated)</sup>  |   | Help Desk role and groups mapping. This role enables you to view the log, transfers and configuration. List of groups (blank separator) |
| am.internal.role.partnermanager <sup>(deprecated)</sup>  |   | Partner Manager role and groups mapping. This role enables you to create and manage partner. List of groups (blank separator) |
| am.internal.role.designer <sup>(deprecated)</sup>  |   | Designer role and groups mapping. This role enables you to manage flows. List of groups (blank separator) |
| am.internal.role.application <sup>(deprecated)</sup>  |   | Application role and groups mapping. This role enables application to send transfers. List of groups (blank separator) |


<span id="Configur"></span>

Configuring internal access management
--------------------------------------

The following step overview was tested on a Windows operating system. You should modify according to your system setup.

> **Note**
>
> Note: When using internal access management, there is no super user and the user who installed and starts the Transfer CFT Copilot server must have administrator rights.

1. Define the type of database. Set:

    `uconfset id=am.internal.group_database,value=system` (`xfbadm`, `file`, or `safclass `depending on your operating system)

1. Access the delivered` roles-smp.conf` sample file in `runtime/conf/ `folder`. `
1. Modify the `CFTPRIV `and `CFTROLES `to achieve the desired security objectives. For example:
    1.  Scroll to the `CFTROLE ID='TRANSFER CFT HELPDESK' `object.
    2.  Remove the commenting on the ALIASES line for this object and enter the alias:
        -   Before editing:    `/* ALIASES = ( ) ,*/`
        -   After editing:         `ALIASES = (MYHELPDESK),`
    3.  Save your changes.
    4.  Interpret the file: `config type=input, fname= conf/roles-smp.conf`
1. *Optionally*, create groups in the system that correspond to either the CFTROLE ID or ALIAS and add users to this group. For example, create a group called `CFT_HELPDESK` and a user `user1` that is recognized on your system. Again, on s Windows system:
    1.  Create a group called MYHELPDESK: `net localgroup MYHELPDESK /add`
    2.  Add a user called user1 to the MYHELPDESK group: `net localgroup MYHELPDESK user1 /add`
1. Set the access management type:` uconfset id=am.type,value=internal`

<!-- -->

- *Optionally* if you re activating internal mode from {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}}.
- It is important that you perform this command *after* performing the previous steps.

Creating, modifying, and mapping roles
--------------------------------------

You can create new roles or modify existing roles using either the {{< TransferCFT/suitevariablesTransferCFTName  >}} user interface or a configuration file. You can use new roles on their own or in combination with predefined roles. Please refer to for role-based, use case scenarios.

> **Note**
>
> Note: If Central Governance is managing your Transfer CFTs, you should manage CFTROLE and CFTPRIVILEGE locally for each Transfer CFT.

> **Note**
>
> Note: When using Flow Manager, all Transfer CFT roles and privileges are managed by Flow Manager.

### Using the configuration file

{{< TransferCFT/axwayvariablesComponentLongName  >}} provides a `roles-smp.conf` sample file in `runtime/conf/` directory (*Unix and Windows*) that contains example roles and privileges. You can add, remove, or modify roles or privileges in this sample file. After adding or modifying a role or privilege, you must interpret the configuration file.

> **Note**
>
> Note: The configuration file is delivered in CFTPROD/UTIN(ROLESSMP) for native IBM i environments.

### Using the user interface

In the {{< TransferCFT/suitevariablesTransferCFTName  >}} user interface, access the **General Configuration** pane and select either **Privileges** or **Roles**. Options include creating, deleting, modifying, or cloning a role or privilege.

### Mapping CFTROLE to groups

The {{< TransferCFT/axwayvariablesComponentLongName  >}} CFTROLE object is mapped to a user group using either:

- `CFTROLE ID=<name of user group>`
- ` CFTROLE ID= <name>, ALIASES=<name of user group>`

<span id="Mapping"></span>

Mapping a group to predefined roles (deprecated)
------------------------------------------------

This section describes the roles and privileges method that was implemented in {{< TransferCFT/axwayvariablesComponentLongName  >}} 3.8 and lower. In this method, you map the list of groups in the database to predefined roles. If you are using the internal access management method described above in [Configuring internal access management](#Configur), do not set the uconf parameters described in this section.

You can use the following information as a basis for your mapping, and enter the values via command line or the user interface.


| Parameter  | The users have the following roles...  |
| --- | --- |
| am.internal.role.admin=group01  | The user who belongs to group “group01” has the admin role.  |
| am.internal.role.helpdesk=group02  | The user who belongs to group “group02” has the “helpdesk” role.  |
| am.internal.role.partnermanager=group03  | The user who belongs to group “group03” has the “partner manager” role.  |
| am.internal.role.designer =group04  | The user who belongs to group “group04” has the “designer” role.  |
| am.internal.role.application=group05  | The user who belongs to group “group05” has the “application” role.  |


### Configure internal AM for {{< TransferCFT/axwayvariablesComponentLongName  >}} 3.8 and lower

Set the access management type:

```
uconfset id=am.internal.role.admin,value=admin
uconfset id=am.type,value=internal
```

> **Note**
>
> Note: When using internal access management, there is no super user and the user who installed and starts the Transfer CFT Copilot server must have administrator rights.

### Role priority and customized roles

When using a combination of predefined roles and custom roles that you created in the CFTPARM object, the new roles will override existing roles if they have the same name. For example, if you create two roles in CFTPARM with sames identifiers as predefined roles:

```
CFTROLE ID='HELPDESK', ...
CFTROLE ID='APPLICATION', ...
```

Here, the new HELPDESK and APPLICATION roles override the predefined HELPDESK and APPLICATION roles. However, the predefined ADMIN, PARTNERMANAGER, and DESIGNER roles are still used since you did not create new roles with these same names.

Internal access management use cases
------------------------------------

This section describes three configuration scenarios when using [internal access management](#) with roles and privileges. Use cases include:

- Predefined roles and privileges without the CFTPRIV and CFTROLE objects
- Customized roles and privileges with the CFTPRIV and CFTROLE objects
- Mixed-use of both predefined and custom CFTPRIV and CFTROLE objects

In each scenario, there is a **user1** that belongs to **group1** and a **user2** that belongs to **group2**.

****Example 1: Custom roles and privileges using only CFTPRIV and CFTROLE objects****

If you configure UCONF as follows:

```
uconfset id=am.internal.group_database,value=system     NOTE: system (Unix Windows,IBM i), xfbadm (Unix,HP NonStop)
uconfset id=am.type,value=internal
listuconf id=am.internal.role.\*
 
D am.internal.role.admin =
D am.internal.role.helpdesk =
D am.internal.role.partnermanager =
D am.internal.role.designer =
D am.internal.role.application =
```

Modify the `conf/roles-smp.conf` sample file:

```
CFTROLE ID = 'TRANSFER CFT ADMINISTRATOR',
COMMENT = 'Enables full management of Transfer CFT.',
ALIASES = ( 'group1' ),
PRIVS = ( 'SWITCH ACCOUNTS', ...
 
CFTROLE ID = 'TRANSFER CFT APPLICATION',
COMMENT = 'Enables applications to send transfers.',
ALIASES = ( 'group2' ) ,
PRIVS = ( 'FILE VIEW' ,...
```

Interpret the modified file:

```
config type=input,fname=$CFTDIRRUNTIME/conf/roles-smp.conf
listparm type=role,content=brief
Type ID Information
-------- -------------------- ---------------------------------------
ROLE TRANSFER CFT ADMINISTRATOR Enables full management of Transfer CFT.
ROLE TRANSFER CFT APPLICATION Enables applications to send transfers.
ROLE TRANSFER CFT DESIGNER
ROLE TRANSFER CFT HELPDESK
ROLE TRANSFER CFT PARTNERMANAGER
```

**Results**

- User1 has the custom Transfer CFT Administrator role defined in CFTROLE id='TRANSFER CFT ADMINISTRATOR',aliases=group1
- User2 has the custom Transfer CFT Application role defined in CFTROLE id='TRANSFER CFT APPLICATION',aliases=group2

****Example 2: Use only predefined roles and privileges****

If you configure UCONF as follows:

```
uconfset id=am.internal.group_database,value=system    NOTE: system (Unix, Windows,IBM i), xfbadm (Unix,HP NonStop)
uconfset id=am.internal.role.admin,value=group1
uconfset id=am.internal.role.application,value=group2
uconfset id=am.type,value=internal
```

To view the results:

```
listuconf id=am.internal.role.\*
 
U am.internal.role.admin = group1
D am.internal.role.helpdesk =
D am.internal.role.partnermanager =
D am.internal.role.designer =
U am.internal.role.application = group2
 
listparm type=role,content=brief
CFTU24W LISTPARM _ Warning ( Parameters no record selected / file empty)
```

To check that you are using only predefined roles and privileges, enter:

```
listparm type=role,content=brief
CFTU24W LISTPARM _ Warning ( Parameters no record selected / file empty)
```

**Results**

- User1 has the predefined Transfer CFT Administrator role
- User2 has the predefined Transfer CFT Application role

****Example 3: Uses both predefined and customized CFTPRIV and CFTROLE objects****

If you configure UCONF as follows:

```
uconfset id=am.internal.role.admin, value=group1
listuconf id=am.internal.role.\*
 
U am.internal.role.admin = group1
D am.internal.role.helpdesk =
D am.internal.role.partnermanager =
D am.internal.role.designer =
U am.internal.role.application =
listparm type=role,content=brief
Type ID Information
-------- -------------------- ---------------------------------------
ROLE TRANSFER CFT ADMINISTRATOR Enables full management of Transfer CFT.
ROLE TRANSFER CFT APPLICATION Enables applications to send transfers.
ROLE TRANSFER CFT DESIGNER
ROLE TRANSFER CFT HELPDESK
ROLE TRANSFER CFT PARTNERMANAGER
```

Modify the `conf/roles-smp.conf` sample file:

```
CFTROLE ID = 'TRANSFER CFT APPLICATION',
COMMENT = 'Enables applications to send transfers.',
ALIASES = ( 'group2' ) ,
PRIVS = ( 'FILE VIEW' ,...
```

Interpret the modified file:

```
config type=input,fname=$CFTDIRRUNTIME/conf/roles-smp.conf
listparm type=role,content=brief
Type ID Information
-------- -------------------- ---------------------------------------
ROLE TRANSFER CFT ADMINISTRATOR Enables full management of Transfer CFT.
ROLE TRANSFER CFT APPLICATION Enables applications to send transfers.
ROLE TRANSFER CFT DESIGNER
ROLE TRANSFER CFT HELPDESK
ROLE TRANSFER CFT PARTNERMANAGER
```

**Results**

- User1 has the predefined Transfer CFT Administrator role
- User2 has the custom Transfer CFT Application role defined in CFTROLE id='TRANSFER CFT APPLICATION',aliases=group2
