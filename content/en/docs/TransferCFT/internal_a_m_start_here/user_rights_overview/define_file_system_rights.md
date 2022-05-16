---

    title: 2. Define file permissions
    linkTitle: 2. Define file permission
    weight: 220

---
This section describes how to define read and write rights. The following points indicate when or why you may want to set these file permissions:

- Regardless of the USERCTRL setting, if you need for any user other than the user that installed the Transfer CFT to start Transfer CFT or the Transfer CFT Copilot server, you must define file rights for at least one user with - *at a minimum* - rights access to the runtime directory.
- When USERCTRL is set to YES, in addition to any user that will start Transfer CFT, you must define read/write rights for files to be **transferred** for any users who will perform transfers.
- When createprocessasuser is set to YES, your must define read/write rights for the **configuration** files (e.g. communication file, partner file, and so on).

## {{< TransferCFT/suitevariablesUNIX  >}} tasks

Use the <span class="code">`chmod `</span>command to define read and write rights.

## Windows tasks

You must give each additional {{< TransferCFT/axwayvariablesComponentLongName  >}} user read and write rights as follows:

1. Right-click the <span class="bold_in_para">****{{< TransferCFT/axwayvariablesComponentShortName >}}****</span> program folder.
1. Select <span class="bold_in_para">****Properties****</span>.
1. In the <span class="italic_in_para">Properties </span>window, select the <span class="bold_in_para">****Security**** </span>tab.
1. In the <span class="italic_in_para" style="font-weight: bold;">****Security**** </span>tab, select the user and grant the user read and write rights. Click **OK**.

## z/OS tasks

See the section on [RACF PERMIT](#RACF%C2%A0pas) (TSS, etc.) in the Transfer CFT z/OS *Installation and Operations Guide.*

****Related topics****

- [About system users](../)
- [User rights use case scenarios](../user_rights_security_scenarios)
- [Recommendations and troubleshooting](../user_rights_tips)