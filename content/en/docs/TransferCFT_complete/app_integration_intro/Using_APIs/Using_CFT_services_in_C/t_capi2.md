{
    "title": "About cftapi2",
    "linkTitle": "About cftapi2",
    "weight": "330"
}The catalog functions enable you to query and modify the catalog. These functions also include a method to recover information about the {{< TransferCFT/axwayvariablesComponentShortName  >}} that is using the catalog.

Additionally, the API catalog supports 32 character identifiers and 512 character file names.

Return code values
------------------

The return code values are available in the `cftapi2.h` header file, located in the `Transfer_CFT/home/inc` directory (for UNIX/Windows), in the section *Error code fields*.

Data structure
--------------

The data structures that are used by the API are as follows:

- Current session in progress: CftApi2Session
- Catalog: CftApi2Catalog
- Selection: CftApi2Selection
- Saved catalog record: CftApi2Record

The programmer can set pointers to these data structures. These are then allocated and initialized by the API.

API functions
-------------


| Service | CftApi2Session *ipcai2_initialize () |
| --- | --- |
| Definition | Initializes the API. |
| Parameter | None. |
| Return value | This function returns a pointer to the CFTApi2Session. If the returned value is NULL, the session cannot be initialized. |


 


| Service | long ipcai2_get_errno(CftApi2Session * session) |
| --- | --- |
| Definition | Recuperates the latest error code. |
| Parameter | session: Pointer to the CftApi2Session is returned by the initialization function ipcai2_initialize() |
| Return value | The last error code for the API for this session. |
| Remarks | This function can be used after all calls to the API except for ipcai2_initialize() and ipcai2_finalize(). |


 


| Service | long ipcai2_get_errno_str(CftApi2Session * session, char *buffer, int bufflen) |
| --- | --- |
| Definition | Recuperates the error message. |
| Parameter | session: Pointer to the CftApi2Session structure is returned by the initialization function ipcai2_initialize()<br /> buffer: The buffer that will be informed of the error message.<br /> bufflen: Length of the buffer sent to the API. |
| Return value | If the return code is positive, it contains the last error code for the API for this session.<br/> If the Return code is negative, the buffer is too short. If this happens, and the code is equal to –n where n is equal to the required length. |
| Remarks | This function can be used after all calls to the API except for ipcai2_initialize() and ipcai2_finalize(). |


 


| Service | long ipcai2_finalize(CftApi2Session * session) |
| --- | --- |
| Definition | Closes the API. |
| Parameter | session: Pointer to the CftApi2Session structure returned by the initialization function ipcai2_initialize() |
| Return value | None. |
| Remarks | None. |


 


| Service | CftApi2Catalog *ipcai2_catalog_open(CftApi2Session * session, char *catalog_fname)  |
| --- | --- |
| Definition | Opens the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog file. |
| Parameter | session: Pointer to the CftApi2Session structure returned by the initialization ipcai2_initialize()<br /> catalog_fname: Name of the catalog file. If the file name is &quot;&quot; the API opens the catalog file by default, for example _CFTCATA for {{< TransferCFT/axwayvariablesComponentShortName  >}}UNIX. |
| Return value | This function returns a pointer to the CftApi2Catalog structure. If the returned value is NULL, the catalog cannot be opened and the error code is returned by calling ipcai2_get_errno(). |
| Remarks | None |


 


| Service | long ipcai2_catalog_reload_cache(CftApi2Catalog *catalog) |
| --- | --- |
| Definition | Reloads the catalog cache. |
| Parameter | catalog: Pointer to the catalog returned by ipcai2_catalog_open() |
| Return value | None. |
| Remarks | None. |


 


