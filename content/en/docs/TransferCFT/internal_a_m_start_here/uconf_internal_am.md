---

    title: Internal access management
    linkTitle: Internal access management
    weight: 160

---
This section describes how to configure Internal AM, which is a type of access management that you can use with or without {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} governance.

Internal access management is an out-of-the-box access management based on predefined roles and privileges, and a group internal datafile. Groups and their members are defined in this supplied database. Note however that when using this type of access management, there is no super user and the user who installed and starts the Transfer CFT Copilot server must have Administrator rights. <span id="security_base"></span>

The supplied pre-defined roles are:

- **Administrator**: Provides full user access
- **Helpdesk**: Enables you to view the catalog and log
- **Partner Manager**: Allows you to manage partners
- **Designer**: Allows you to manage application flows
- **Application**: Allows applications to request and manage transfers, and view the catalog

Additionally you can:

- **Custom**: Create new roles

Please refer to the [*Transfer CFT *{{< TransferCFT/axwayvariablesReleaseNumber  >}} *Security Guide*](https://docs.axway.com/bundle/TransferCFT_36_SecurityGuide_allOS_en_HTML5/page/Content/security_guide/predefined_privileges.htm) for a complete list of privileges and roles. Login is required. Additionally, the <a href="" class="MCXref xref"> </a> page describes three use cases and their configuration.

## Configuring internal access management

Set the specific group database parameter (see the table below for OS specifics) using CFTUTIL:

`uconfset id=am.internal.group_database,value=[ system | safClass | file | xfbadm  ]`

Use the parameters and descriptions in the <span class="bold_in_para">****AM Parameters****</span> table (just below Step 3) to help you customize the internal access management roles. For example, to assign the administrator role to the "admin" group:


| AM Parameters  | Default  | Description  |
| --- | --- | --- |
| am.internal.group_database  | file (z/OS)<br/> system (all other platforms) | Group database where group members are defined.<br/> • system (UNIX, Windows, and IBM i): the groups are defined in the OS group database (Unix, Windows, IBM i - see <a href="../../cft_intro_install/about_this_document_ibmi/install_intro_ibmi/access-managment_ibmi">security base</a>)<br/> • system (z/OS only): the service 'IRRSEQ00' is used to recall the user's groups from RACF, for example: • USER001 ADMIN OPERATOR PARTNER DESIGNER TRANSFER<br/><br/> • USER002 DESIGNER<br/><br/> <br/> • safClass (z/OS only): the resources are defined in the SAF (System Authorization Facility) where Transfer CFT maps the groups to resources - see the *<a href="../../cft_intro_install/about_this_document_zos">*Transfer CFT z/OS Installation and Operation Guide*</a>* for details<br/><br/> • file (z/OS): a variable file containing a users list and a groups list<br/> • xfbadm (UNIX, HP NonStop): the groups are defined in the xfbadm database; see <a href="" >Transfer CFT control utilities</a> |
| am.internal.group_database.fname  |   | If you set am.internal.group_database=file, you must define this file name, which is a variable file containing the groups associated with each user.<br/> For example:<br/> • USER001 group01 group02 group04<br/> • USER002 group04 group05<br/> Where the groups are mapped as shown in the example <a href="#Mapping">mapping</a> table below. |
| am.internal.role.admin  |   | Admin role and groups mapping. This role enables you to perform all administrative tasks.<br/> • List of groups (blank separator) |
| am.internal.role.helpdesk  |   | Help Desk role and groups mapping. This role enables you to view the log, transfers and configuration.<br/> • List of groups (blank separator) |
| am.internal.role.partnermanager  |   | Partner Manager role and groups mapping. This role enables you to create and manage partner.<br/> • List of groups (blank separator) |
| am.internal.role.designer  |   | Designer role and groups mapping. This role enables you to manage flows.<br/> • List of groups (blank separator) |
| am.internal.role.application  |   | Application role and groups mapping. This role enables application to send transfers.<br/> • List of groups (blank separator) |
| am.internal.persistence_timeout  | 300  | Delay in seconds between updating the list of group that a user belongs to. |


Set the access management type:

`uconfset id=am.internal.role.admin,value=admin`

`uconfset id=am.type,value=internal`

<span id="Mapping"></span>

## Mapping the group to predefined roles

To use the feature you will need to map the list of groups in the database to the {{< TransferCFT/axwayvariablesComponentShortName  >}} predefined roles. Use the following information as a basis for your mapping. You can enter these values either using command line or in the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI.


| Parameter  | Means the user in this group will have the role...  |
| --- | --- |
| am.internal.role.admin=group01  | The user who belongs to group “group01” has the admin role.  |
| am.internal.role.helpdesk=group02  | The user who belongs to group “group02” has the “helpdesk” role.  |
| am.internal.role.partnermanager=group03  | The user who belongs to group “group03” has the “partner manager” role.  |
| am.internal.role.designer =group04  | The user who belongs to group “group04” has the “designer” role.  |
| am.internal.role.application=group05  | The user who belongs to group “group05” has the “application” role.  |


## Creating or modifying roles

In addition to the out-of-the box roles, you can create new roles or modify existing roles using either the {{< TransferCFT/suitevariablesTransferCFTName  >}} user interface or a {{< TransferCFT/suitevariablesTransferCFTName  >}} configuration file. You can use new or modified roles on their own or in combination with predefined roles. Please refer to <a href="" class="MCXref xref"> </a> for role-based use case scenarios.

> **Note**
>
> When Central Governance is managing your Transfer CFTs, you should manage CFTROLE and CFTPRIVILEGE locally for each Transfer CFT.

### Using the configuration file

You can add, remove, or modify roles or privileges in the <span class="code">`role-smp.conf`</span> sample file delivered in <span class="code">`runtime/conf/`</span> directory. After adding or modifying a role or privilege, you must interpret the configuration file.

### Using the user interface

In the {{< TransferCFT/suitevariablesTransferCFTName  >}} UI, access the General Configuration pane and select either **Privileges** or **Roles**. Options include creating, deleting, modifying, or cloning a role or privilege.

 

### Role priority

When using a combination of predefined roles and custom roles that you created in the CFTPARM object, the new roles will override existing roles if they have the same name. For example, if you create two roles in CFTPARM with sames identifiers as predefined roles:

```
CFTROLE ID='HELPDESK', ...
CFTROLE ID='APPLICATION', ...
```

Here, the new HELPDESK and APPLICATION roles override the predefined HELPDESK and APPLICATION roles. However, the predefined ADMIN, PARTNERMANAGER, and DESIGNER roles are still used since you did not create new roles with these same names.

## Internal access management use cases

This section describes three configuration scenarios when using <a href="#" class="selected">internal access management</a> with roles and privileges. Use cases include:

- Predefined roles and privileges without the CFTPRIV and CFTROLE objects
- Customized roles and privileges with the CFTPRIV and CFTROLE objects
- Mixed-use of both predefined and custom CFTPRIV and CFTROLE objects

In each scenario, there is a **user1** that belongs to **group1** and a **user2** that belongs to **group2**.

****Example 1: Use only predefined roles and privileges****

If you configure UCONF as follows:

```
uconfset id=am.internal.group_database,value=system         NOTE: system (Unix,Windows), xfbadm (Unix,HP NonStop)
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

****Example 2: Custom roles and privileges using only CFTPRIV and CFTROLE objects****

If you configure UCONF as follows:

```
uconfset id=am.internal.group_database,value=system     NOTE: system (Unix,Windows), xfbadm (Unix,HP NonStop)
uconfset id=am.type,value=internal
listuconf id=am.internal.role.\*
 
D am.internal.role.admin =
D am.internal.role.helpdesk =
D am.internal.role.partnermanager =
D am.internal.role.designer =
D am.internal.role.application =
```

Modify the <span class="code">`conf/roles-smp.conf`</span> sample file:

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

Modify the <span class="code">`conf/roles-smp.conf`</span> sample file:

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
