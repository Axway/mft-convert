{
    "title": "Troubleshoot the UCONFRUN dataset",
    "linkTitle": "Troubleshoot the UCONFRUN dataset",
    "weight": "220"
}****CFTMAIN hangs while it waits for the UCONFRUN dataset to be freed****

****Messages****

`IEF861I FOLLOWING RESERVED DATA SET NAMES UNAVAILABLE TO CFTMAIN`

`IEF863I DSN = CFAUTO.AXWAY.D322.CFT.UCONFRUN CFTMAIN RC = 04`

`IEF099I JOB CFTMAIN WAITING FOR DATA SETS`

****Cause****

When CFTMAIN is started, the UCONFRUN dataset is exclusively held by CFTMAIN and Copilot waits for the dataset to be freed.

-or-

If Copilot is started, the UCONFRUN dataset is held exclusively by Copilot.

****Resolution****

This issue is related to the use of at least one of the z/OS storage management tools, such as the IBM Tivoli Allocation Optimizer. The easiest way to disable Tivoli Allocation Optimizer is to add the bypass DDNAME in the JCL (COPRUN and CFTMAIN). Doing so bypasses the Tivoli Allocation Optimizer for all of the datasets accessed by this step.

The ddname is AOBYPASS and the format of the DD card is: `//AOBYPASS DD DUMMY`

****Example****

```
//\* ---------------------------------------------------
//\* Turning off Fault Analyzer with a JCL switch (IDIOFF)
//\* and/or other debuging products.
//\* uncomment the following statement(s).
//\* ---------------------------------------------------
//\*IDIOFF DD DUMMY IBM FAULT ANALYZER OFF
//\*
//\*ABNLIGNR DD DUMMY ABEND-AID OFF
//\*ESPYIBM DD DUMMY EYE-SPY OFF
//\*CAOESTOP DD DUMMY CA-OPT II & CA-SYMDUMP OFF
//\*DMBENAN DD DUMMY TURN OFF DUMPMASTER
//\*PSPOFF DD DUMMY TURN OFF SOFTWORKS PERFORMANCE ESSENTIAL
//\*
//\* ---------------------------------------------------
//\* Turn off PDSMAN
//\* uncomment the following statement.
//\* ---------------------------------------------------
//\*FCOPYOFF DD DUMMY
//\*
//\*PROIGN DD DUMMY To bypass Stop-X37
//\*AOBYPASS DD DUMMY To bypass Tivoli Allocation Optimizer
//\*
```
