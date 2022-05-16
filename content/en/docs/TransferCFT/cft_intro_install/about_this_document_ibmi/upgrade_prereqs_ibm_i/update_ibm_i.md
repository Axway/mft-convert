---

    title: Apply a service pack or patch
    linkTitle: Update with a service pack or patch
    weight: 210

---
This section describes how to apply a patch or Service Pack to Transfer CFT in an IBM i environment.

## Display information

Use the <span class="code">`CFTUTIL ABOUT`</span> command to display the product information, including the service pack number and patch number.

******Results******

```
CFT information :
\* product = CFT/OS400
\* version = 3.2.4
\* level = SP0_P1
\* upgrade = 8668000
\* target = os400
```

## Update with a service pack

To apply a service pack:

1. Log in as the **CFT** user (user with rights to manage and use Transfer CFT).
1. Stop Transfer CFT.
1. Log in as the **CFTINST** user (user with \*ALLOBJ rights).
1. Create a SAVF on your IBM i system.
1. Upload the Transfer\_CFT\_3.2.x\_SPx\_os400.bin (in binary mode) to the SAVF you created in Step 2.
1. Restore the SAVF to a temporary library, add it to your library list, and then launch the <span class="code">`UPDATE `</span>command.
1. Complete the required fields:

- In the first field, enter your program library.

- In the second field, enter your production library.

- In the third field, enter the name of the SAVF where the backup of your current version is stored.

- **The SAVF must be in your production library. If there is no SAVF, it is created.**

- Press Enter to continue.

    Use the <span class="code">`CFTUTIL ABOUT`</span> command to check the service pack level.

    Results, for example, after applying SP4:

> **Note**
>
>  

When you install a service pack, the contents of the home directory are updated, but the runtime directory remains untouched. This is so that your customizations, such as APIs, are not overwritten.

## Apply a patch

1. Stop Transfer CFT.
1. Create a SAVF on your IBM i system.
1. Upload the Transfer\_CFT-SPx\_Patchz\_os400.bin (in binary mode) to the SAVF you created in Step 2.
1. Restore the SAVF to a temporary library, add it to the top of your library list, and then launch the <span class="code">`PATCHER `</span>command.
1. Complete the required fields:

- In the first field, enter your CFT program library.
- In the second field, enter the name of the SAVF where the backup of current

Module versions impacted by the patch are stored. If the SAVF does not exist, it is created; if it exists, it is cleared.

```
Install Transfer CFT patch (PATCHER)
Type choices, press Enter.
 
Program library . . . . . . . . CFTPGM Character value
SAVF for backup . . . . . . . . PATCHSAV Character value
```

**Results**

Use the <span class="code">`CFTUTIL ABOUT`</span> command to check the patch level.

Results, for example, after applying SP2\_Patch2:

```
CFT information :
\* product = CFT/OS400
\* version = 3.2.4
\* level = SP2_Patch2
\* upgrade = 8712000
\* target = os400
```
