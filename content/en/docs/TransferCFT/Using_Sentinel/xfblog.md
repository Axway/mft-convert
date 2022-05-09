{
    "title": "XFBLog Tracked Object",
    "linkTitle": "XFBLog Tracked Object",
    "weight": "220"
}When you configure Sentinel to monitor Transfer CFT, you can import
the XFBLog Tracked Object. The XFBLog is a Tracked Object that describes
the contents of an Transfer CFT log file. This log stores traces of the
events and errors that occur on Transfer CFT. This topic describes the XFBLog attributes.

XFBLog attributes
-----------------

The XFBLog includes the following attributes.


| Attribute  | Description  | Type  | Length  |
| --- | --- | --- | --- |
| ApplicationName  | Application name<br/> CFT: PART (CFTPARM) | Variable string  | 40  |
| CodeMsg  | Message code  | Variable string  | 10  |
| IdentMsg  | Message identifier CFTxYYz  | Variable string  | 10  |
| Monitor  | Hardcoded value to: XFB  | Variable string  | 10  |
| Product  | Product name:  | Variable string  | 10  |
| RecDate  | Recorded date in local log file  | Date  |   |
| RecTime  | Recorded time in local log file  | Time  |   |
| SeverityClass  | Severity class:<br/> ES – System Error<br/> EC – Component Error<br/> EM – Message Error<br/> AV – Error and Warning<br/> IG – General Information<br/> IP – Program Information | Variable string  | 10  |
| SessionTag  | Session tag  | Variable string  | 20  |
| TransferTag  | Transfer tag  | Variable string  | 20  |


****Related topics****

[XFBTransfer requests](../xfbtransfer_request)
