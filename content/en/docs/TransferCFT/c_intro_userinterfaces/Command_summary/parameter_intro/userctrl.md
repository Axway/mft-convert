---

    title: userctrl
    linkTitle: userctrl
    weight: 3680

---
<span id="userctrl"></span>

### userctrl

#### CFTPARM

****\[USERCTRL = { <span style="text-decoration: underline;">NO</span>
| YES }\]****

Use this field to define the file access control. You can use the UserCtrl parameter to define access to files under another credential (UserId parameter) other than the user who started Transfer CFT.

- YES:
    {{< TransferCFT/axwayvariablesComponentShortName >}} checks the user access rights on the file to be transferred.
- NO: No check is performed.

> **Note**
>
> Caution  
> Windows only - You cannot use a UNC or mapped drive (as opposed to files on a local disk) when USERCTRL=YES. Note though that even when USERCTRL=NO, you cannot access files on a local or remote disk under another account.

- For details in UNIX, please see <a href="#Manually" class="MCXref xref">Enable the file user rights (USERCTRL)</a>
- For details in Windows, please see <a href="../../../../cft_intro_install/windows_install_start_here/windows_install_start_here/running_cft_for_the_first_time_windows/add_system_user_windows#Enable3" class="MCXref xref">Enable the file user rights (USERCTRL)</a>

 

[Return to Command index](../../)
