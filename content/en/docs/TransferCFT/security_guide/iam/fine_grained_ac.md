---

    title: Fine-grain access control resources 
    linkTitle: Fine grained access control resources
    weight: 90

---
Transfer CFT supports fine-grained access control (FGAC) of objects by users. Objects subject to FGAC are IDs in the database, filename patterns, and object parameter filters. For example, you can specify that a user can view or change only certain organizations. You also can prohibit a user from viewing or changing organizations. Moreover, limitations can be enforced for multiple users through the use of access groups.

If there is a conflict between FGAC privileges based on the same resources, PassPort enforces the privilege with the least restrictions.

Grayed options indicate that the associated functionality in the product is deprecated or obsolete.

FGAC privileges


| Resource  | Description  | Resource properties  | Resource actions  |
| --- | --- | --- | --- |
| AM:RIGHTS | Rights from passport AM | ID | VIEW_SELF, VIEW_OTHERS |
| CONFIGURATION:PKICER | Manage authority certificates in the local database | ID | CREATE, DELETE, EDIT, VIEW, ACTIVATE, DEACTIVATE |
| CONFIGURATION:PKIKEY  | Manage the key for SFTP protocol  | ID  | CREATE, DELETE, EDIT, VIEW, ACTIVATE, DEACTIVATE  |
| CONFIGURATION:PKIENTITY  | Manage entities, which are a group of root authorities  | ID  | CREATE, DELETE, EDIT, VIEW, ACTIVATE, DEACTIVATE  |
| CONFIGURATION:CFTPARM | Manage global Transfer CFT operations parameter objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTNET | Manage local network resources | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTPROT | Manage file transfer protocol for partner exchanges | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTSEND | Manage SEND transfer characteristics and values | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTSENDI | Manage implicit SEND object definitions | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTRECV | Manage RECV transfer characteristics and values | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTAUTH | Manage authorized file identifiers that define access authorization for partners | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTXLATE | Manage translation between two defined tables | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTLOG | Manage transfer log file object | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTCAT | Manage the catalog file object containing transfer associated data | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTCOM | Manage the Transfer CFT communication media | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTACCNT | Manage the recording mode for statistical data objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTEXIT | Manage the environment for an exit task objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTIDF | Manage model network file identifier objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTFOLDER | Manage folder objects | ID | CREATE, DELETE, VIEW, EDIT, ACTIVATE, DEACTIVATE |
| CONFIGURATION:CFTETB | Manage ETEBAC parameter card objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTAPPL | Manage local application objects for transfer owners | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTSSL | Manage transport security profile objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTCRON | Schedule job executions | ID | CREATE, DELETE, VIEW, EDIT, ACTIVATE, DEACTIVATE, RELOAD |
| CONFIGURATION:CFTPART | Configure Transfer CFT partner parameter objects | ID | CREATE, DELETE, VIEW, EDIT, TURN, ACTIVATE, DEACTIVATE |
| CONFIGURATION:CFTDEST | Manage broadcast list objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTTCP | Manage TCP network file identifiers objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTX25 | Manage X25 network file identifiers objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTSNA | Manage SNA network access resource objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTLU62 | Manage LU62 access method objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTDSA | Manage DSA protocol objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTDNA | Manage DNA protocol network objects | ID | CREATE, DELETE, VIEW, EDIT |
| CONFIGURATION:CFTUCONF | Access the UCONF configuration interface | ID | DELETE, VIEW, EDIT |
| SERVICE:BATCH | Manage batch files | ID, FNAME | EXECUTE |
| SERVICE:UI | User interface | TYPE, ID, GROUP | CONNECT |
| TRANSFER | Manage transfers | IDAPPL, ID, PART, SPART, RPART, IPART, TYPE, DIRECT, MODE, FNAME, MESSAGE, SUSER, RUSER, SAPPL, RAPPL, NFNAME | CREATE, DELETE, VIEW, EDIT, CANCEL, RESUME, PAUSE, EXECUTE, SUBMIT, END, VIEWFILE, EDITFILE, DELETEFILE |
| FILTER:CATALOG | Manage catalog filter objects | ID | CREATE, EDIT, VIEW, DELETE |
| FILTER:LOG | Manage log filter objects | ID | CREATE, EDIT, VIEW, DELETE |
| FILE | Manage Transfer CFT file definition objects | FNAME | CREATE, EDIT, VIEW, DELETE |
| URL | Manage file name definitions | URL | VIEW |


**Note for SERVICE:UI** If you set a FGAC privilege on the SERVICE:UI resource for a user, this user can only connect to Transfer CFTs 3.3.2 and higher that match the privilege. Meaning that this user cannot log on Transfer CFT 3.2.4 and lower.
