---
title: "Configuring an Access Management exit"
linkTitle: "Configuring the exit"
weight: 190
--- This section describes how to configure access management when not using {{< TransferCFT/PrimaryCGorUM  >}}.

To enable an Access Management exit, set the ****uconf**** parameters described in this page.

From the Administration pane in the graphical user interface, select ****Unified Configuration****. The Unified Configuration window is displayed.

Double- click in a Unified Configuration window field to begin editing parameters.

Configure the AM exit using the parameters in the following table.

| Access Management exit parameters  | Value  | Description  |
| - - - | - - - | - - - |
| am.exit.libpath  |   | The absolute path of the dynamic library.  |
| am.exit.check_login  | Yes/No  | Indicate if the login must be checked through the AM exit.  |
| am.exit.check_permissions  | Yes/No  | Indicate if permissions must be checked though the AM exit.  |
| am.exit.custom.tracelevel.value  | Default: 1  | Trace level where:<br/> • 1=ERR<br/> • 2=WRN<br/> • 3=INF |
| am.exit.custom.rbac_fname.value  |   | Path to the RBAC flat file used by the Access Management exit sample.  |
| am.exit.custom.ldap_host.value  |   | LDAP server hostname/IP address that is used by the Access Management exit sample.  |
| am.exit.custom.ldap_port.value  |   | LDAP server port that is used by the Access Management exit sample.  |

> **Note**
>
> When the AM exit is enabled these conditions apply:

- uconf:am.exit.check_login=No: the native authentication procedure is still performed
- uconf:am.exit.check_permissions=No: all privileges are granted for the current user

Set the type parameter: `am.type = exit`

1. **Important**: Remember that the` am.type` is the last parameter to set when activating an AM exit and the first to unset when deactivating it.
1. Restart the {{< TransferCFT/axwayvariablesComponentShortName >}} server and {{< TransferCFT/axwayvariablesComponentShortName >}} UI (Copilot) server.

****Example Access Management exit configuration****

1. Using command line set the am.type to `none.`  
    **CFTUTIL UCONFSET ID=am.type, VALUE=none**
1. Configure the library location and the Access Management exit usage.  
    CFTUTIL UCONFSET ID=am.exit.libpath, VALUE=&lt;location_of_dynamic_library>  
    CFTUTIL UCONFSET ID=am.exit.check_login, VALUE=Yes  
    CFTUTIL UCONFSET ID=am.exit.check_permissions, VALUE=Yes
1. Enable the Access Management exit.  
    **CFTUTIL UCONFSET ID=am.type, VALUE=exit**

### Using the custom AM exit sample

This section presents parameters that you may need to set if you are creating an EXIT based on the ` examldap.c` sample. The `examldap.c` sample is built with the OpenLDAP library, which Axway does not deliver. To use, compile this library and add it to the LD_LIBRARY_PATH.

- am.exit.custom.ldap_base: The top level of the LDAP directory tree is the base, referred to as the "base DN". Enter the base (DN) to authenticate on the connected LDAP server, which defines which LDAP tree node to use as the root node. Example: `dc=example,dc=com`
- am.exit.custom.ldap_login_dn_format: The user DN format used to search for a user DN on the LDAP server.  
    For example, if a user is identified by cn=theUser,ou=people,dc=example,dc=com, set `am.exit.custom.ldap_login_dn_format` to `"cn=%s,ou=people,dc=example,dc=com"`
- am.exit.custom.ldap_get_roles_filter: The filter used to retrieve the groups that a given user belongs to.  
    For example, if the filter `"(&(objectClass=group)(member=cn=theuser,ou=people,dc=example,dc=com))"` returns the list of groups for the user "theUser", set `am.exit.custom.ldap_get_roles_filter` to `"(&(objectClass=group)(member=cn=%s,ou=people,dc=example,dc=com))"`

****Related topics****

[About Access Management exits](../)
