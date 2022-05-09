{
    "title": "Submit the JCL ",
    "linkTitle": "Submit the JCL install(XSUPPORA)",
    "weight": "200"
}The JCL XSUPPORA extracts information about the current instance for Axway Support, a very useful JCL. This JCL must be able to run for all Transfer CFT instances (Test, PREPROD, PROD).

Collected information includes:

Index:

- ID=SSILIST - Transfer CFT LOAD _ SSI list
- ID=PATCHLOG - Patchs log
- ID=DSINLOAD - DSINFO Transfer CFT LOAD
- ID=DSINUCNF - DSINFO UCONF file
- ID=DSINUCRU - DSINFO UCONFRUN file
- ID=DSINUPAR - DSINFO UPARM dataset
- ID=DSINPARM - DSINFO CFTPARM dataset
- ID=DSINPART - DSINFO CFTPART dataset
- ID=DSINCAT - DSINFO CATALOGUE dataset
- ID=DSINCOM - DSINFO COM dataset
- ID=DSINPKI - DSINFO PKI base
- ID=DSINLOG1 - DSINFO Log 1 dataset
- ID=DSINLOG2 - DSINFO Log 2 dataset
- ID=DSINACC1 - DSINFO Account 1 dataset
- ID=DSINACC2 - DSINFO Account 2 dataset
- ID=DSINAM - DSINFO AM Persistent cache
- ID=SYSINFO - System and TCPIP information
- ID=LOCALCP - Local code page
- ID=CODEPAGE - Available code page
- ID=A03PARM - Customization parameters
- ID=SGINSTAL - Macro SGINSTAL (A12OPTSP)
- ID=CFTSGIGN - Genere Macro SGINSTAL from LOAD()
- ID=CFTENV - JCL Environment variables
- ID=CNFENV - L.E. Environment variables
- ID=CFTCGREG - Member INSTALL(CFTCGREG)
- ID=USSCOP - List USS Copilot files
- ID=USSDISKC - USS Disk space _ COPILOT
- ID=UCONF - UCONF file
- ID=UCONFRUN - UCONFRUN file
- ID=UCONFDEF - UCONF dictionary file
- ID=LISTNODE - CFTUTIL command listnode
- ID=LISTUCON - CFTUTIL command listuconf scope=user
- ID=LISTPKI - PKI base LIST
- ID=CFTABALL - CFTUTIL command ABOUT KEY=ALL
- ID=CFTABOUT - CFTUTIL command ABOUT KEY=FIRST
- ID=CSDCFTV - CSDCFT version
- ID=CSDCGV - CSDCG version
- ID=CFTEXT - Extract Transfer CFT parameters
- ID=CHECK - Check Transfer CFT parameters
- ID=LISTCOM - Main communication file list
- ID=LISTCAT - Catalog list

You can customize some list or extraction options before running this JOB. For example:

```
SET OLISTCAT='BRIEF' LISTCAT content OPTION
SET OLISTCAT=’DEBUG,IDTU=A0000001’ for a specific transfer
SET OLISTCOM='ACTIVE' LISTCOM content OPTION
SET OCFTEXT='BRIEF' CFTEXT content OPTION
```

Axway support may request additional information, such as:

- The complete JOB(s) execution sysout.
- The CEEDUMP sysout for SYSUDUMP.
- The Transfer CFT logs
