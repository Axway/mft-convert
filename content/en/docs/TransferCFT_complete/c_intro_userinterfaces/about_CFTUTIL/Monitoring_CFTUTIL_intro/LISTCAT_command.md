{
    "title": "LISTCAT - List catalog  contents",
    "linkTitle": "LISTCAT - List catalog contents",
    "weight": "290"
}This page describes how to list the catalog contents using command
line operations. The LISTCAT command is used to query the information
associated with the selected transfers, recorded in the Transfer CFT catalog.

Depending on the value of the CONTENT parameter, the information is
displayed in the form of:

- Lists in which
    only the most important features of transfers appear CONTENT = BRIEF default
    value
- Pages in which
    all the features of transfers appear CONTENT = FULL

See also, [LISTCAT - Brief/Full catalog listings](../brief_catalog_listing).

In the absence of a previous command CONFIG TYPE = OUTPUT, this information
or the execution report for a syntax error, no associated information,
and so on, is written on the standard output of the task.

This command is used to display output for:

- Static transfer
    information associated with the parameters of the SEND and RECV commands
- Dynamic transfer
    information such as the:

<!-- -->

- Transfer status
- Number of items
    transferred
- Compression
    factor, etc.

The command parameters are selection criteria. See also, LISTCAT/DISPLAY - Statistical variables.

Parameter descriptions
----------------------

Command syntax: LISTCAT

For the definition of the states of a transfer,
refer to [Transfer control commands](../../../../concepts/transfer_command_overview/transfer_control_commands).


