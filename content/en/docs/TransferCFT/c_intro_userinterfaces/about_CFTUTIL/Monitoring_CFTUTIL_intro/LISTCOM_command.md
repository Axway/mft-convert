---

    title: LISTCOM - List the communication medium records
    linkTitle: LISTCOM - List COM
    weight: 330

---
This topic describes how to list the communication media records for
FILE type communication medium.

Command syntax: [LISTCOM](../../../command_summary#LISTCOM)

Use the LISTCOM command to lists the communication media
records in non-ciphered text whether these records are ciphered or not.

Only
the file type communication medium is concerned by this command.


| Parameters  | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/content">CONTENT</a>  | Results display type:<br/> • FULL: List of all records (active or otherwise)<br/> • ACTIVE (default): List of records that are not empty, meaning some messages may not display |
| <a href="">FIRST</a> | Number of the first record to be displayed or listed.<br/> The maximum number of records is the one defined in the RECNB parameter of the CFTFILE command at the time the file is created. |
| FILE {see the comment | filename}] | Communication medium filename.<br/> • The file type is implicit.<br/> • The default value for this parameter is fixed.<br/> To designate the current communication file for the Transfer CFT, set this parameter to the value of the NAME parameter in the CFTCOM TYPE=FILE command. |
| <a href="">LAST</a>  | Number of the last record to be displayed or listed.<br/> The default value is the maximum number of records defined in the RECNB parameter of the CFTFILE command at the time the file is created.<br/> The maximum number of records is the one defined in the RECNB parameter of the CFTFILE command at the time the file is created. |
| <a href="../../../command_summary/parameter_intro/verify">VERIFY</a> | Request to verify the validity of each record in the file at the time it is listed or displayed. |

