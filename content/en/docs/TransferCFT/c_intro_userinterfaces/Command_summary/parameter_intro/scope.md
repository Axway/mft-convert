---
title: "scope"
linkTitle: "scope"
weight: 3110
---<span id="scope"></span>

### scope

#### KEEP, HALT, DELETE, END, START, RESUME

****[SCOPE = { <u>PARENT</u> &#124; CHILDREN &#124; ALL } ]****

- PARENT: Applies the command to the selected transfers.
- CHILDREN: Applies the command to the children of the selected transfers if the selected transfers are generic. Otherwise:
    -   If the selected transfers are not generic, then the command is performed on the selected transfers themselves
    -   If the selected transfers are generic parents, the command is NOT performed on these transfers
- ALL: Applies to the selected transfers and all of their children.

#### LISTUCONF, UCONFSET, UCONFGET

****[ SCOPE= {
USER &#124; DEFAULT &#124; ALL &#124; \* } ]****

Filters according to the UCONF setting. For example, use USER to filter for values set by the user.

- USER: displays values that have been user-modified
- DEFAULT: displays only default values from the UCONF dictionary
- ALL or \*: includes both user and default values

 

 

[Return to Command index](../../)
