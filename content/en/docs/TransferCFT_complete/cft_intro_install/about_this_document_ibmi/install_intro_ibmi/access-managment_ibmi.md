{
    "title": "Define internal access management",
    "linkTitle": "Define internal access management",
    "weight": "250"
}This section describes the OS specific settings required to enable internal access management. As a prerequisite, you should read and be familiar with the [Internal access management](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/internal_access_mgt/internal_a_m_start_here.htm) information in the *Transfer CFT User Guide*.

Using system as the internal access management
----------------------------------------------

With ****system**** access management, you define the mapping between predefined roles and groups in UCONF and the user groups assignment in the GRPPRF field.

For system access management, you must create different Groups to associate with your users, creating a Groups in the same way as you do a user.

```
CRTUSRPRF USRPRF(ADMIN) TEXT('CFT admin group') SPCAUT(\*JOBCTL \*SPLCTL \*ALLOBJ)
CRTUSRPRF USRPRF(APPLI) TEXT('CFT application group') SPCAUT(\*NONE)
CRTUSRPRF USRPRF(DESIGNER) TEXT('CFT designer group') SPCAUT(\*NONE)
CRTUSRPRF USRPRF(HELPDESK) TEXT('CFT helpdesk group') SPCAUT(\*NONE)
CRTUSRPRF USRPRF(PARTMANAG) TEXT('CFT partnermanager group') SPCAUT(\*NONE)
```

You can then associate your users with the various groups you created.

****Example****

To grant CFTUSER1 access to transfers and the configuration, associate this user with the previously created HELPDESK group using the command:

```
CHGUSRPRF USRPRF(CFTUSER1) GRPPRF(HELPDESK)
```

You can now activate the access management in {{< TransferCFT/axwayvariablesComponentLongName  >}} according to your different groups:

```
UCONFSET id=am.type ,value = internal
UCONFSET id=am.internal.group_database ,value = system
UCONFSET id=am.internal.role.admin ,value = ADMIN
UCONFSET id=am.internal.role.application ,value = APPLI
UCONFSET id=am.internal.role.designer ,value = DESIGNER
UCONFSET id=am.internal.role.helpdesk ,value = HELPDESK
UCONFSET id=am.internal.role.partnermanager ,value = PARTMANAG
```
