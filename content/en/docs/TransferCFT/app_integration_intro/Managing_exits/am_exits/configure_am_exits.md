---

    title: Configuring an Access Management exit
    linkTitle: Configuring an AM exit
    weight: 360

---
This section describes how to configure access management when not using {{< TransferCFT/PrimaryCGorUM  >}}.

To enable an Access Management exit, set the <span class="bold_in_para">****uconf**** </span>parameters described in this page.

From the Administration pane in the graphical user interface, select<span class="bold_in_para"> ****Unified Configuration****</span>. The <span class="italic_in_para">Unified Configuration</span> window is displayed.

Double-click in a <span class="italic_in_para">Unified Configuration</span> window field to begin editing parameters.

Configure the AM exit using the parameters in the following table.


| Access Management exit parameters  | Value  | Description  |
| --- | --- | --- |
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

- uconf:am.exit.check\_login=No: the native authentication procedure is still performed
- uconf:am.exit.check\_permissions=No: all privileges are granted for the current user

Set the type parameter: <span class="code">`am.type = exit`</span>

1. **Important**: Remember that the<span class="code">` am.type`</span> is the last parameter to set when activating an AM exit and the first to unset when deactivating it.
1. Restart the {{< TransferCFT/axwayvariablesComponentShortName >}} server and {{< TransferCFT/axwayvariablesComponentShortName >}} UI (Copilot) server.

****Example Access Management exit configuration****

1. Using command line set the am.type to <span class="code">`none.`</span>  
    **CFTUTIL UCONFSET ID=am.type, VALUE=none**
1. Configure the library location and the Access Management exit usage.  
    CFTUTIL UCONFSET ID=am.exit.libpath, VALUE=&lt;location\_of\_dynamic\_library>  
    CFTUTIL UCONFSET ID=am.exit.check\_login, VALUE=Yes  
    CFTUTIL UCONFSET ID=am.exit.check\_permissions, VALUE=Yes
1. Enable the Access Management exit.  
    **CFTUTIL UCONFSET ID=am.type, VALUE=exit**

### Using the custom AM exit sample

This section presents parameters that you may need to set if you are creating an EXIT based on the <span class="code">` examldap.c`</span> sample. The <span class="code">`examldap.c`</span> sample is built with the OpenLDAP library, which Axway does not deliver. To use, compile this library and add it to the LD\_LIBRARY\_PATH.

- am.exit.custom.ldap\_base: The top level of the LDAP directory tree is the base, referred to as the "base DN". Enter the base (DN) to authenticate on the connected LDAP server, which defines which LDAP tree node to use as the root node. Example: <span class="code">`dc=example,dc=com`</span>
- am.exit.custom.ldap\_login\_dn\_format: The user DN format used to search for a user DN on the LDAP server.  
    For example, if a user is identified by cn=theUser,ou=people,dc=example,dc=com, set <span class="code">`am.exit.custom.ldap_login_dn_format`</span> to <span class="code">`"cn=%s,ou=people,dc=example,dc=com"`</span>
- am.exit.custom.ldap\_get\_roles\_filter: The filter used to retrieve the groups that a given user belongs to.  
    For example, if the filter <span class="code">`"(&(objectClass=group)(member=cn=theuser,ou=people,dc=example,dc=com))"`</span> returns the list of groups for the user "theUser", set <span class="code">`am.exit.custom.ldap_get_roles_filter`</span> to <span class="code">`"(&(objectClass=group)(member=cn=%s,ou=people,dc=example,dc=com))"`</span>

****Related topics****

[About Access Management exits](../../../../internal_a_m_start_here/am_exits)
