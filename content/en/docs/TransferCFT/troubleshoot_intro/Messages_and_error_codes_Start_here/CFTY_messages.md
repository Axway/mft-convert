---
title: "Transfer CFT messages: CFTY"
linkTitle: "CFTY messages"
weight: 400
--- This topic lists the CFTYxx (CFT xnnx) messages and provides the type, a description, consequence, and corrective actions when applicable.

**Message format**

Earlier versions of Transfer CFT used a different message format than version 3.1.3 and higher. This document displays both formats when applicable and available, otherwise only the v23 is used. Using the CFTLOG Format = V24 setting, the log displays as shown:

`CFTXXX: fixed text message <variables>`

**Example**

CFTLOG FORMAT=[V23,V24]

For V23: `CFTT57I PART=&part IDF=&idf IDT=&idt &str transfer started`

For V24: `CFTT57I &str transfer started   <IDTU=&idtu PART=&part IDF=&idf IDT=&idt>`

| V23 format<br/> V24 format<br/> Information | <span id="CFTY03E"></span>CFTY03E PID=&amp;pid System error [&amp;string] CR=&amp;cr CS=&amp;cs<br/> CFTY03E PID=&amp;pid System error [&amp;string] CR=&amp;cr CS=&amp;cs |
| --- | --- |
| Explanation | A new SSL task cannot initialize its working environment. According to the error origin, various messages are given below. |
| Result | The SSL session in progress is aborted. |
| V23 format<br/> V24 format<br/> Information | CFTY03E PID=&amp;pid System error [MMALLOC] CR=&amp;cr CS=&amp;cs<br/> CFTY03E PID=&amp;pid System error [MMALLOC] CR=&amp;cr CS=&amp;cs |
| Explanation | Dynamic memory allocation failure. |
| V23 format<br/> V24 format<br/> Information | CFTY03E PID=&amp;pid System error [SYDEF] CR=&amp;cr CS=&amp;cs<br/> CFTY03E PID=&amp;pid System error [SYDEF] CR=&amp;cr CS=&amp;cs |
| Explanation | Task semaphore creation failure:<br/> • CR=- 1 Maximum semaphore count reached<br/> • CR=- 2 Internal error<br/> • CR=- 9 System error |
| V23 format<br/> V24 format<br/> Information | CFTY03E PID=&amp;pid System error [SYPOST] CR=&amp;cr CS=&amp;cs<br/> CFTY03E PID=&amp;pid System error [SYPOST] CR=&amp;cr CS=&amp;cs |
| Explanation | Semaphore write failure:<br/> • CR=- 1 Undefined or already closed semaphore<br/> • CR=- 2 Too many messages waiting in semaphore<br/> • CR=- 3 Message length too long <br/> • CR=- 9 System error |
| V23 format<br/> V24 format<br/> Information | CFTY03E PID=&amp;pid System error [SYWAIT] CR=&amp;cr CS=&amp;cs<br/> CFTY03E PID=&amp;pid System error [SYWAIT] CR=&amp;cr CS=&amp;cs |
| Explanation | Semaphore read failure:<br/> • CR=- 1 Undefined semaphore<br/> • CR=- 3 Already closed semaphore<br/> • CR=- 9 System error |
| V23 format<br/> V24 format<br/> Information | CFTY03E PID=&amp;pid System error [CTXDEF] CR=&amp;cr CS=&amp;cs<br/> CFTY03E PID=&amp;pid System error [CTXDEF] CR=&amp;cr CS=&amp;cs |
| Explanation | SSL session context manager creation failure:<br/> • CR=- 2,- 3 Dynamic memory allocation failure<br/> • CR=- 9 Maximum context manager count reached |
| V23 format<br/> V24 format<br/> Information | CFTY03E PID=&amp;pid System error [STARTPKI] CR=&amp;cr CS=&amp;cs<br/> CFTY03E PID=&amp;pid System error [STARTPKI] CR=&amp;cr CS=&amp;cs |
| Explanation | PKI internal error. The CS code is in the form « PKII nnn » for a Transfer CFT internal PKI error or « PKIE nnn » for an external PKI error. nnn is a SSL alert code. See SSL alert errors. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY04E"></span>CFTY04E PID=&amp;pid PKIFNAME=&amp;string Internal PKI error [&amp;string] CR=&amp;cr<br/> CS=&amp;cs CFTY04E PID=&amp;pid PKIFNAME=&amp;string Internal PKI error [&amp;string] CR=&amp;cr CS=&amp;cs |
| --- | --- |
| Explanation | A new SSL task cannot read the index file of the local certificate data base.<br/> This file name is set by default in the CFTPARM object. Depending on the error's origin, the message could be one of the following: |
| Result | The SSL session in progress is aborted. |
| V23 format<br/> V24 format<br/> Information | CFTY04E PID=&amp;pid PKIFNAME=&amp;string Internal PKI error [FMALLOC] CR=&amp;cr CS=&amp;cs<br/> CFTY04E PID=&amp;pid PKIFNAME=&amp;string Internal PKI error [FMALLOC] CR=&amp;cr CS=&amp;cs |
| Explanation | File allocation failure:<br/> • CR=- 1: File not found<br/> • CR=- 3: File already allocated (exclusive mode) by another application<br/> • CR=- 9: System error |
| V23 format<br/> V24 format<br/> Information | CFTY04E PID=&amp;pid PKIFNAME=&amp;string Internal PKI error [DMOPEN] CR=&amp;cr CS=&amp;cs<br/> CFTY04E PID=&amp;pid PKIFNAME=&amp;string Internal PKI error [DMOPEN] CR=&amp;cr CS=&amp;cs |
| Explanation | File open failure:<br/> • CR=- 1: File not allocated<br/> • CR=- 2: Invalid open mode<br/> • CR=- 3: Access conflict<br/> • CR=- 9: System error |
| V23 format<br/> V24 format<br/> Information | CFTY04E PID=&amp;pid PKIFNAME=&amp;string Internal PKI error [DMGN] CR=&amp;cr CS=&amp;cs<br/> CFTY04E PID=&amp;pid PKIFNAME=&amp;string Internal PKI error [DMGN] CR=&amp;cr CS=&amp;cs |
| Explanation | File read failure:<br/> • CR=- 9: System error |
| V23 format<br/> V24 format<br/> Information | CFTY04E PID=&amp;pid PKIFNAME=&amp;string Internal PKI error [HPUT] CR=&amp;cr CS=&amp;cs<br/> CFTY04E PID=&amp;pid PKIFNAME=&amp;string Internal PKI error [HPUT] CR=&amp;cr CS=&amp;cs |
| Explanation | File loading error:<br/> • CR=- 3: Dynamic memory allocation error when loading file |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY05E"></span>CFTY05E PID=&amp;pid PKIFNAME=&amp;file Syntax error _ &amp;string<br/> CFTY05E PID=&amp;pid PKIFNAME=&amp;file Syntax error _ &amp;string |
| --- | --- |
| Explanation | The index file of the local certificate data base is not valid. This file name is set by default in the CFTPARM object.<br/> Depending on the error's origin, the message could be one of the following:<br/> • MISSING SECTION=TrustedCas: the file doesn’t contain a [TrustedCas] section. This section is used to declare certificate authorities (CA)<br/> • SECTION=TrustedCas IS EMPTY: [TrustedCas] section is empty<br/> • BAD VALUE LINE=linenumber: Invalid syntax for a certificate or private key statement. The line number in the file is displayed<br/> • INVALID SECTION LINE=linenumber: Invalid syntax for a section statement. The line number in the file is displayed |
| Result | The SSL session in progress is aborted. |
| Action  | Rectify the index file. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY06E"></span>CFTY06E CTX=&amp;ctx Certificate Request Message error _ &amp;string<br/> CFTY06E CTX=&amp;ctx Certificate Request Message error _ &amp;string |
| --- | --- |
| Explanation | SSL handshake error: a request certificate message, sent by the server, is invalid. According to the error origin, various reasons are given below:<br/> • UNSUPPORTED TYPE FIELD: The server requires an authentication type which is not supported by {{< TransferCFT/axwayvariablesComponentShortName  >}}.<br/> • INVALID DN LENGTH: The DN (Distinguished Name) length is invalid |
| Result | The SSL session in progress is aborted. An alert is sent to the server. |
| V23 format<br/> V24 format<br/> Information | <span id="CFTY07E"></span>CFTY07E CTX=&amp;ctx System error [&amp;string] CR=&amp;cr CS=&amp;cs<br/> CFTY07E CTX=&amp;ctx System error [&amp;string] CR=&amp;cr CS=&amp;cs |
| Explanation | SSL handshake error. According to the error origin, various messages are given below. |
| Result | The SSL session in progress is aborted. An alert is sent to the remote entity. |
| V23 format<br/> V24 format<br/> Information | CFTY07E CTX=&amp;ctx System error [MMALLOC] CR=&amp;cr CS=&amp;cs<br/> CFTY07E CTX=&amp;ctx System error [MMALLOC] CR=&amp;cr CS=&amp;cs |
| Explanation | Dynamic memory allocation failure. |
| Result | If the SSL handshake is in progress, the session is aborted and an alert is sent to the remote entity.<br/> If the SSL session is established (handshake successful) the network session is cleared. |
| V23 format<br/> V24 format<br/> Information | CFTY07E CTX=&amp;ctx System error [SYPOST] CR=&amp;cr CS=&amp;cs<br/> CFTY07E CTX=&amp;ctx System error [SYPOST] CR=&amp;cr CS=&amp;cs |
| Explanation | Semaphore write failure:<br/> • CR=- 1 :The semaphore is undefined or already closed<br/> • CR=- 2: Too many messages are waiting on the semaphore<br/> • CR=- 3: The message length is too big<br/> • CR=- 9: System error |
| V23 format<br/> V24 format<br/> Information | CFTY07E CTX=&amp;ctx System error [CTXALLOC] CR=&amp;cr CS=&amp;cs<br/> CFTY07E CTX=&amp;ctx System error [CTXALLOC] CR=&amp;cr CS=&amp;cs |
| Explanation | Memory allocation error for a new SSL session context:<br/> • CR=- 2,- 3: Dynamic memory allocation failure<br/> • CR=- 9: Maximum context count reached |
| V23 format<br/> V24 format<br/> Information | CFTY07E CTX=&amp;ctx System error [CTXCHK] CR=&amp;cr CS=&amp;cs<br/> CFTY07E CTX=&amp;ctx System error [CTXCHK] CR=&amp;cr CS=&amp;cs |
| Explanation | Invalid message received from another {{< TransferCFT/axwayvariablesComponentShortName  >}} task (context is invalid or already free). |
| V23 format<br/> V24 format<br/> Information | CFTY07E PROT=&amp;prot SSLPID=&amp;pid HOST=&amp;host synchronization error CR=&amp;cr CS=&amp;scs<br/> CFTY07E PROT=&amp;prot SSLPID=&amp;pid HOST=&amp;host synchronization error CR=&amp;cr CS=&amp;scs |
| Explanation | Problem with sending an internal {{< TransferCFT/axwayvariablesComponentShortName  >}} message to the protocol task during the SSL initialization phase |
| Consequence | The transfer is aborted |
| Action | Analyze the &amp;scs code and contact the product support team if necessary |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY08I"></span>CFTY08I PID=&amp;pid Task started successfully<br/> CFTY08I PID=&amp;pid Task started successfully |
| --- | --- |
| Explanation | Successful creation of a new SSL task. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY09I"></span>CFTY09I PID=&amp;pid Task ended<br/> CFTY09I PID=&amp;pid Task ended |
| --- | --- |
| Explanation | SSL task ended. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY10E"></span>CFTY10E PID=&amp;pid CTX=&amp;ctx Invalid reference on &amp;string<br/> CFTY10E PID=&amp;pid CTX=&amp;ctx Invalid reference on &amp;string |
| --- | --- |
| Explanation | Invalid network message received (context is invalid or already free). |
| Result | Message is not treated. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY11I"></span>CFTY11I CTX=&amp;ctx PART=&amp;id SSL=&amp;id Closing client SSL session<br/> CFTY11I CTX=&amp;ctx PART=&amp;id SSL=&amp;id Closing client SSL session |
| --- | --- |
| Explanation | A client SSL session is closed. The session reference and the transfer partner are displayed. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY12I"></span>CFTY12I CTX=&amp;ctx PROT=&amp;id SSL=&amp;id Closing server SSL session<br/> CFTY12I CTX=&amp;ctx PROT=&amp;id SSL=&amp;id Closing server SSL session |
| --- | --- |
| Explanation | A server SSL session is closed. The session reference and the protocol are displayed. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY13E"></span>CFTY13E CTX=&amp;ctx SSL Handshake local error [&amp;string] CR=&amp;cr<br/> CFTY13E CTX=&amp;ctx SSL Handshake local error [&amp;string] CR=&amp;cr |
| --- | --- |
| Explanation | SSL session handshake failure. |
| Result | The SSL session in progress is aborted. An alert is sent to the remote entity. |
| Action | Call the {{< TransferCFT/axwayvariablesComponentShortName  >}} hot line.<br/> Analyze the &amp;cr error code (refer to the SSL protocol error codes). Contact the product support team if necessary. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY14I"></span>CFTY14I CTX=&amp;ctx PART=&amp;id SSL=&amp;id client session established VERSION=&amp;ver CIPHER=&amp;num AUTH=&amp;mode CFTY14I CTX=&amp;ctx PART=&amp;id SSL=&amp;id client session established VERSION=&amp;ver CIPHER=&amp;num AUTH=&amp;mode<br/>  |
| --- | --- |
| Explanation | Successful handshake. A new client SSL session is established. The negotiated version, cypher suite, and the authentication mode (SERVER or BOTH) are displayed. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY15I"></span>CFTY15I CTX=&amp;ctx PROT=&amp;id SSL=&amp;id server session established VERSION=&amp;ver CIPHER=&amp;num AUTH=&amp;mode CFTY15I CTX=&amp;ctx PROT=&amp;id SSL=&amp;id server session established VERSION=&amp;ver CIPHER=&amp;num AUTH=&amp;mode |
| --- | --- |
| Explanation | Successful handshake. A new server SSL session is established. The negotiated version, cypher suite, and the authentication mode (SERVER or BOTH) are displayed. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY16I"></span>CFTY16I CTX=&amp;ctx &amp;message<br/> CFTY16I CTX=&amp;ctx &amp;message |
| --- | --- |
| Explanation | Message sent by the external PKI exit. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY17I"></span>CFTY17I CTX=&amp;ctx &amp;msg<br/> CFTY17I CTX=&amp;ctx &amp;msg |
| --- | --- |
| Explanation | Specific exit security (PKI System) message. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY18E"></span>CFTY18E CTX=&amp;ctx &amp;str<br/> CFTY18E CTX=&amp;ctx &amp;str |
| --- | --- |
| Explanation | Internal error on calling up the internal PKI. The "&amp;str" field can have the following values:<br/> • PKI_NOT_TREATED : PKI function not treated<br/> • PKI_ERR_CERT_BAD : Incorrect certificate (format error)<br/> • PKI_ERR_CERT_UNSUPPORTED : Certificate not supported<br/> • PKI_ERR_CERT_REVOKED : Certificate revoked<br/> • PKI_ERR_CERT_EXPIRED : Certificate expired<br/> • PKI_ERR_CERT_UNKNOWN : Certificate unknown<br/> • PKI_ERR_CERT_NOT_VALID : Certificate not valid<br/> • PKI_ERR_CERT_BAD_SIGN : Integrity error (incorrect signature)<br/> • PKI_ERR_CERT_BAD_HASH : Integrity error (hash code incorrect)<br/> • PKI_ERR_CERT_BAD_CA :Certification organism certificate invalid<br/> • PKI_ERR_CERT_ALGO_UNSUPPORTED : Unsupported ciphering algorithm<br/> • PKI_ERR_CERT_NOT_FOUND : User certificate not found<br/> • PKI_ERR_CA_NOT_FOUND : Certification organism certificate not found<br/> • PKI_ERR_BAD_KEY : Invalid ciphering key<br/> • PKI_ERR_BUF_TOO_SHORT : Memory buffer size too small<br/> • PKI_ERR_SYS : Internal error linked to the system (memory allotment, system function, and so on)<br/> • PKI_ERR_PARM : Ciphering parameter invalid<br/> • PKI_ERR_OTHERS : Other error (authentication, ciphering, integrity, and so on) |
| Consequence | The transfer is aborted. |
| Action | Contact the product support team if necessary. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY19I"></span>CFTY19I PART=&amp;id SSL=&amp;id opening client session CTX=&amp;ctx on task PID=&amp;pid<br/> CFTY19I PART=&amp;id SSL=&amp;id opening client session CTX=&amp;ctx on task PID=&amp;pid |
| --- | --- |
| Explanation | Handshake is started for a new client SSL session. {{< TransferCFT/axwayvariablesComponentShortName  >}} gives a unique reference to it. Using this reference, the session could be tracked. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY20I"></span> CFTY20I PROT=&amp;id SSL=&amp;id opening server session CTX=&amp;ctx on task PID=&amp;pid<br/> CFTY20I PROT=&amp;id SSL=&amp;id opening server session CTX=&amp;ctx on task PID=&amp;pid |
| --- | --- |
| Explanation | Handshake is started for a new server SSL session. {{< TransferCFT/axwayvariablesComponentShortName  >}} gives a unique reference to it. Using this reference, the session could be tracked. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY21I"></span>CFTY21I CTX=&amp;ctx Remote server certificate accepted<br/> CFTY21I CTX=&amp;ctx Remote server certificate accepted |
| --- | --- |
| Explanation | A server certificate is accepted during a session handshake. The authority identifier which has signed the certificate is displayed. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY22I"></span>CFTY22I CTX=&amp;ctx Remote client certificate accepted<br/> CFTY22I CTX=&amp;ctx Remote client certificate accepted |
| --- | --- |
| Explanation | A client certificate is accepted during a session handshake. The authority identifier which has signed the certificate is displayed. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY23I"></span>CFTY23I CTX=&amp;ctx Client certificate ID=&amp;id ROOTID=&amp;id<br/> CFTY23I CTX=&amp;ctx Client certificate ID=&amp;id ROOTID=&amp;id |
| --- | --- |
| Explanation | Client certificate used locally for authentication. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY24I"></span>CFTY24I CTX=&amp;ctx Server certificate ID=&amp;id ROOTID=&amp;id<br/> CFTY24I CTX=&amp;ctx Server certificate ID=&amp;id ROOTID=&amp;id |
| --- | --- |
| Explanation | Server certificate used locally for authentication. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY25I"></span>CFTY25I CTX=&amp;ctx remote address HOST=&amp;string<br/> CFTY25I CTX=&amp;ctx remote address HOST=&amp;string |
| --- | --- |
| Explanation | This message is displayed after the message CFTY20I. It gives the address (HOST name or IP address using TCP/IP network) of the remote connected entity. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY26I"></span>CFTY26I CTX=&amp;ctx Anonymous &amp;str session<br/> CFTY26I CTX=&amp;ctx Anonymous &amp;str session |
| --- | --- |
| Explanation | Opening of a secure session without authentication in either client or server mode. Refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} Online documentation.<br/> • &amp;ctx= context SSL<br/> • str = client or server |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY28W"></span>CFTY28W CTX=&amp;ctx &amp;str2 = &amp;filename<br/> CFTY28W CTX=&amp;ctx &amp;str2 = &amp;filename |
| --- | --- |
| Explanation | The file contains the remote certificate has not been recorded. |
| Consequence | The transfer can be performed but the remote certificate is not recorded. |
| Action | Write down the &amp;str2 value and contact the product support team if necessary. |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY30E"></span>CFTY30E CTX=&amp;ctx SSL Handshake remote error [&amp;string] CR=&amp;cr<br/> CFTY30E CTX=&amp;ctx SSL Handshake remote error [&amp;string] CR=&amp;cr |
| --- | --- |
| Explanation | SSL session handshake failure. An alert has been received from the remote peer.  |
| Consequence | The SSL session in progress is aborted.  |
| Action | Analyze the &amp;cr error code (refer to the SSL protocol error codes). Also check the remote partner log for more details. Contact Axway support if necessary.  |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY50I"></span>CFTY50I SSH client session established CTX=&amp;ctx PART=&amp;part Version=&amp;version Cipher in=&amp;cipher Cipher out=&amp;cipher Key exchange=&amp;key hmac in=&amp;hmac hmac out=&amp;hmac &amp;IPVersion<br/> CFTY50I SSH client session established CTX=&amp;ctx PART=&amp;part Version=&amp;version Cipher in=&amp;cipher Cipher out=&amp;cipher Key exchange=&amp;key hmac in=&amp;hmac hmac out=&amp;hmac |
| --- | --- |
| Explanation  | Established a new SSH client session. &amp;version is SSH1 or SSH2.  |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY51I"></span>CFTY51I SSH server session established CTX=&amp;ctx PART=&amp;part Version=&amp;version Cipher in=&amp;cipher Cipher out=&amp;cipher Key exchange=&amp;key hmac in=&amp;hmac hmac out=&amp;hmac &amp;IPVersion<br/> CFTY51I SSH server session established CTX=&amp;ctx PROT=&amp;prot Version=&amp;version Cipher in=&amp;cipher Cipher out=&amp;cipher Key exchange=&amp;key hmac in=&amp;hmac hmac out=&amp;hmac |
| --- | --- |
| Explanation  | Established a new SSH client session. &amp;version is SSH1 or SSH2.  |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY52I"></span>CFTY52I SFTP client session established CTX=&amp;ctx PART=&amp;part Version=&amp;version Authentication=&amp;authentication CFT Server=&amp;server<br/> CFTY52I SFTP client session established CTX=&amp;ctx PART=&amp;part Version=&amp;version Authentication=&amp;authentication CFT Server=&amp;server |
| --- | --- |
| Explanation  | Established a new SFTP client session. &amp;version is the SFTP version negotiated with the partner (&gt;= 3). &amp;Authentication is Key or Password. &amp;server indicates if we are connected to a Transfer CFT server (Yes or No).  |

| V23 format<br/> V24 format<br/> Information | <span id="CFTY53I"></span>CFTY53I SFTP server session established CTX=&amp;ctx PROT=&amp;prot Version=&amp;version Authentication=&amp;authentication CFT Client=&amp;client<br/> CFTY53I SFTP server session established CTX=&amp;ctx PROT=&amp;prot Version=&amp;version Authentication=&amp;authentication CFT Client=&amp;client |
| --- | --- |
| Explanation  | Established a new SFTP server session. &amp;version is the SFTP version negotiated with the partner (&gt;= 3).<br/> • &amp;Authentication is Password, System, PassPortAM, Key or XFBADM.<br/> • &amp;client indicates if you are connected to a Transfer CFT client. |

