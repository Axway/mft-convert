{
    "title": "LISTCOM - List the communication medium records",
    "linkTitle": "LISTCOM - List communication media",
    "weight": "310"
}This topic describes how to list the communication media records for
FILE type communication medium. Use the LISTCOM command to list the communication media
records in non-ciphered text whether these records are ciphered or not.

Command syntax: [LISTCOM](../../../command_summary#LISTCOM)

This command only applies to
the TYPE=FILE communication medium.


| Parameters  | Description  |
| --- | --- |
| [CONTENT](../../../command_summary/parameter_intro/content)  | Results display type:<br/> • FULL: List of all records (active or otherwise)<br/> • ACTIVE (default): List of records that are not empty, meaning some messages may not display<br/> • STAT: Displays the header and footer but not the command records |
| [FIRST]() | Number of the first record to be displayed or listed.<br/> The maximum number of records is the one defined in the RECNB parameter of the CFTFILE command at the time the file is created. |
| FILE {see the comment &#124; filename}] | Communication medium filename.<br/> • The file type is implicit.<br/> • The default value for this parameter is fixed.<br/> To designate the current communication file for the Transfer CFT, set this parameter to the value of the NAME parameter in the CFTCOM TYPE=FILE command. |
| [LAST]()  | Number of the last record to be displayed or listed.<br/> • The default value is the maximum number of records defined in the RECNB parameter of the CFTFILE command at the time the file is created.<br/> • The maximum number of records is the one defined in the RECNB parameter of the CFTFILE command at the time the file is created. |
| [VERIFY](../../../command_summary/parameter_intro/verify) | Request to verify the validity of each record in the file at the time it is listed or displayed. |

