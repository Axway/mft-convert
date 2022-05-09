{
    "title": "Extended  catalog query APIs",
    "linkTitle": "Extended catalog query APIs",
    "weight": "160"
}This topic describes the additional
fields relating to transport security that can be accessed via the catalog
query APIs (cftaix). These fields are used to determine whether transport
security was implemented for a terminated transfer, and if so, the session
parameters (authentication mode, suite negotiated, certificates used,
and so on).

<span id="Extended_catalog_query_API_structure"></span>

### Extended catalog query API structure

The output structure of the extended catalog query API, cftaix, has
been modified. New fields relating to transport security have been added.
These fields are as follows:

```
char cMode ;             /\* Session mode (client/server) \*/
char cAuthPolicy ;       /\* Session auth (server/both) \*/
char sCipher[3+1] ;      /\* Session cipher suite \*/
char sParm[64+1] ;       /\* Profile free parameter \*/
char sRemoteCn[32+1];    /\* CN of remote user certificate \*/
char sRemoteCaId[8+1];   /\* CA alias of remote user certificate \*/
char sUserCId[8+1];      /\* Local user certificate alias \*/
char sCertFname[64+1];   /\* File including remote DER certificate \*/
char sProf[8+1] ;        /\* SSL profile ID \*/
```

The following table describes the new fields declared in the structure
returned by the extended catalog query API, cftaix.

All string-type fields, char name[n+1], are expressed in text format,
either ASCII or EBCDIC depending on the system, are left justified, and
terminated by a null byte.

****Fields in cftaix queries****


| Field  | Description  |
| --- | --- |
| cMode  | Direction of the SSL session in which the transfer was performed.<br/> C signifies client and S signifies server.  |
| cAuthPolicy  | Authentication mode of the SSL session in which the transfer was performed.<br/> • S signifies that only the server was authenticated.<br/> • B signifies that the client and server were authenticated.<br/> • A signifies that the anonymous mode has been implemented.  |
| sCipher  | Suite negotiated for the SSL session.<br/> This suite is set to one of the values from the suites supported by Transfer CFT (1, 2, 4, 5, 9, 10 or 47).  |
| sParm  | Value of the PARM parameter in the CFTSSL command used to negotiate the session parameters.  |
| sRemoteCn  | CN field of the user certificate presented by the remote partner.  |
| sRemote CaId  | Certificate identifier (in the local certificate database) of the root authority of the certificate presented by the remote partner.  |
| sUserCId  | Identifier (in the local certificate database) of the user certificate used locally for authentication by the remote partner.  |
| sCertFname  | Physical name of the file in which the certificate presented by the remote partner was recorded.  |
| sProf  | Identifier of the CFTSSL command used to negotiate the session parameters.  |

