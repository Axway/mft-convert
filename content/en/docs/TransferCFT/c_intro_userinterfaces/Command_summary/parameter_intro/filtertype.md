---

    title: filtertype
    linkTitle: filtertype
    weight: 1170

---
****\[ filtertype = { <u>STRJCMP</u> | EREGEX } \]****

Defines the type of filter to be applied when sending a generic transfer.

- STRJCMP: internally filter matches with \* and ?
- EREGEX: applies a regular expression filter

To replicate the filter behavior of previous Transfer CFT versions, select STRJCMP (default) as the filtertype and leave the [filter](../filter) parameter empty.

See also <span class="bold_in_para">****Sending a group of files &gt;****</span> [Create filters using CFTSEND](../../../../concepts/using_the_send_command/send_group_of_files_cl#Create).

[Return to Command index](../../)
