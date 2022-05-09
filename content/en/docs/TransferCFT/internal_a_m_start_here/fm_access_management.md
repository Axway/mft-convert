{
    "title": "Access Management using Flow Manager",
    "linkTitle": "Flow Manager access management ",
    "weight": "140"
}You can use {{< TransferCFT/suitevariablesFlowManager  >}} to define and control Transfer CFT access management as described in the following sections.

****How it works****

If you have opted to use the {{< TransferCFT/suitevariablesFlowManager  >}} for {{< TransferCFT/suitevariablesTransferCFTName  >}} access management, after configuring both {{< TransferCFT/suitevariablesTransferCFTName  >}} and {{< TransferCFT/suitevariablesFlowManager  >}}:

1. A user logs in via the {{< TransferCFT/suitevariablesTransferCFTName  >}} user interface, CFTUTIL, or other.
1. A login request is sent to {{< TransferCFT/suitevariablesFlowManager  >}}.
1. If the login is successful, {{< TransferCFT/suitevariablesFlowManager  >}} returns the list of roles for the user.
1. The user login is complete. Transfer CFT then stores these roles in the cache and applies them accordingly. The information about this user is stored in the cache and is only updated when a new login is performed.

> **Note**
>
> Note: All role and permission definitions are stored in CFTPARM.

However, if you are an `am.superuser` user, {{< TransferCFT/suitevariablesTransferCFTName  >}} does not check your access for resources and permissions are granted unconditionally. Additionally, if you define a service account during {{< TransferCFT/axwayvariablesComponentLongName  >}} installation, this user is automatically added to the UCONF `am.superuser` parameter's list.

