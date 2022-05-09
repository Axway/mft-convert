{
    "title": "CFTROLE",
    "linkTitle": "Roles - CFTROLE",
    "weight": "250"
}Set the roles, which define a type of user and application permission. See also [Access Management using Flow Manager](../../../../internal_a_m_start_here/fm_access_management)

### Using CFTROLE

A role is a general profile that can be associated with a user. A role is based on one or more privileges, and a privilege is based on a resource. There are two types of roles: predefined and user-defined. Predefined roles are available by default to assign to users.

You can assign users to one or more roles. Typically, users with multiple roles have more privileges than users with fewer roles.

Examples of roles can be ADMINISTRATOR, PARTNER MANAGER, IT MANAGER, and so on.


| Field | Type | Comment |
| --- | --- | --- |
| id | String32 | Role identifier |
| comment | String80 | Comment |
| privs[] | List of String32 | List of privileges associated to this role (1 to 128) |
| aliases  | List of String64  | List of aliases associated with this role  |


**Example 1**

CFTROLE in a configuration file:

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

**Example 2**

Alias definitions that have a correspondence between roles created in an existing IDP (Identity Provider) and roles created with CFTROLE object.

```
CFTROLE ID = 'TRANSFER CFT APPLICATION',
  COMMENT = 'Enables applications to send transfers.',
  ALIASES = ( 'cft_appli' , 'group_appli') ,
  PRIVS = ( 'FILE VIEW' ,... ),...
```

**Example 3**

The 'TRANSFER CFT APPLICATION' role can be also referenced as either cft_appli or group_appli. However, you cannot specify the same ALIAS in different roles.

```
CFTROLE ID = 'TRANSFER CFT APPLICATION',
  COMMENT = 'Enables applications to send transfers.',
  ALIASES = ( 'cft_appli' , ' group_appli
') ,...
CFTROLE ID = 'TRANSFER CFT ADMINISTRATOR',
  COMMENT = 'Enables full management of Transfer CFT.',
  ALIASES = ( ' group_appli
' , 'cft_admin' ),...
```
