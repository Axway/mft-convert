---

    title: Listing  partner details
    linkTitle: LISTPART - List partner details
    weight: 490

---
This topic describes how to use the LISTPART command to view partner
characteristics.

Use this command to view the characteristics of partners
(general or network characteristics), according to the selection criteria
indicated in the command.

In the absence of a previous CONFIG TYPE = OUTPUT command,
the execution report is written on the standard output of the CFTUTIL
program.


| ****Parameters**** | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/id">ID</a>  | Partner or partner list identifier.<br/> Used to select a single partner or a set of partners, using the special wildcard character "*****".<br/> Example:<br/> ID = PART1: for the partner PART1 only<br /> ID = IB*: for all the partners whose identifier begins with "IB"<br /> ID = *: for all the partners |
| <a href="../../../command_summary/parameter_intro/type">TYPE</a> | Defines the type of characteristics to be listed.<br/> TYPE can take the predefined values indicated in the Type table. |


****Example 1****

To view all the contents of the records of the PARTNER
file:

```
LISTPART TYPE = ALL
```

****Example 2****

To view broadcasting lists:

```
LISTPART TYPE = DEST
```
