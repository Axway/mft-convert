{
    "title": "Transfer CFT internal datafile directory",
    "linkTitle": "Installed directories",
    "weight": "200"
}Logical name CFT_DAT points to D$CFT:[CFT.RUNTIME.DATA] directory.

It contains the internal datafile files used by Transfer CFT.


| File | Contents |
| --- | --- |
| CFTPARM.INX  | CFT PARAMETER file. This file is not supplied. It is created by CFTUTIL in the CFTDAT procedure.  |
| CFTPART.INX  | CFT PARTNER file. This file is not supplied. It is created by CFTUTIL in the CFTDAT procedure.  |
| CFTCATA.REL | CFT CATALOG file.<br /> This file is not supplied. It is created by CFTUTIL in the CFTDAT procedure. |
| CFTCOM.REL | CFT COMMUNICATION file.<br /> This file is not supplied. It is created by CFTUTIL in the CFTDAT procedure. |
| CFTKEY.PARM  | This file contains the CFT license key  |
| SEND.DIR  | Directory with default file (sample) to send when it is not specified.  |
| RECV.DIR  | Default directory to receive the transferred files.  |


### X.509 certificate directory

This directory contains the X.509 certificates.

D$CFT:[CFT.CFTPKI] =&gt; CFT_PKI Directory

### Transfer CFT log file directory

Logical name CFT_LOG points to D$CFT:[CFT.RUNTIME.LOG] directory.

It stores Transfer CFT log files.


| File | Contents |
| --- | --- |
| CFTLOG.LOG | CFT LOG file.<br /> This file is not supplied. It is created by CFTUTIL in the CFTDAT procedure. |
| CFTLOG.LOGA | CFT LOG file.<br /> This file is not supplied. It is created by CFTUTIL in the CFTDAT procedure. |
| CFTACCNT.LOG | CFT ACCOUNT file.<br /> This file is not supplied. It is created by CFTUTIL in the CFTDAT procedure. |
| CFTACCNT.LOGA | CFT ACCOUNT file.<br /> This file is not supplied. It is created by CFTUTIL in the CFTDAT procedure. |
| CFTMAIN.LOG  | Log of CFTMAIN task.  |
| COPSMNG.LOG  | Log of Copilot task.  |
| TRKTRACE.LOG  | Log of Sentinel task.  |


### Files to send directory

The D$CFT:[CFT.CFTSEND] =&gt; CFT_SEND directory used to store the files to be sent in the sample configurations.

`D$CFT:[CFT.CFTSEND] => CFT_SEND directory         </code></p>`


| File | Contents |
| --- | --- |
| TEST.SND | Variable sequential file. |
| FIXED.SND | Fixed sequential file. |


### Rebuild Transfer CFT directory

The D$CFT:[CFT.LIB] =&gt; XMK_BDIR:[LIB] directory contains the libraries and objects required to rebuild Transfer CFT.
