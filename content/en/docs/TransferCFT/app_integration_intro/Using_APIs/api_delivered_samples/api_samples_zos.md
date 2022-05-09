{
    "title": "Delivered templates for z/OS",
    "linkTitle": "Delivered templates for z/OS",
    "weight": "360"
}This topic lists the {{< TransferCFT/axwayvariablesCompanyName  >}} {{< TransferCFT/axwayvariablesComponentShortName  >}} API templates that are delivered for the z/OS platform. You may decide to use the delivered samples as a basis for integrating APIs, or as a model to create your own. Templates include samples in:

- [Assembler language](#Assembl)
- [C language](#C)
- [COBOL language](#COBOL)

The {{< TransferCFT/axwayvariablesComponentShortName  >}} z/OS templates are located in the installed {{< TransferCFT/axwayvariablesComponentShortName  >}} distribution library.

<span id="Assembl"></span>

Assembler language
------------------


| Template  | Function  | Services |
| --- | --- | --- |
| AAPIDLL | cftai - cftac - cftinit  | COM - SEND - OPEN - CLOSE - (SELECT - NEXT) or (SELECT240 - NEXT240)  |
| AXPIDLL | cftaix - cftac - cftinit  | COM - SEND - OPEN - CLOSE - DO - SELECT (NEXT or NEXT240)  |


<span id="C"></span>

C language
----------


| Template  | Function  | Services | Description  |
| --- | --- | --- | --- |
| CAPI2A | ipcai2_*  |  • ipcai2_initialize-ipcai2_catalog_open-ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set<br/> • ipcai2_catalog_selection_sortby<br/> • ipcai2_catalog_selection_skip<br/> • ipcai2_catalog_selection_next<br/> • ipcai2_catalog_record_get<br/> • ipcai2_catalog_info_get<br/> • ipcai2_catalog_selection_delete<br/> • ipcai2_catalog_close-ipcai2_finalize-<br/> • ipcai2_get_errno_str | {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog API sample program, listing all catalog content.  |
| CAPI2B | ipcai2_*  |  • ipcai2_initialize<br/> • ipcai2_catalog_open<br/> • ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set<br/> • ipcai2_catalog_selection_sortby<br/> • ipcai2_catalog_selection_skip<br/> • ipcai2_catalog_selection_next<br/> • ipcai2_catalog_record_get<br/> • ipcai2_catalog_info_get<br/> • ipcai2_catalog_selection_delete<br/> • ipcai2_catalog_close<br/> • ipcai2_finalize<br/> • ipcai2_get_errno_str | {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog API sample program, which changes all Terminated transfers to Ended.  |
| CAPIC | cftai - cftac - cftau  | OPEN- SELECT - NEXT - MODIFY - CLOSE - SEND -RECV - HALT - START - DELETE  | C Sample for {{< TransferCFT/axwayvariablesComponentShortName  >}} API.  |
| CAPISYN | cftau  | COM - SEND - GETXINFO - SWAITCAT  | {{< TransferCFT/axwayvariablesComponentShortName  >}} communication sample program using Synchronous Communication media (multiple send commands are possible).  |
| CAPIW | cftai - cftau  | SEND - OPEN - CLOSE - (SELECT - NEXT) or (SELECT240 - NEXT240)  | Perform a SEND request with an IDA and wait for the transfer to end (completed) until the timer expires or the transfer is aborted.  |
| CAPIX | cftaix  | OPEN - CLOSE - SELECT (NEXT - NEXT240) - SORT - DO - COUNT  | A C language template for a catalog list with selection and sorting.  |


<span id="COBOL"></span>

COBOL language
--------------


| Template  | Function  | Services | Description  |
| --- | --- | --- | --- |
| OAPI2A | ipcai2_*  |  • ipcai2_initialize<br/> • ipcai2_catalog_open<br/> • ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set<br/> • ipcai2_catalog_selection_sortby<br/> • ipcai2_catalog_selection_skip<br/> • ipcai2_catalog_selection_next<br/> • ipcai2_catalog_record_get<br/> • ipcai2_catalog_info_get<br/> • ipcai2_catalog_selection_delete<br/> • ipcai2_catalog_close<br/> • ipcai2_finalize<br/> • ipcai2_get_errno_str | {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog API template program, which lists all catalog content.  |
| OAPI2AS | ipcai2_*  |  • ipcai2_initialize<br/> • ipcai2_catalog_open<br/> • ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set<br/> • ipcai2_catalog_selection_sortby<br/> • ipcai2_catalog_selection_skip<br/> • ipcai2_catalog_selection_next<br/> • ipcai2_catalog_record_get<br/> • ipcai2_catalog_info_get<br/> • ipcai2_catalog_selection_delete<br/> • ipcai2_catalog_close<br/> • ipcai2_finalize<br/> • ipcai2_get_errno_str | {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog API template program, which lists all of the catalog content.  |
| OAPI2B | ipcai2_*  |  • ipcai2_initialize-<br/> • ipcai2_catalog_open<br/> • ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set<br/> • ipcai2_catalog_selection_sortby<br/> • ipcai2_catalog_selection_skip<br/> • ipcai2_catalog_selection_next<br/> • ipcai2_catalog_record_get<br/> • ipcai2_catalog_info_get<br/> • ipcai2_catalog_selection_delete<br/> • ipcai2_catalog_close<br/> • ipcai2_finalize<br/> • ipcai2_get_errno_str | {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog API template program, which changes all successful transfers to a completed state.  |
| OAPI2BS | ipcai2_*  |  • ipcai2_initialize<br/> • ipcai2_catalog_open<br/> • ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set<br/> • ipcai2_catalog_selection_sortby<br/> • ipcai2_catalog_selection_skip<br/> • ipcai2_catalog_selection_next<br/> • ipcai2_catalog_record_get-<br/> • ipcai2_catalog_info_get-<br/> • ipcai2_catalog_selection_delete-<br/> • ipcai2_catalog_close-<br/> • ipcai2_finalize-<br/> • ipcai2_get_errno_str | {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog API template program, which changes all successful transfers to the completed state.  |
| OAPIC | CFTC  | SEND -RECV - HALT - START - DELETE  | Issue {{< TransferCFT/axwayvariablesComponentShortName  >}} commands. |
| OAPICS | CFTC  | SEND -RECV - HALT - START - DELETE  | Issue {{< TransferCFT/axwayvariablesComponentShortName  >}} commands  |
| OAPII | CFTI  | OPEN - CLOSE - (SELECT - NEXT) or (SELECT240 - NEXT240)  | List the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog. |
| OAPIIS | CFTI  | OPEN - CLOSE - (SELECT - NEXT) or (SELECT240 - NEXT240)  | List the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog.  |
| OAPISYN | cftau  | COM - SEND - GETXINFO - SWAITCAT  | {{< TransferCFT/axwayvariablesComponentShortName  >}} communication sample program using the synchronous communication media, where multiple SEND commands are possible.  |
| OAPIW | CFTI - CFTU  | SEND, OPEN, CLOSE , (SELECT - NEXT), or (SELECT240 - NEXT240)  | Perform a SEND request with an IDA and wait for the transfer to complete successfully, reach the time out, or abort the transfer.  |
| OAPIWS | CFTI - CFTU  | SEND - OPEN - CLOSE - (SELECT - NEXT) or (SELECT240 - NEXT240)  | Perform a SEND request with an IDA and wait for the transfer to complete successfully, reach the time out, or abort the transfer.  |


 


| Template  | Function  | Services |
| --- | --- | --- |
| OAPI2A | ipcai2_*  |  • ipcai2_initialize<br/> • ipcai2_catalog_open<br/> • ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set<br/> • ipcai2_catalog_selection_sortby<br/> • ipcai2_catalog_selection_skip<br/> • ipcai2_catalog_selection_next<br/> • ipcai2_catalog_record_get<br/> • ipcai2_catalog_info_get<br/> • ipcai2_catalog_selection_delete<br/> • ipcai2_catalog_close<br/> • ipcai2_finalize<br/> • ipcai2_get_errno_str |
| OAPI2AS | ipcai2_*  |  • ipcai2_initialize<br/> • ipcai2_catalog_open<br/> • ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set<br/> • ipcai2_catalog_selection_sortby<br/> • ipcai2_catalog_selection_skip<br/> • ipcai2_catalog_selection_next<br/> • ipcai2_catalog_record_get<br/> • ipcai2_catalog_info_get<br/> • ipcai2_catalog_selection_delete<br/> • ipcai2_catalog_close<br/> • ipcai2_finalize<br/> • ipcai2_get_errno_str |
| OAPI2B | ipcai2_*  |  • ipcai2_initialize-<br/> • ipcai2_catalog_open<br/> • ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set<br/> • ipcai2_catalog_selection_sortby<br/> • ipcai2_catalog_selection_skip<br/> • ipcai2_catalog_selection_next<br/> • ipcai2_catalog_record_get<br/> • ipcai2_catalog_info_get<br/> • ipcai2_catalog_selection_delete<br/> • ipcai2_catalog_close<br/> • ipcai2_finalize<br/> • ipcai2_get_errno_str |
| OAPI2BS | ipcai2_*  |  • ipcai2_initialize<br/> • ipcai2_catalog_open<br/> • ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set<br/> • ipcai2_catalog_selection_sortby<br/> • ipcai2_catalog_selection_skip<br/> • ipcai2_catalog_selection_next<br/> • ipcai2_catalog_record_get-<br/> • ipcai2_catalog_info_get-<br/> • ipcai2_catalog_selection_delete-<br/> • ipcai2_catalog_close-<br/> • ipcai2_finalize-<br/> • ipcai2_get_errno_str |
| OAPIC | CFTC  | SEND -RECV - HALT - START - DELETE  |
| OAPICS | CFTC  | SEND -RECV - HALT - START - DELETE  |
| OAPII | CFTI  | OPEN - CLOSE - (SELECT - NEXT) or (SELECT240 - NEXT240)  |
| OAPIIS | CFTI  | OPEN - CLOSE - (SELECT - NEXT) or (SELECT240 - NEXT240)  |
| OAPISYN | cftau  | COM - SEND - GETXINFO - SWAITCAT  |
| OAPIW | CFTI - CFTU  | SEND, OPEN, CLOSE , (SELECT - NEXT), or (SELECT240 - NEXT240)  |
| OAPIWS | CFTI - CFTU  | SEND - OPEN - CLOSE - (SELECT - NEXT) or (SELECT240 - NEXT240)  |

