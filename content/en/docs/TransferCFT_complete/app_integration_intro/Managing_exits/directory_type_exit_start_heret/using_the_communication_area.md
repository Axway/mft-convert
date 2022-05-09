{
    "title": "Using  server mode",
    "linkTitle": "Using server mode",
    "weight": "390"
}This topic describes how to use the communication area for a directory
exit in server mode. The [next topic](../using_requester_mode)
explains a directory exit in the requester mode.

<span id="Server_Mode"></span>

Server mode
-----------

The entire communication structure is defined by the interface before
the user function is called. You must perform checks concerning the characteristics
of the calling partner.

### Partner information fields


| Field  | Explanation  |
| --- | --- |
| ptype  | Partner type  |
| nrpart  | Remote partner name  |
| nrpassw  | Remote partner password  |
| newnrpw  | Remote partner new password  |
| nspart  | Local name  |
| nspassw  | Local password  |
| prot  | Protocol type  |
| prof  | Profile if PESIT protocol  |
| sap  | Remote Sap (Service Access Point)  |
| imaxtime  | Maximum incoming call time (HHMMSSCC) |
| imintime  | Minimum incoming call time (HHMMSSCC) |
| ntype  | Network type  |
| addr  | Remote partner address  |
| udata  | User data  |
| pcvin  | Incoming reverse charge call |
| gfa  | Closed subscriber group number  |


When the user function is called, if the partner name ****part****
is empty, so that the partner local identifier is unknown to Transfer
CFT, the only fields that contain valid data (imintime and imintime are
initialized to "23595999" and "00000000" respectively)
are the above fields in addition to the general information fields.  
The ****ret1**** return code field must
be defined when the user function is returned.  
If there is a connection refusal, return code value of 2, the ****ret2****
field may be defined to inform the calling partner of the cause of the
refusal.  
The content of the diag field appears with the appropriate error message
if the return code is not 0 and 1.  
If the msg field is defined, its content is sent to the {{< TransferCFT/axwayvariablesComponentShortName  >}} standard
output.

If the return code value is 0 or 1, the user can modify the fields indicated
in the following System information field table.

### System information fields


| Field  | Explanation  |
| --- | --- |
| syst  | System  |
| code  | Code:<br/> • A: ASCII<br/> • E: EBCDIC |
| open  | Obsolete parameter |
| commut  | Store and forward indicator  |
| nspart  | Local name  |
| nspassw  | Local password  |
| part  | Partner local identifier  |
| group  | Group identifier  |
| sauth  | File send authorization list identifier  |
| rauth  | File receive authorization list identifier  |
| xlate  | Transcoding table identifier  |
| idf  | Partner file identifier  |


If the part field of the communication
structure is empty on return from the user function, the partner local
identifier UNDEFPTN appears in
the catalog and on the {{< TransferCFT/axwayvariablesComponentShortName  >}} standard output.  
If the part field has been modified
and if the new identifier is located in the {{< TransferCFT/axwayvariablesComponentShortName  >}} partner base,
{{< TransferCFT/axwayvariablesComponentShortName  >}} sets the ret1 field to 9 (processing error) and the diag
field to "PTNEXIST".
