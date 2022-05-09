{
    "title": "Get started with Web services",
    "linkTitle": "Get started with Web services",
    "weight": "310"
}This section describes how to get started using Web services with {{< TransferCFT/axwayvariablesComponentLongName  >}}. You can use a tool such as SoapUI to create and test SOAP requests. A typical usage for Web services when running Transfer CFT is:

- To use web services to submit a high number of transfer requests.
- To trace exchanges using the transfer requests' IDT and IDTU.

When using Web services with {{< TransferCFT/headerfootervariableshflongproductname  >}}, select the type media communication type, either COM FILE or COM TCP, to use for transfer requests (SEND/RECV), or administration services (such as HALT). If you are using Web services with TCP as the selected COM media type, refer to [About synchronous communication](../../../synch_comm_tcpip_intro) for information on configuring the client.

Prerequisites
-------------

You require a tool for developing Web services requests, such as SoapUI.

- SoapUI information: [www.soapui.org/about-soapui/what-is-soapui-.html](http://www.soapui.org/about-soapui/what-is-soapui-.html)
- SoapUI download: [www.soapui.org/downloads/latest-release.html](http://www.soapui.org/downloads/latest-release.html)

View available operations
-------------------------

You can access a full list of available operations at: &lt;Transfer_CFT_install_dir&gt;\\home\\distrib\\copilot\\wsdl\\doc\\index.html

Select communication media
--------------------------

Regardless of the mode you select, COM FILE or [COM TCP](../../../synch_comm_tcpip_intro), the service is the same; the only difference in comportment is the performance and error management (including transfer tracking).

### COM FILE

- Pros: There is no effect on persistence; if {{< TransferCFT/headerfootervariableshflongproductname  >}} stops the request is stored in the COM file. Additionally, there is no need to implement error handling on the client side except if the communication file is full (see Note below).
- Cons: The performance response time per request is at the very least `copilot.cft.timerwaitcftcata` seconds, where the maximum time could be `copilot.cft.timerwaitcftcata` multiplied by the `copilot.cft.nbwaitcftcata` value.

> **Note**
>
> Note: If Transfer CFT is not started the transfer requests accumulate in the communication file. However the communication file size is limited. If the com file is full, requests fail leading to an error &lt;RETURN_MESSAGE&gt;ERROR : mediacom is full (-411/0). CSCcom()&lt;/RETURN_MESSAGE&gt;.

### COM TCP

- Pros: Performance is improved due to a faster request response time (under ideal conditions less than 1 second, however the actual time depends on the Transfer CFT process load, as opposed to `copilot.cft.timerwaitcftcata` seconds at the very least if you are using COM FILE).
- Cons: There is an effect on persistence - if {{< TransferCFT/headerfootervariableshflongproductname  >}} stops the request is not recorded. Additionally, error handling must be implemented on the client.

Customize the Transfer CFT COM configuration
--------------------------------------------

According to the mode you select, FILE or TCP, you can modify the following parameters in the unified configuration to optimize performance and tracking.

### COM FILE

When using COM FILE the two relevant parameters to consider are `copilot.cft.timerwaitcftcata` and `copilot.cft.nbwaitcftcata`. If you use the default values of 5 and 6 respectively, the minimum wait time is 5 seconds and the maximum wait time for each request is 30 seconds (5 x 6).

Concerning errors when using COM TCP mode, if CFTMAIN stops the connection is lost on the server side and there is no retry. The client then receives an error code 7 with the message " ERROR: Open channel failed (-6006/0)".

### COM TCP or COM FILE

When using either COM FILE or COM TCP, the `copilot.misc.maxnbprocess` defines the maximum number of parallel sessions accepted by the Transfer CFT Copilot server (default is 20). You can modify this value in function with the peaks in parallel requests your system experiences (maximum is 64), however consider the impact of having a high number of parallel processes. Lastly note that each process treats only one application at a time.

Any requests that exceed the limit cause a network error with either a " connection refused" or "connection closed by remote" type of error.

> **Note**
>
> Note: In multi-node/multi-host on a Transfer CFT Copilot server, the maximum number of parallel requests is copilot.misc.maxnbprocess multiplied by n (where n is the number of hosts).

Create a request
----------------

The following procedure is based on using the SoapUI tool. The exact steps may vary depending on the Web services tool you use.

1. Start SoapUI.
1. From the File menu, select New SOAP Project to create a project:
    -   Project Name: Transfer_CFT
    -   Initial WSDL: &lt;CFTINSTALLDIR&gt;\\home\\distrib\\copilot\\wsdl\\copilotcft.wsdl
1. Select and expand the service to use, for example:
    -   XFER_CMD_SEND_FILE to send a file, or
    -   XFER_CAT_SELECT to monitor a transfer request
1. Enter the Copilot server URL in the request window: &lt;host&gt;:&lt;copilot server port&gt;
1. Modify the request as needed. See also:
    -   [Perform a send file request](../example_send_request)
    -   [Perform a catalog search request](../example_search_catalog)
1. Submit the request.
1. Check the result. See [successful or unsuccessful responses.](../example_send_request)

Return codes
------------

When using Web services the return codes are as follows:


| Name  | Description  | XTS <br/> mapping |
| --- | --- | --- |
| RCVA_RESPONSE_NOK_VALUE_ERROR  | Value error  | 10  |
| RCVA_RESPONSE_NOK_NOT_FOUND  | Not found  | 11  |
| RCVA_PASSPORT_ERROR  | Passport AM interrogation error  | 12  |
| RCVA_RESPONSE_OK  | Ok  | 3  |
| RCVA_RESPONSE_OK_POSTED  | Inquiry filed  | 4  |
| RCVA_RESPONSE_OK_EXECUTED  | Inquiry executed  | 5  |
| RCVA_RESPONSE_NOK  | Error  | 6  |
| RCVA_RESPONSE_NOK_IO_ERROR  | Error when opening, writing, reading, or closing  | 7  |
| RCVA_RESPONSE_NOK_MEMORY_ERROR  | Memory error  | 8  |
| RCVA_RESPONSE_NOK_PARAMETER_ERROR  | Parameter error  | 9  |


****Related topics****

[About Web services](../../../../cft_intro_install/about_this_document_ibmi/using_apis/about_web_services)
