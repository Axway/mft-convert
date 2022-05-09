{
    "title": "Troubleshooting",
    "linkTitle": "Troubleshooting",
    "weight": "210"
}Using the support tool
----------------------

To assist Axway Customer Support, the CFTSUPPORT command collects useful information from a Transfer CFT environment including the configuration, Unified Configuration parameters (UCONF), catalog information, log files, and so on. This information is then packaged and stored in a tar file in the specified IFS folder.

Collected information for the Transfer CFT {{< TransferCFT/PrimaryForOS400  >}} platform includes these different CFTPROD/SUPOUT file members:


| File  | Comment  |
| --- | --- |
| ABOUT  | About information  |
| ALOG  | Cftlog file  |
| CFTOUTQ  | List the Transfer CFT Out Queue messages  |
| CFTEXT  | CFT parameter extract  |
| CFTOUT  | CFT jobs outputs  |
| COPJLOG.CSV  | Primary Copilot QPJOBLOGs  |
| COPJLOG2.CSV  | Secondary Copilot QPJOBLOGs  |
| COPOUT  | Copilot jobs outputs  |
| COPTRC  | Copilot traces  |
| JLOG.CSV  | Primary CFT QPJOBLOGs  |
| JLOG2.CSV  | Secondary CFT QPJOBLOGs  |
| LISTCAT  | Brief listcat  |
| LISTCATD  | Debug listcat  |
| LISTCATF  | Full listcat  |
| LISTCOM  | listcom  |
| LISTNODE  | Listnode  |
| LISTPKI  | Listpki output  |
| LISTPKID  | Listpki debug output  |
| LISTPKIF  | Listpki full output  |
| LISTUCONF  | Listuconf output  |
| LOG  | Cftlog file  |
| SAVFOUTQ.bin  | Back up the Transfer CFT Out Queue (in *SAVF format)  |
| WRKACTJOB  | List system activity  |


Using the CFTSUPPORT command
----------------------------

The CFTSUPPORT command executes the CFTSUPPORT program, which retrieves information about the Transfer CFT and stores it in a tar file.

> **Note**
>
> Note: CFTSUPPORT is currently not supported with an independent ASP (IASP).

You can use Transfer CFT IBM i command line to execute the command:

1. Enter the CFTSUPPORT command and press PF4.
1. Enter the IFS path where the CFTSUPPORT.tar file should be created. If the IFS path does not exist it will be created.

> **Note**
>
> Note: Alternatively, from the CFT menu select 3. Administration commands then 2. Submit CFT support request.

If the generated CFTSUPPORT.tar is too large, you can compress it prior to sending it to Axway support.

**Example**

In the following example, the command creates the CFTSUPPORT.tar and SAVFOUTQ.bin files in `/home/cft/cftsupport/`.

```
CFTSUPPORT IFSPATH('/home/cft/cftsupport')
```
