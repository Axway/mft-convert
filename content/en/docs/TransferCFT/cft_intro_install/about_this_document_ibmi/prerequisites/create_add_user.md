---
title: "Create an additional user "
linkTitle: "Create additional user "
weight: 210
--- ## Manage user rights

System managers can use one of several methods to block Transfer CFT usage, as a system administrator may need to prohibit the general use of Transfer CFT IBM i.

<span id="kanchor16"></span>One way for system administrators to manage users is to assign the right to execute Transfer CFT to any user with a group profile (\*GRPPRF). Another is to give the Transfer CFT profile password only to certain users.

If Transfer CFT is used by several profiles, issues over object rights can occur, possibly affecting product operations. To grant specific authority for an object to a user or group, you may need to use the GRTOBJAUT command.

Additionally, when these programs create a file dynamically they grant all users [USER(\*PUBLIC)] the default authorization(AUT(\*LIBCRTAUT)

In a live environment, the security manager can modify:

- Program usage rights
- User profile usage rights
- Update rights for Transfer CFT service files (PARM, PART, CAT, COM, and optionally LOG and ACCNT files)
- Update rights for source members, such as:
- Source files that can be interpreted (particularly the configuration source file)
- Source files to be submitted by Transfer CFT at the end of the transfer

The method used to address security issues for specific rights depends on the:

- Number and diversity of the user profiles concerned by file transfers
- Required level of data protection, since most constraints when using the product are associated with data confidentiality issues

There are no preset security rules, as Transfer CFT security issues are closely associated with the administration of the system on which Transfer CFT is installed.

You can create an additional user that interacts with the Transfer CFT instance. For example, you may need an additional user related to a dedicated application. In this section, the example user account is APP1.

1. Create a user.
1. Give the user permission to use Transfer CFT.
1. Update the user profile.
1. Give the user permission to use Transfer CFT with {{< TransferCFT/PrimaryCGorUM >}}.

## Create an APP1 user

It is up to the system administrator to make the decision to create a Transfer CFT- specific user profile. We recommend that if possible you use this type of profile to simplify security management, execution environment generation, and Transfer CFT operations.

Unless you wish to differentiate between several Transfer CFT IBM i instances running concurrently on the same system, you should use the Transfer CFT profile name (USRPRF = APP1). If you decide to use Transfer CFT as a profile, you must create this Transfer CFT profile prior to installing the product.

The profile used to install the product must be in the \*SECOFR class.

The APP1 user has the following special authorities:

- \*JOBCTL
- \*SPLCTL

****Example****

```
CRTUSRPRF USRPRF(APP1) PASSWORD(APP1) PWDEXP(\*YES) USRCLS(\*USER) INLPGM(\*NONE) INLMNU(MAIN) LMTCPB(\*NO) TEXT('\*SHARED: APP1 Profile') SPCAUT(\*JOBCTL \*SPLCTL) PWDEXPITV(\*SYSVAL)
```

## Grant the APP1 user permissions

To avoid being blocked by the execution rights for commands used in Transfer CFT programs, you must assign user rights to the Transfer CFT profile.

You can use the delivered CHGOWNCFT and GRTOBJCFT commands as aids in granting permissions, with the library and profile name as call parameters.

### Using GRTOBJCFT

The GRTOBJCFT command is based on the GRTOBJAUT command, and it revokes authority, grants the \*ALL authority to the selected user, and gives the \*USE authority to \*PUBLIC users of objects in the selected library.

Alternatively, you can use the GRTOBJAUT system command directly. Enter the following command to grant this user the permission to use Transfer CFT at the system level.

```
GRTOBJAUT OBJ(CFTPGM/\*ALL) OBJTYPE(\*ALL) USER(APP1) AUT(\*ALL)
GRTOBJAUT OBJ(CFTPROD/\*ALL) OBJTYPE(\*ALL) USER(APP1) AUT(\*ALL)
```

### Using CHGOWNCFT

CHGOWNCFT applies a CHGOBJOWN command to give object ownership to the selected user.

You must give \*RX rights to all IFS files created in the `/home/cft/Transfer_CFT/install` (default) and `/home/cft/Transfer_CFT/runtime` (default) directories.

The system environment of the user who performs the installation procedure is applied by default if there is no Transfer CFT- specific profile applied during installation. The user profile owns the objects created during installation, but not restored objects such as programs, commands, etc.

## Update the APP1 profile to use Transfer CFT instance

```
CHGUSRPRF USRPRF(APP1) JOBD(CFTPROD/CFTJOBD)
```

## Grant the APP1 user Central Governance rights

You must create a user (for example, APP1) in Central Governance before you can use Transfer CFT in Central Governance. As this user is not a super user, assign the APP1 user a Transfer CFT role. For example, you can assign it the default role "Transfer CFT Administrator".

> **Note**
>
> The credentials required to log on to the Transfer CFT Copilot server are the system credentials. The password set in Central Governance for APP1 is not used.

You can now connect to the Copilot server using the APP1 user.