See also, {{< TransferCFT/suitevariablesFlowManager  >}} *Security Guide &gt;*[Predefined roles](https://docs.axway.com/bundle/FlowManager_20_allOS_en_HTML5/page/predefined_roles.html) and [Predefined privileges](https://docs.axway.com/bundle/FlowManager_20_allOS_en_HTML5/page/predefined_privileges.html) (requires account login).

![](/Images/TransferCFT/cg_am.jpg)

****Limitations****

- {{< TransferCFT/headerfootervariableshflongproductname  >}} ROLES are stored on {{< TransferCFT/headerfootervariableshflongproductname  >}} in upper case. This means that if you create roles **XXX** and **Xxx** on {{< TransferCFT/suitevariablesFlowManager  >}}, there is only one ROLE in {{< TransferCFT/headerfootervariableshflongproductname  >}}, which is `ID=XXX`.

<span id="Using"></span>

Using roles and privileges
--------------------------

The {{< TransferCFT/suitevariablesFlowManager  >}} method of access management impacts two {{< TransferCFT/suitevariablesTransferCFTName  >}} objects that are defined in the CFTPARM database: roles (CFTROLE) and privileges (CFTPRIV). You assign these roles and privileges in {{< TransferCFT/suitevariablesFlowManager  >}}, which are then deployed on Transfer CFT.

Conversely, you can create roles and privileges locally in Transfer CFT, as you do with other objects.

### Using CFTROLE

A role is a general profile that can be associated with a user. A role is based on one or more privileges, and a privilege is based on a resource. There are two types of roles: predefined and user-defined. Predefined roles are available by default to assign to users.

You can assign users to one or more roles. Typically, users with multiple roles have more privileges than users with fewer roles.

Examples of roles can be ADMINISTRATOR, PARTNER MANAGER, IT MANAGER, and so on.


| Field | Type | Comment |
| --- | --- | --- |
| id | String32 | Role identifier |
| comment | String80 | Comment |
| privs[] | List of String32 | List of privileges associated to this role (1 to 128) |


Example of CFTROLE in a configuration file:

```
CFTROLE      ID          = 'Application',
             COMMENT     = 'My comments',
             PRIVS       = ( 'SERVICE:UI_CONNECT',
                             'MYPRIV1',
                             'CONFIGURATION:CFTCOM_VIEW',
                             'CONFIGURATION:CFTPARM_VIEW',
                             'CONFIGURATION:CFTPART_VIEW',
                             'CONFIGURATION:CFTDEST_VIEW',
                             'CONFIGURATION:CFTSEND_VIEW',
                             'CONFIGURATION:CFTRECV_VIEW',
                             'CONFIGURATION:CFTLOG_VIEW',
                             'FILTER:CATALOG_ALL',
                             'FILTER:LOG_ALL',
                             'FILE_VIEW'),
             ORIGIN      = 'CFTUTIL',
             MODE        = 'REPLACE'
```

### Using CFTPRIV

Privileges give users authorization to access and perform actions in the user interface. Examples of actions include CREATE, DELETE, VIEW, EDIT (use \* to assign all actions).


| Field | Type | Comment |
| --- | --- | --- |
| id | String32 | Privilege identifier |
| comment | String80 | Comment |
| resource | String32 | Resource on which this privilege applies |
| actions | List of String32 | Actions authorized on the resource (1 to 16 actions) |
| condition | String256 | Condition to check for authorizing ([see below](#Specifyi)) |


Example of CFTPRIV in a configuration file:

```
CFTPRIV      ID          = 'MYPRIV1',
            COMMENT     = 'My comment',
             RESOURCE    = 'TRANSFER',
             ACTIONS     = ( 'CREATE' , 'DELETE', 'VIEW', 'EDIT', 'CANCEL', 'RESUME',
                            'PAUSE', 'EXECUTE', 'SUBMIT', 'END' ),
             CONDITION   = '',
             ORIGIN      = 'CFTUTIL',
             MODE        = 'REPLACE'
```
<span id="Specifyi"></span>

### Specifying conditions

Conditions allow you to assign finer control on resources and actions by specifying a logical condition that must be true to authorize the action.

****Examples****

In these examples `PART `and `ID `are properties of the resource being checked. As you can see, you can use parenthesis and logical operators `&&` (AND) and `ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù` (OR).

```
PART=="PARIS" && ID=="IDFDEF"
(PART=="PARIS" ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù PART==”NEWYORK”) && ID~="IDF\*"
```

Comparison operators include:

- == : equals
- != : not equal
- ~= : matches (use \* and ? for jokers)
- /= : not matching (use \* and ? for jokers)
- &lt; : inferior to
- &gt; : superior to
- &lt;= : inferior or equal to
- &gt;= :superior or equal to

### Resource properties in privileges

The following table is an exhaustive list of all properties for all resources. These properties are available regardless of the action to be checked. However, if a resource has no properties, setting a condition for it has no impact.


| Resource | Actions | Properties |
| --- | --- | --- |
| CONFIGURATION:PKICER | CREATE, DELETE, VIEW, EDIT, ACTIVATE, DEACTIVATE | ID |
| CONFIGURATION:PKIENTITY | CREATE, DELETE, VIEW, EDIT, ACTIVATE, DEACTIVATE | ID |
| CONFIGURATION:PKIKEY | CREATE, DELETE, VIEW, EDIT, ACTIVATE, DEACTIVATE | ID |
| CONFIGURATION:CFTPARM | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTNET | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTPROT | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION: CFTSEND | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION: CFTSENDI <sup>(1)</sup> | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION: CFTRECV | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTAUTH | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTXLATE | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTLOG | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTCAT | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTCOM | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTACCNT | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTEXIT | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTIDF | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTFOLDER | CREATE, DELETE, VIEW, EDIT, ACTIVATE, DEACTIVATE | ID |
| CONFIGURATION:CFTETB | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTAPPL | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTSSL | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTCRON | CREATE, DELETE, VIEW, EDIT, ACTIVATE, DEACTIVATE, RELOAD | ID |
| CONFIGURATION:CFTPART | CREATE, DELETE, VIEW, EDIT, ACTIVATE, DEACTIVATE, TURN | ID |
| CONFIGURATION:CFTDEST | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTTCP | CREATE, DELETE, VIEW, EDIT | ID |
| CONFIGURATION:CFTUCONF | DELETE, VIEW, EDIT | ID |
|   |   |   |
| SERVICE:BATCH <sup>(2)</sup> | EXECUTE | ID, FNAME |
| SERVICE:UI <sup>(3)</sup> | CONNECT | TYPE, ID, GROUP |
| SERVICE:BATCH | EXECUTE | ID, FNAME |
| SERVICE:CATALOG | PURGE |   |
| SERVICE:LOG | SWITCH |   |
| SERVICE:ACCOUNT | SWITCH |   |
| SERVICE:CFTSRV | STARTUP, SHUTDOWN |   |
| SERVICE:COM | DELETE, VIEW |   |
|   |   |   |
| ****COMMAND:EXTRACT**** | EXECUTE |   |
| ****COMMAND:MQUERY**** | EXECUTE |   |
| ****COMMAND:TURN**** | EXECUTE |   |
| ****COMMAND:CFTSUPPORT**** | EXECUTE, VIEW, DELETE  |   |
| TRANSFER | CREATE, DELETE, VIEW, EDIT, CANCEL, RESUME, PAUSE, EXECUTE, SUBMIT, END, VIEWIFLE, EDITFILE, DELETEFILE | IDAPPL, ID, PART, SPART, RPART, IPART, TYPE, DIRECT, MODE, FNAME, MESSAGE, SUSER, RUSER, SAPPL, RAPPL, NFNAME |
| FILTER:CATALOG | CREATE, DELETE, VIEW, EDIT | ID |
| FILTER:LOG | CREATE, DELETE, VIEW, EDIT | ID |
| FILE <sup>(5)</sup> | CREATE, DELETE, VIEW, EDIT | FNAME |
| URL <sup>(6)</sup> | VIEW | URL |


1. Manage implicit SEND object definitions.
1. Manage batch files.
1. Connect to user interface.
1. Rights from PassPort AM.
1. Manage Transfer CFT file definition objects.
1. Manage file name definitions.

Use cases
---------

### Restrict user to a specific partner and IDF

As an Administrator, you want to authorize a user to work only with a given partner and a specific IDF.

This user can perform the following operations only with an IDF named "MYIDF" and the PART named "MYPART":

- View transfers (LISTCAT and DISPLAY)
- Create transfers
- Delete transfers
- Cancel transfers
- Resume transfers
- Execute transfers

In this use case, you assign the user a role that references a privilege having these characteristics:

- RESOURCE = 'TRANSFER',
- ACTIONS = ( ‘VIEW’, ‘CREATE’, ‘DELETE’, ‘CANCEL', 'RESUME', ‘EXECUTE’ ),
- CONDITION = ' IDF=="MYIDF" && PART=="MYPART" '

The following is an example of the {{< TransferCFT/suitevariablesTransferCFTName  >}} configuration for this use case, where the ROLE must exist in {{< TransferCFT/suitevariablesFlowManager  >}}, and be available for required users:

```
CFTROLE      ID          = ' TRANSFER-ROLE
',
             COMMENT     = '',
/\*           ALIASES     = ( ) ,\*/
             PRIVS       = ( ' PRIV-XFER-SPE
',
                              'PRIV-CONN-INTERFACES',
                              'CONFIGURATION:CFTCOM_VIEW',
                              'CONFIGURATION:CFTPARM_VIEW',
                              'FILTER:CATALOG_ALL',
                              'FILTER:LOG_ALL',
                              'FILE_VIEW',
                              'CONFIGURATION:PKICER_VIEW',
                              'CONFIGURATION:PKIENTITY_VIEW',
                               'CONFIGURATION:PKIKEY_VIEW',
                              'CONFIGURATION:CFTPARM_VIEW',
                              'CONFIGURATION:CFTNET_VIEW',
                              'CONFIGURATION:CFTPROT_VIEW',
                              'CONFIGURATION:CFTSEND_VIEW',
                              'CONFIGURATION:CFTSENDI_VIEW',
                              'CONFIGURATION:CFTRECV_VIEW',
                              'CONFIGURATION:CFTAUTH_VIEW',
                              'CONFIGURATION:CFTXLATE_VIEW',
                              'CONFIGURATION:CFTLOG_VIEW',
                              'CONFIGURATION:CFTCAT_VIEW',
                              'CONFIGURATION:CFTCOM_VIEW',
                              'CONFIGURATION:CFTACCNT_VIEW',
                              'CONFIGURATION:CFTEXIT_VIEW',
                              'CONFIGURATION:CFTIDF_VIEW',
                              'CONFIGURATION:CFTFOLDER_VIEW',
                              'CONFIGURATION:CFTETB_VIEW',
                              'CONFIGURATION:CFTAPPL_VIEW',
                              'CONFIGURATION:CFTSSL_VIEW',
                              'CONFIGURATION:CFTCRON_VIEW',
                              'CONFIGURATION:CFTPART_VIEW',
                              'CONFIGURATION:CFTDEST_VIEW',
                              'CONFIGURATION:CFTTCP_VIEW',
                              'CONFIGURATION:CFTUCONF_VIEW',
                              'SERVICE:COM_VIEW'),
              MODE        = 'REPLACE'
 
CFTPRIV       ID          = ' PRIV-XFER-SPE
',
              COMMENT     = 'PRIV limits transfers - no delete condition',
              RESOURCE    = 'TRANSFER',
              ACTIONS     = ( 'CREATE',
                              'RESUME',
                              'VIEW',
                              'CANCEL',  
                               'PAUSE',
                              'EXECUTE'),
              CONDITION   = ' IDF=="MYIDF" && PART=="MYPART" ',
              ORIGIN      = 'CFTUTIL',
              MODE        = 'REPLACE'
```

### Add an administrator role

As an administrator, you want to assign a classic 'Administrator' role to a user, but would like to restrict access in the UI (Copilot or CFTUI) to a given Transfer CFT or group of Transfer CFTs based on the Transfer CFT instance ID and group.

In this use case, you assign the user role that refers to a privilege having these characteristics:

- RESOURCE = SERVICE:UI,
- ACTIONS = ( 'CONNECT' ),
- CONDITION = ' GROUP=="PRODUCTION" && ID~=''CFT-PROD-ITEM\*'' '

A user with this privilege can only connect to a Transfer CFT server whose UCONF `cft.instance_group` value is set to PRODUCTION, and whose `cft.instance_id` value begins with CFT-PROD-ITEM.

The following is an example of the {{< TransferCFT/suitevariablesTransferCFTName  >}} configuration for this use case (the ROLE must exist in {{< TransferCFT/suitevariablesFlowManager  >}}, and be available for required users):

```
CFTROLE      ID          = ' ADMIN_ROLE
',
              COMMENT     = 'Administrator role for Production Transfer CFT Windows',
              PRIVS       = ('PRIV-CONN-INTERFACES',               
                              'CONFIGURATION:PKICER_ALL',
                              'CONFIGURATION:PKIENTITY_ALL',
                              'CONFIGURATION:PKIKEY_ALL',
                              'CONFIGURATION:CFTPARM_ALL',
                              'CONFIGURATION:CFTNET_ALL',
                              'CONFIGURATION:CFTPROT_ALL',
                              'CONFIGURATION:CFTSEND_ALL',
                              'CONFIGURATION:CFTSENDI_ALL',
                              'CONFIGURATION:CFTRECV_ALL',
                              'CONFIGURATION:CFTAUTH_ALL',
                              'CONFIGURATION:CFTXLATE_ALL',
                              'CONFIGURATION:CFTLOG_ALL',
                              'CONFIGURATION:CFTCAT_ALL',
                              'CONFIGURATION:CFTCOM_ALL',
                               'CONFIGURATION:CFTACCNT_ALL',
                              'CONFIGURATION:CFTEXIT_ALL',
                              'CONFIGURATION:CFTIDF_ALL',
                              'CONFIGURATION:CFTFOLDER_ALL',
                              'CONFIGURATION:CFTETB_ALL',
                              'CONFIGURATION:CFTAPPL_ALL',
                              'CONFIGURATION:CFTSSL_ALL',
                              'CONFIGURATION:CFTCRON_ALL',
                              'CONFIGURATION:CFTPART_ALL',
                               'CONFIGURATION:CFTDEST_ALL',
                              'CONFIGURATION:CFTROLE_ALL',
                              'CONFIGURATION:CFTPRIV_ALL',
                              'CONFIGURATION:CFTTCP_ALL',
                               'CONFIGURATION:CFTUCONF_ALL',
                              'SERVICE:CATALOG_PURGE',
                              'SERVICE:LOG_SWITCH',
                              'SERVICE:ACCOUNT_SWITCH',
                              'SERVICE:BATCH_EXECUTE',
                               'SERVICE:CFTSRV_ALL',
                              'SERVICE:COM_ALL',
                              'TRANSFER_ALL',
                              'FILTER:CATALOG_ALL',
                              'FILTER:LOG_ALL',
                              'FILE_ALL',
                              'URL_VIEW',
                              'COMMAND:EXTRACT_ALL',
                              'COMMAND:MQUERY_ALL',
                              'COMMAND:TURN_ALL',
                              'COMMAND:CFTSUPPORT_ALL'),
              MODE        = 'REPLACE'
 
CFTPRIV      ID          = ' PRIV-CONN-INTERFACES
',
              COMMENT     = 'PRIV LIMITs the connection for a given Transfer CFT name',
             RESOURCE    = 'SERVICE:UI',
             ACTIONS     = ( 'CONNECT'),
             CONDITION   = 'GROUP=="PRODUCTION" && ID~="CFT-PROD-ITEM\*"', 
             MODE        = 'REPLACE'
 
```

A user with this privilege can only connect to a Transfer CFT server whose UCONF `cft.instance_group` value is set to `PRODUCTION`, and whose `cft.instance_id` value begins with `CFT-PROD-ITEM`.
