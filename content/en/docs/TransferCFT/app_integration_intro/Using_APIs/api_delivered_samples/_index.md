{
    "title": "Delivered API templates",
    "linkTitle": "Delivered API templates",
    "weight": "330"
}This topic describes the {{< TransferCFT/axwayvariablesCompanyName  >}} {{< TransferCFT/axwayvariablesComponentShortName  >}} API templates, or samples, that are delivered with your {{< TransferCFT/axwayvariablesComponentShortName  >}} product. You can use the delivered samples as a template for integrating APIs, as a model, or create your own.

The templates titles are listed in the following tables according to OS and language. In the **Template** column, you can click the template link to view the sample template as a text file.

- All platform C language templates
- Specific z/OS templates
- OpenVMS templates
- IBM i templates

All platform C language templates
---------------------------------

The following C language templates are delivered on UNIX, Windows, OpenVMS, IBM i (OS/400), with the exceptions as noted in the table below.


| Template  | Function  | Services | Description  |
| --- | --- | --- | --- |
| api2xmp1  | ipcai2_*  |  • ipcai2_initialize-ipcai2_catalog_open-ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set-ipcai2_catalog_selection_sortby<br/> • ipcai2_monitor_info_get<br/> • ipcai2_catalog_selection_next-ipcai2_catalog_info_get<br/> • ipcai2_catalog_selection_delete-ipcai2_catalog_close<br/> • ipcai2_finalize | {{< TransferCFT/axwayvariablesComponentShortName  >}} Catalog API sample program, which lists all catalog content.<br/> ** Not delivered on z/OS (iseries). |
| api2xmp2  | ipcai2_*  |  • ipcai2_initialize-ipcai2_catalog_open<br/> • ipcai2_catalog_selection_new<br/> • ipcai2_catalog_selection_set<br/> • ipcai2_catalog_selection_sortby<br/> • ipcai2_monitor_info_get<br/> • ipcai2_catalog_selection_next-<br/> • ipcai2_catalog_info_get<br/> • ipcai2_catalog_selection_delete<br/> • ipcai2_catalog_close<br/> • ipcai2_finalize | {{< TransferCFT/axwayvariablesComponentShortName  >}} Catalog API sample program, which changes all Terminated transfers to Ended.<br/> ** Not delivered on z/OS (iseries). |
| tcftsyn  | cftau  | COM, SEND, GETXINFO, SWAITCAT, CLOSEAPI  | {{< TransferCFT/axwayvariablesComponentShortName  >}} Communication and Catalog API sample program using a synchronous communication media.<br/> ** Not delivered on OS/400 or z/OS (iseries). |


### File location of the templates

##### UX/Win

The templates are located in the `<Transfer_CFT>\home\distrib\template` directory. For example in Windows, `C:\Axway\Transfer_CFT\home\distrib\template`.

##### IBM i (OS/400)

The templates located in the production library (CFTPROD by default), in the CFTSRC folder.
