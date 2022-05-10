---
    "title": "Entities - PKIENTITY",
    "linkTitle": "Entities - PKIENTITY",
    "weight": "230"
---
This topic describes how to create lists of certificate authority IDs in the PKI database.

The maximum number of CAs that you can enter for the ROOTCID parameter of the CFTSSL command is 10. To overcome this limitation, use the PKIENTITY object to create a list of up to 100 certificate authority IDs. You can then enter up to ten PKIENTITY objects in the CFTSSL ROOTCID parameter, to enable a maximum of 1000 certificate authorities IDs in the PKI database.

> **Note**
>
> Note: You can directly update PKIENTITY content in the PKI internal datafile without modifying Transfer CFT parameter settings.

{{% TransferCFT/snippets/parm_not_in_UI%}}

Define a certificate list
-------------------------

To define a certificate list, use the PKIENTITY parameters:


| Parameter  | Description  |
| --- | --- |
| ID | An identifier is a case-insensitive string with a maximum of 32 characters. If the identifier contains spaces, enclose the identifier in single quotes. (mandatory) |
| CERTIFICATES<br />  | A list of up to 100 certificate IDs. Each ID is a case-insensitive string with a maximum of 32 characters. There is no check other than syntax when you insert this parameter, so if you use an ID in the CERTIFICATES list that is the same as a PKIENTITY object ID {{< TransferCFT/axwayvariablesComponentShortName  >}} ignores this ID when loading CFTSSL properties.  |
| MODE | An action on the certificate, CREATE, REPLACE, or DELETE. (default = REPLACE) |
| PKIFNAME  | The name of the PKI internal datafile to use. (default = $CFTPKU) (*available only in command line*)  |


> **Note**
>
> Note: See the PKICER command for more information on certificate parameters and settings.

**Caution**  The PKIENTITY command neither checks nor manages the existing certificates in the internal PKI internal datafile when creating a new certificates list.