| Service | long ipcai2_catalog_close(CftApi2Catalog * catalog |
| --- | --- |
| Definition | Closes the catalog. |
| Parameter | catalog: Pointer to the catalog returned by ipcai2_catalog_open() |
| Return value | Calling ipcai2_get_errno() ou ipcai2_get_errno_str() enables you to recuperate the return code for this function. |
| Remarks | None. |


 


| Service | CftApi2Selection *ipcai2_catalog_selection_new(CftApi2Catalog *catalog) |
| --- | --- |
| Definition | New selection in the catalog. |
| Parameter | catalog: The pointer to the catalog returned by ipcai2_catalog_open() |
| Return value | This function returns a pointer to a CftApi2Selection structure. If the returned value is NULL the selection cannot be initialized and an error code is returned by calling ipcai2_get_errno(). |
| Remarks | None. |


 


| Service | long ipcai2_catalog_selection_ref(CftApi2Selection *selection) |
| --- | --- |
| Definition | References a selection. |
| Parameter | Selection: The pointer for the selected structure returned by calling ipcai2_catalog_selection_new() |
| Return value | Negative or null: Error, recuperated the error code from ipcai2_get_errno() or ipcai2_get_errno_str().<br /> Positive: Total number of sessions referenced after this call. |
| Remarks | A selection can be referenced several times, and must be referenced at least one time before it is used. |


 


| Service | long ipcai2_catalog_selection_set(CftApi2Selection * selection, char *param, char *value) |
| --- | --- |
| Definition | Initializes a parameter selection. |
| Parameter | selection: Pointer for the selected structure referenced by ipcai2_catalog_selection_ref()<br /> param: Modify selection parameter. Selection parameter available in cftapi2.h under “Selection parameters” section: CFTAPI2_SELECTION_*<br /> value: Parameter value in a character string |
| Return value | None. |
| Remarks | None. |


 


| Service | long ipcai2_catalog_selection_next(CftApi2Selection *selection) |
| --- | --- |
| Definition | Executes a selection. |
| Parameter | selection: Pointer for the selection is referenced by ipcai2_catalog_selection_ref() |
| Return value | A call to ipcai2_get_errno() ou ipcai2_get_errno_str() enables you to recover the return code for this function. |
| Remarks | None. |


 


| Service | long ipcai2_catalog_record_get(CftApi2Selection * selection, char *param, char *buffer, int bufflen) |
| --- | --- |
| Definition | Recovers a saved field from the selected catalog. |
| Parameter | selection: Pointer for the selection structure is referenced by ipcai2_catalog_selection_ref().<br /> param: Parameter to be recovered. The parameter to be recovered is available in cftapi2.h in the “Catalog record fields” section: CFTAPI2_RECORD_*<br /> buffer: Buffer that will be informed with the parameter value<br /> bufflen: Length of the buffer sent by the API |
| Return value | If the Return code is positive it contains the last API error code for this session. If the return code is negative, the buffer is too short and the code is equal to –n where n is the required length for the buffer.  |
| Remarks | None. |


 


| Service | long ipcai2_catalog_selection_unref(CftApi2Selection *selection) |
| --- | --- |
| Definition | De-lists a selection. |
| Parameter | selection: Pointer to a selection structure that references ipcai2_catalog_selection_ref() |
| Return value | Negative: Error, recovered the error code by calling ipcai2_get_errno() or ipcai2_get_errno_str()<br /> Null: Okay, and no other referred sessions.<br /> Positive: Total number of referred sessions after this call. |
| Remarks | None |


 


| Service | long ipcai2_catalog_selection_delete(CftApi2Selection *selection) |
| --- | --- |
| Definition | Deletes a selection. |
| Parameter | selection: Pointer to a selection that is not referred by calling ipcai2_catalog_selection_unref() |
| Return value | A call to ipcai2_get_errno() or ipcai2_get_errno_str() enables you to recover the Return code for this function. |
| Remarks | None. |


 


| Service | long ipcai2_transfert_change_state(CftApi2Selection * selection, char state) |
| --- | --- |
| Definition | Modifies the transfer state for the selected catalog. |
| Parameter | selection: Pointer to a selection carried out by ipcai2_catalog_selection_next() |
| Return value | None. |
| Remarks | The {{< TransferCFT/axwayvariablesComponentShortName  >}} API must have already opened the communication medium. |


 


| Service | long ipcai2_catalog_info_get(CftApi2Catalog *catalog, char *param, char *buffer, int bufflen) |
| --- | --- |
| Definition | Recovers catalog information. |
| Parameter | catalog: Pointer to the catalog structure returned by ipcai2_catalog_open()<br /> param: Recover parameter is available in cftapi2.h under the “Catalog information parameters” section heading: CFTAPI2_CAT_INFO_*<br /> buffer: Buffer that will be informed of the parameter value.<br /> bufflen: Length of the buffer sent to the API. |
| Return value | If the Return code is positive it contains the last API error code for this session.<br/> If the return code is negative, the buffer is too short. The code is equal to –n ,where the n is equal to the required length. |
| Remarks | None. |


 


| Service | long ipcai2_monitor_info_get(CftApi2Catalog *catalog, char *param, char *buffer, int bufflen) |
| --- | --- |
| Definition | Recovers information about the {{< TransferCFT/axwayvariablesComponentShortName  >}}. |
| Parameter | catalog: Pointer to the catalog returned by ipcai2_catalog_open()<br /> param: Parameter to recover. The parameter is available in cftapi2.h under “Monitor information parameters” topic : CFTAPI2_MON_INFO__*<br /> buffer: Buffer that was provided the parameter value .<br /> bufflen: Length of the buffer sent to the API. |
| Return value | If the return code is positive, it contains the last API error code for the session.<br/> If the return code is negative, the buffer is too short. In this case, the code is equal to –n where n is the required length. |
| Remarks | None. |


#### Examples of API usage

The heading file cftapi2.h and the commented example source files are delivered with the product.

****Related topics****

[About {{< TransferCFT/axwayvariablesComponentShortName  >}} services in C](../../../../cft_intro_install/about_this_document_ibmi/using_apis/using_cft_services_in_c)

[About application
programming interfaces](../../../../cft_intro_install/about_this_document_zos/using_apis)
