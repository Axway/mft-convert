{
    "title": "Broadcast lists - CFTDEST ",
    "linkTitle": "CFTDEST - Broadcast lists",
    "weight": "320"
}This topic describes how to manages the list of partners for distribute/collect
operations using the CFTDEST object.

****Related
topics****

- Command syntax
    [CFTDEST](../../../command_summary#CFTDEST)
- Object concepts
    [Defining
    broadcasting lists]()


| Parameter  | Description  |
| --- | --- |
| [EXEC](../../../command_summary/parameter_intro/exec#exec_CFTDEST) | End of transfer procedure submit mode type.<br/> When all transfers are terminated, an end of transfer procedure is submitted. The symbolic variables are submitted in line with the generic record. For example, the &amp;PART variable is substituted with the CFTDEST command identifier. |
| [FNAME](../../../command_summary/parameter_intro/fname#fname_CFTDEST)  | Name of the file in which the list of the identifiers of the various partners, each corresponding to one value of the ID parameter of a CFTPART command, is defined. |
| [FOR](../../../command_summary/parameter_intro/for) | Control of the use of the partner list.<br/> The value defined can be:<br/> • LOCAL: the list of partners is used locally for broadcasting (send transfers) or collecting (receive transfers) the file (or message) concerned<br/> • COMMUT: the local Transfer CFT monitor performs the broadcasting as an intermediate site<br/> • BOTH: includes both the LOCAL and COMMUT facilities |
| [ID](../../../command_summary/parameter_intro/id)  | Identifier of the list of partners.<br/> This identifier must be unique. You cannot have several CFTDEST with the same ID. |
| [NOPART](../../../command_summary/parameter_intro/nopart) | Do not complete this field if you completed the fname field.<br/> Select the action to perform when one of the partners is not defined. Abort is the default value. |
| [PART](../../../command_summary/parameter_intro/part)  | Explicit list of the identifiers of the various partners (each corresponding to one value of the ID parameter of a CFTPART command).<br/> If no partner is defined (CFTPART) no transfer (broadcasting or collection) can be performed. |