| Command or Parameter  | Description |
| --- | --- |
| LISTCAT command | Use this command to query the information associated with the selected transfers, recorded in the Transfer CFT catalog.  |
|  [CONTENT](../../../command_summary/parameter_intro/content)  | Used to obtain part or all of the information of a catalog entry. |
|  [DATETIMEMAX](../../../command_summary/parameter_intro/datetimemax)  | Use to display catalog transfers that happened on or before this end date and time according to the transfer record creation (DATEK, TIMEK).  |
|  [DATETIMEMIN](../../../command_summary/parameter_intro/datetimemin)  | Use to display catalog transfers that happened on or after this start date and time according to the transfer record creation (DATEK, TIMEK).  |
|  [DIAGI](../../../command_summary/parameter_intro/diagi)  | Define the diagi catalog transfer field display.  |
|  [DIRECT](../../../command_summary/parameter_intro/direct)  | Transfer direction.<br/> The possible values are:<br/> • BOTH *or* *<br/> • SEND<br/> • RECV |
|  FILE <br /> see the comment | Complete name or logical name of the catalog file.<br/> The default value for this parameter is fixed. <br/> To designate the current catalog of the monitor, this parameter should be defined with the filename set in the FNAME parameter of the CFTCAT command. |
|  [IDA](../../../command_summary/parameter_intro/ida#ida)  | Local identifier of the transfer assigned by the user or the user application.<br/> Several catalog entries may be associated with a given IDA. |
|  [IDF](../../../command_summary/parameter_intro/idf#idf_CFTCAT)  | Model file identifier.<br/> Several catalog entries may be associated with a given IDF. |
|  [IDT](../../../command_summary/parameter_intro/idu#idt)  | Transfer identifier.<br/> Identifies a transfer for a given partner and transfer direction. |
|  [IDTU](../../../command_summary/parameter_intro/idtu)  | Unique local transfer reference identifier. |
|  [NPART](../../../command_summary/parameter_intro/npart)  | Network identifier of the partner(s) for the selected transfers.<br/> The information displayed for LISTCAT CONTENT = BRIEF is different, according to whether the NPART parameter is defined or not (see the paragraphs below). |
|  [PART](../../../command_summary/parameter_intro/part)  | Partner identifier for the selected transfers.<br/> The value of this parameter may be:<br/> • An identifier<br/> • A mask<br/> • Omitted*<br/> If the NPART parameter is defined, the PART parameter is ignored. |
|  [SORTBY](../../../command_summary/parameter_intro/sortby)  | Sorts the LISTCAT command information in an alphabetical/alphanumberic order.  |
|  [STATE](../../../command_summary/parameter_intro/state) | Possible states of a catalog entry.<br/> The catalog entries in the state indicated by this parameter are selected. Any combination of the various states (D,C,H,K,T,X) is authorized. |
|  [TYPE](../../../command_summary/parameter_intro/type)  | Type of catalog entry.<br/> If TYPE = * *or* ALL, no selection is made: all transfers present in the catalog (files, messages, reply messages) are displayed if they fulfill the selection criteria which may be defined by other parameters. |


### Examples

****Example 1****

```
LISTCAT TYPE = ALL, PART = HQ, STATE = DC
```

Displays the most important information (CONTENT=BRIEF by default) concerning
all transfers (TYPE=ALL) sent to or received from (DIRECT=BOTH by default)
the partner (PART = HQ), the states of which are "Available"
or "in Process" (STATE=DC).

****Example 2****

```
LISTCAT TYPE = FILE, DIRECT = SEND, PART = PARIS5
```

Displays the most important information (CONTENT=BRIEF by default) concerning
the file transfers (TYPE=FILE) sent (DIRECT=SEND) to the partner (PART)
PARIS5, all states included (STATE=\* by default).

****<span id="sortby_example"></span>Example 3****

```
LISTCAT SORTBY=IDTU
```

Displays the records by IDTU. This can be useful because the catalog's compact behavior takes advantage of unused records meaning the IDTU may not display in order.

LISTCAT CONTENT = COMMUT
------------------------

`LISTCAT CONTENT = COMMUT`


| Heading  | Meaning  |
| --- | --- |
| 1  | Intermediate transfer (IPART)  |
| 2  | State transfer<br /> The DTSA characters represent:<br/> • Direction = S/R (Send/Receive)<br/> • Type = F/M/R (File/Message/Reply)<br/> • State = D/C/H/K/T/X (Disp/Current/Hold/Keep/Terminated/eXecuted)<br/> • Ack = A (Acknowledge)<br/> <blockquote> **Note**<br/> Note: If the UCONF compatibility option is set to the default value (no), the format is DTSAPP to include Phase and PhaseStep. For more information, see Backward compatibility.<br/> </blockquote>  |
| 3  | File network identifier (NFNAME)  |
| 4  | Transfer protocol identifier (NIDT)  |
| 5  | Local transfer identifier (IDTU)  |
| 6  | Transfer CFT internal diagnostic code (DIAGI)  |


LISTCAT CONTENT = EXTEND
------------------------

`LISTCAT CONTENT = EXTEND `


| Heading  | Meaning  |
| --- | --- |
| 1  | Local partner identifier described in the CFTPART (ID) or CFTDEST command (one of the PARTS of the broadcasting list)  |
| 2  | Transfer state<br /> The DTSA characters mean:<br/> • Direction = S/R (Send/Receive)<br/> • Type = F/M/R (File/Message/Reply)<br/> • State = D/C/H/K/T/X (Disp/Current/Hold/Keep/Terminated/eXecuted)<br/> • Ack = A (Acknowledge)<br/> <blockquote> **Note**<br/> Note: If the UCONF compatibility option is set to the default value (no), the format is DTSAPP to include Phase and PhaseStep. For more information, see Backward compatibility.<br/> </blockquote>  |
| 3  | File identifier (IDF)  |
| 4  | Transfer identifier (IDT)  |
| 5  | Identifier of the application associated to the transfer (IDA)  |
| 6  | Requester (&quot;R&quot;) or server (&quot;S&quot;) mode  |
| 7  | Client security mode (&quot;C&quot;) or server (&quot;S&quot;) mode  |
| 8  | Exits used<br /> The characters FAT represent:<br/> • F: file exit (&quot;x&quot; if present)<br/> • A: directory exit (&quot;x&quot; if present)<br/> • T: end of transfer exit (&quot;x&quot; if present) |
| 9  | Use of an end of transfer procedure (&quot;x&quot; if present)  |
| 10  | Transfer CFT internal diagnostic (DIAGI)  |

