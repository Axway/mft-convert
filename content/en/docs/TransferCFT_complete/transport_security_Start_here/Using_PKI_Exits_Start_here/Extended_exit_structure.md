{
    "title": "Extended exit structure",
    "linkTitle": "Extended exit structure",
    "weight": "220"
}This table displays the fields and communication structure for the
directory, end of transfer, and file exits in server mode:

Exit structure

```
char cMode;      /\* Client/Server \*/
char cAuthPolicy;      /\* Server/Both \*/
char bCipher;      /\* Cipher suite \*/
char sParm[64+1];      /\* Free parameter from CFTSSL
command \*/
char sRemoteUserDn[256+1]; /\* Remote user certificate DN \*/
char sRemoteIssuerDn[256+1]; /\* Remote issuer DN \*/
char sRemoteCaId[8+1];      /\* Remote CA alias \*/
char sUserCId[8+1];           
/\* Local user alias \*/
char sCertFname[64+1];      /\* File including remote
DER certificate \*/
char sProf[8+1] ;      /\* SSL profile ID. \*/
char sRemoteSerial[64+1];      /\* Serial number \*/
char zExitFree[64];      /\* Free area for external
PKI \*/
```
