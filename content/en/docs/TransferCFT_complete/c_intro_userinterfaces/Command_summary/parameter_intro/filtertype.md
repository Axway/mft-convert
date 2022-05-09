{
    "title": "filtertype",
    "linkTitle": "filtertype",
    "weight": "1160"
}****[ filtertype = { <span class="underline">STRJCMP</span> &#124; EREGEX } ]****

Defines the type of filter to be applied when sending a generic transfer.

- STRJCMP: internally filter matches with \* and ?
- EREGEX: applies a regular expression filter

To replicate the filter behavior of previous Transfer CFT versions, select STRJCMP (default) as the filtertype and leave the [filter](../filter) parameter empty.

See also ****Sending a group of files &gt;**** [Create filters using CFTSEND](../../../../concepts/send_command/send_group_of_files_cl#Create).

[Return to Command index](../../)
