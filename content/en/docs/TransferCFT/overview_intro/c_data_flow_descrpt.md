{
    "title": "Types of data flows",
    "linkTitle": "Types of data flows",
    "weight": "100"
}This topic describes the various types of flows that you can create in your enterprise environment. You may use one or more of the following data flow types in your A2A or B2B use case.

Application integration
-----------------------

Application integration refers to the transfer of files between applications that produce and consume files. To achieve application to application integration, data flows may specify:

- How the source application indicates that a file is ready for transfer and how the file transfer agent picks up the file from the source application
- How the target application is notified that the file has arrived and how the file transfer agent delivers the file to the destination application
- The level of security to apply
- Additional actions to launch depending on transfer status, acknowledgement, exceptions, and so on

Multi-site integration
----------------------

{{< TransferCFT/axwayvariablesComponentShortName  >}} can be used as part of a multi-site integration where files are transferred between remote locations, such as stores, factories, sites, and data centers of a single enterprise, as well as between partners.

In multi-site integration file transfer, the sites and technology are owned by the same enterprise. Multi-site integration allows for better integration with the systems at the site for increased control and visibility. However, multi-site and business-to-business file transfer patterns must both address security concerns and network usage and bandwidth issues.

Portal-based file transfer
--------------------------

For portal-based file transfers, an end user may be using a company portal to upload or download files within that portal. The portal is typically not provided by {{< TransferCFT/axwayvariablesCompanyName  >}}, but the file transfer can be managed by {{< TransferCFT/axwayvariablesComponentShortName  >}}. A common use case is a bank portal that provides links to download bank statements or loan documents. The end-user experience is online banking, and behind the scenes, the bank ensures secure file upload and download with the audit, security, and benefits of MFT.
