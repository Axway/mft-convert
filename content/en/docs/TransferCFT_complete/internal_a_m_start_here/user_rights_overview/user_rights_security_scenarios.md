{
    "title": "User rights and privileges use cases",
    "linkTitle": "User rights and privileges use cases",
    "weight": "240"
}This topic describes how you can add mounting levels of security to your Transfer CFT environment, and features 4 typical user types. In all but the first example, no added security, the user types are defined by Central Governance roles. By adding file and user controls you can add increasing security controls.

For more information on roles and privileges in Central Governance, refer to the *Central Governance User Guide*.

In this section:

- User scenario with no security applied
- [User types](#User): Describes the actions that example users can perform
- [Use case 1](#Security): Security controlled by Central Governance roles
- [Use case 2](#Security2): Security controlled by USERCTRL and file rights
- [Use case 3](#Security3): Security controlled by copilot.misc.createprocessasuser
- [Define additional user rights security option](#Define)

<span id="Use"></span>

Use case with no security applied
---------------------------------

In this use case, there is no security enabled either in Transfer CFT or Central Governance (`am.type=none`). However, normally when using Central Governance with Transfer CFT the default value is set to `am.type=passport`. This results in no control over the transfer owner.

<span id="User"></span>

User types
----------

This section presents example user types, and describes the actions that they can perform given the Central Governance roles and systems rights. Machine1 represents a system with an installed Transfer CFT and user directories.

These scenarios are based on a single {{< TransferCFT/headerfootervariableshflongproductname  >}}, **Machine1** in our examples, that is managed by {{< TransferCFT/PrimaryCGorUM  >}}.


| User type  | CG role(s)  | Machine1 user  | File access  |
| --- | --- | --- | --- |
| Monitoring Assistant | Help Desk<br/> for Transfer CFT | Not defined  | N/A  |
| Operator  | IT Manager for CG  | Defined  | All permissions on runtime files, but does not have access to other user's working directories |
| Partner Manager  | CG Admin  | Not defined  | No privileges on the physical files  |
| Flow Manager  | Middleware<br/> Manager for CG, and Application for Transfer CFT | Defined  | Rights on Machine1, and his own working directory  |
| Superuser  | N/A  | Defined  | Rights on all runtime files on Machine1, but no rights on user's working directories  |


Remember that these are examples and your system users, assigned roles, and file rights will vary.

> **Note**
>
> Note: When referring to user's working directories, in these use cases the working directories are located outside of the runtime directory.

<span id="Security"></span>

Security controlled by Central Governance roles
-----------------------------------------------

In this security scenario, the central governance roles are the exclusive defining security system (i.e. the parameters USERCTRL and copilot.misc are set to NO, the default settings).

> **Note**
>
> Tip  
> The transfer owner in this scenario is the user that started Transfer CFT. All actions are done by the user that started the Copilot server, pending rights given by the Central Governance roles. This applies to all of the registered Central Governance users. The superuser however can perform actions only by using CFTUTIL, but not via the Transfer CFT UI.

#### Monitoring Assistant

The help desk cannot monitor Transfer CFT through Central Governance if they have no other role assigned to them.

- Perform monitoring: YES, but only via a Transfer CFT client (not using Central Governance visibility services)
- Submit a transfer: NO
- Connect to Transfer CFT UI: YES
- Modify configuration: NO 
- Start/stop Transfer CFT: NO

#### Operator

- Monitor: YES, but only using Central Governance
- Transfer: NO
- Connect to Transfer CFT UI: NO
- Modify configuration: YES, but only through Central Governance
- Start/stop Transfer CFT: YES

#### Partner Manager

- Monitor: YES, but only through Central Governance
- Transfer: NO
- Connect to Transfer CFT UI: NO
- Modify configuration: NO
- Start/stop Transfer CFT: NO

#### Flow Manager

- Monitor: YES, but only by using Central Governance
- Transfer: YES, can access and perform transfers using any user's working directory
- Connect to Transfer CFT UI: YES
- Modify configuration: YES, via Central Governance only
- Start/stop Transfer CFT: NO

#### System Engineer (superuser)

Using CFTUTIL this user can perform configuration actions and transfers, but cannot do anything from {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.

- Monitor: YES
- Transfer: YES
- Connect to Transfer CFT UI: NO
- Modify configuration: YES
- Start/stop Transfer CFT: YES, but only via CFTUTIL

<span id="Security2"></span>

Security controlled by USERCTRL and file rights
-----------------------------------------------

The following scenario consists of a single {{< TransferCFT/headerfootervariableshflongproductname  >}} with the USERCTRL parameter set to **yes**.

> **Note**
>
> Note: Reminder, when copilot.misc.createprocessasuser=no, the user may be known by Central Governance, though not necessarily known by Machine1. All actions in Transfer CFT client are done as if the user was the user who started server.

USERCTRL is set to YES and file rights are assigned to each specific type of user. Rights depend on user/role type (limitation).

#### Monitoring Assistant

Help desk alone cannot monitor Transfer CFT through Central Governance if they have no other role assigned to them.

- Perform monitoring: YES, but only via a Transfer CFT client (not using CG visibility services)
- Submit a transfer: NO
- Connect to Transfer CFT UI: YES
- Modify configuration: NO 
- Start/stop Transfer CFT: NO

#### Operator

- Monitor: YES, but only through Central Governance
- Transfer: NO
- Connect to Transfer CFT UI: NO
- Modify configuration: YES, but only through CG.
- Start/stop Transfer CFT: YES

#### Partner Manager

- Monitor: YES, but only via Central Governance
- Transfer: NO
- Connect to Transfer CFT UI: NO
- Modify configuration: NO
- Start/stop Transfer CFT: NO

#### Flow Manager

- Monitor: YES, via Central Governance only
- Transfer: YES, but is limited to his own working directory
- Connect to Transfer CFT UI: YES
- Modify configuration: YES, via Central Governance only
- Start/stop Transfer CFT: NO

#### System Engineer (superuser)

Using CFTUTIL this user can perform configuration actions and transfers, but cannot do anything from {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.

- Monitor: YES, but only using CFTUTIL
- Transfer: NO, because he cannot access (no rights on) the user's working directories
- Connect to Transfer CFT UI: NO
- Modify configuration: YES
- Start/stop Transfer CFT: YES, but only via CFTUTIL

> **Note**
>
> Note: When copilot.misc.createprocessasuser=no, the user may be known on Central Governance, but not necessarily known on Machine1. All actions in Transfer CFT client are done as if the user was the user who started server.

<span id="Security3"></span>

Security controlled by copilot.misc.createprocessasuser
-------------------------------------------------------

On top of the previous security steps, additionally you have set copilot.misc.createprocessasuser to YES.

- The log in connection is a system check and not a CG check
- All actions on files that Copilot can access are performed on behalf of the user connected to Copilot

#### Monitoring Assistant

Help desk alone cannot monitor Transfer CFT through Central Governance if they have no other role assigned to them.

- Perform monitoring: NO, this user is not defined on Machine1
- Submit a transfer: NO
- Connect to Transfer CFT UI: NO
- Modify configuration: NO 
- Start/stop Transfer CFT: NO

#### Operator

- Monitor: YES, through either Central Governance or Copilot
- Transfer: NO
- Connect to Transfer CFT UI: NO
- Modify configuration: YES
- Start/stop Transfer CFT: YES

#### Partner Manager

- Monitor: YES, but only through CG.
- Transfer: NO
- Connect to Transfer CFT UI: NO
- Modify configuration: NO
- Start/stop Transfer CFT: NO

#### Flow Manager

- Monitor: YES, via CG only.
- Transfer: YES, but is limited to his own working directory.
- Connect to Transfer CFT UI: YES
- Modify configuration: YES, via CG only.
- Start/stop Transfer CFT: NO

#### System Engineer (superuser)

Using CFTUTIL this user can perform configuration actions and transfers, but cannot do anything from {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.

- Monitor: YES, but only using CFTUTIL 
- Transfer: NO, because he cannot access (no rights on) the user's working directories
- Connect to Transfer CFT UI: NO
- Modify configuration: YES, using either CFTUTIL or Copilot
- Start/stop Transfer CFT: YES

<span id="Define"></span>

Define additional user rights security option
---------------------------------------------

This example describes how to add an additional user rights security restriction. The user in this case is not known on Central Governance, but has all rights on all files on the Transfer CFT system, runtime as well as working directories.

- When am.passport.userctrl.check_permissions_on_transfer_execution=no, the default value, this user, who defined on the Machine1, can use CFTUTIL to perform a transfer even though not known on {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.
- When am.passport.userctrl.check_permissions_on_transfer_execution=yes, this same user cannot perform transfers as he is not defined in {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.

****Related topics****

- [About system users](../)
- [Recommendations and troubleshooting](../user_rights_tips)
