{
    "title": "Delivered Access Management exit samples",
    "linkTitle": "Delivered exit samples",
    "weight": "370"
}{{% TransferCFT/snippets/access_management%}}

Axway delivers an Access Management exit sample, examsmp1.c, in the `<CFTDIRRUNTIME>/src/exit` directory.

### Services provided by delivered sample

The delivered sample provides two services, authentication and permissions checking.


| Sample  | Authentication  | Permissions checking  |
| --- | --- | --- |
| examsmp1.c  | System authentication (Windows only)  | Flat file based on flat [RBAC Role Based Access Control]() model  |


### Building the dynamic library associated with the sample

To build the exit:

1. Change the directory to: &lt;CFTDIRRUNTIME&gt;/src/exit
1. Run the following command:

- UNIX: `make`
- Windows: `nmake -f exit.mak`

The output is a library located at `<CFTDIRRUNTIME>/lib/libcftexam.(so/dll)`.

Flat file based on flat RBAC 
-----------------------------

To check users rights, Axway delivers a sample flat file based on flat [RBAC Role Based Access Control]() (Role Based Access Control) located in: `<CFTDIRRUNTME>/conf/exam.csv`. This file contains a set of permission and user assignments.

![Simplied diagram of relationship between users, roles and permissions](/Images/TransferCFT/am_exits_rbac.GIF)

Assigning permission
--------------------

The following line shows how to add a permission to a role:  
`<cmd_type> <role> <resource> <actions> <policy>`

Where:


| Field  | Description  |
| --- | --- |
| &lt;cmd_type&gt;  | PA for Permission Assignment  |
| &lt;role&gt;  | The role for which the permission must be assigned  |
| &lt;resource&gt;  | Name of the resource  |
| &lt;actions&gt;  | List of actions with each action separated by a comma  |
| &lt;policy&gt;  | ACCEPT: accept the actions on the resource<br /> REFUSE: refuse the actions on the resource  |


****Examples****

All available actions on the resource “CONFIGURATION:CFTPARM”:

```
PA ADMIN CONFIGURATION:CFTPARM CREATE,DELETE,EDIT,VIEW ACCEPT
```

or

```
PA ADMIN CONFIGURATION:CFTPARM \* ACCEPT
```

All available actions on resources that start with “CONFIGURATION:” for the ADMIN role:

```
PA ADMIN CONFIGURATION:\* \* ACCEPT
```

All permissions for the ADMIN role:

```
PA ADMIN \* \* ACCEPT
```

All permissions for the ADMIN role except for the resource “TRANSFER”:

```
PA ADMIN TRANSFER \* REFUSE
PA ADMIN \* \* ACCEPT
```

Assigning users
---------------

The following line shows how to add a user to a role:  
`<cmd_type> <role> <users>`


| Field  | Description  |
| --- | --- |
| &lt;cmd_type&gt;  | UA for User Assignment  |
| &lt;role&gt;  | The role to which users must be assigned  |
| &lt;users&gt;  | List of users with each user separated by a comma  |


#### Examples

```
UA ADMIN admin,user01,user02
UA DESIGNER user03
UA HELPDESK user03,user04
UA APPLICATION user05
```

Predefined roles
----------------

You can find some roles defined in &lt;CFTDIRRUNTIME&gt;/conf/exam.csv.

****Predefined roles****


| Role  | Description  |
| --- | --- |
| Administrator  | Provides full user access  |
| Helpdesk  | Enables you to view the Catalog and Log  |
| Partner Manager  | Allows you to manage partners  |
| Designer  | Allows you to manage application flows  |
| Application  | Allows applications to request transfers and view the Catalog  |


The resources and available actions for {{< TransferCFT/axwayvariablesComponentShortName  >}} are listed in the PassPort AM CSD file.

After installing {{< TransferCFT/axwayvariablesComponentShortName  >}}, access the CSD file:

`<Transfer CFT install directory>/distrib/am/csd_Transfer_CFT.xml`

For more information, refer to the PassPort AM CSD.

****Related topics****

[About Access Management exits](../../../../internal_a_m_start_here/am_exits)

[Configuring an Access Management exit](../../../../internal_a_m_start_here/am_exits/configure_am_exits)
