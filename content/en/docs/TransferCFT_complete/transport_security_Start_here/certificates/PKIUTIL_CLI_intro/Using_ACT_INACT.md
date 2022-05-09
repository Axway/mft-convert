{
    "title": "Using  the ACT and INACT commands",
    "linkTitle": "Using ACT and INACT",
    "weight": "220"
}Activating or deactivating a certificate
----------------------------------------

****Syntax****

`ACT or INACT`

`ROOTCID     =       string,`

`ID = string,`

`[INUM = number,]`

`TYPE = string`

 

Description: Use the ACT and INACT commands to activate or deactivate
one or more certificates in the local database. If a root
or intermediate authority certificate is deactivated, all dependent certificates
(user or intermediate authorities) are automatically deactivated.

The syntax is the same for both commands.


| Parameter  | Description  |
| --- | --- |
| ID = string1..8 | Unique local identifier of the certificate(s) to be activated or deactivated, depending on the command.<br/> The * and ? wildcard characters are accepted for the ID parameter value. |
| [ INUM= number1..99]  | Internal number for the intermediate certificates in an imported chain of certificates (in the PKI database). |
| [PKIFNAME = string1..64]  | [PKIFNAME = string1..64] *Obsolete for Windows/Unix.*  |
| ROOTCID = string1..8 | Local identifier of the authority of the certificates to be enabled/disabled.<br/> This parameter must even be indicated for an authority certificate. In this case, the ID and ROOTCID parameters have the same value. |
| TYPE = KEY  | To activate/deactivate the PKIKEY.  |

