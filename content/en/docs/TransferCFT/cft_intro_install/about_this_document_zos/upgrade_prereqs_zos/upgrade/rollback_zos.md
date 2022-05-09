{
    "title": "Rollback upgrade and restore the catalog",
    "linkTitle": "Rollback upgrade and restore catalog",
    "weight": "260"
}Prerequisites
-------------

- You must have made a backup of the environment prior to the upgrade you are rolling back.
- Upgraded from {{< TransferCFT/headerfootervariableshflongproductname  >}} 3.4 or higher to {{< TransferCFT/headerfootervariableshflongproductname  >}} {{< TransferCFT/axwayvariablesReleaseNumber  >}}

Procedure
---------

1. Export all catalogs. Base your export procedure on the following example:  
    ```
    // EXPORT SYMLIST=(CFTCAT)
    // SET CFTCAT=current.CFT.CATALOG (to be customized)
    // SET TMPCAT=CFTWRK.TMPCAT (to be customized)
    //\* -------------------
    //\* Export catalog
    //\* -------------------
    //EXPCAT EXEC PGM=CFTMI24B,REGION=32M
    //STEPLIB DD DISP=SHR,DSN=current.CFT.LOAD
    //TMPCAT DD DISP=(,CATLG),
    // DSN=&TMPCAT
    // DCB=(LRECL=4120,RECFM=VB,BLKSIZE=27998),
    // SPACE=(CYL,(50,10),RLSE) (to be customized)
    //CEEDUMP DD SYSOUT=\*
    //CFTOUT DD SYSOUT=\*
    //CFTIN DD DATA,SYMBOLS=(JCLONLY)
    MIGR TYPE=CAT,DIRECT=FROMCAT,OFNAME=DD:TMPCAT,
    IFNAME='&CFTCAT'
    /\*
    ```
1. Perform a version rollback as described in [Upgrade version rollback](#Upgrade) below.
1. Recreate the catalogs and re-import the catalogs. Base your procedure on the following example:  
    ```
    // EXPORT SYMLIST=(CFTCAT)
    // SET CFTCAT=current.CFT.CATALOG (to be customized)
    // SET TMPCAT=CFTWRK.TMPCAT (to be customized)
    //\* -----------------
    //\* Import catalog
    //\* -----------------
    //IMPCAT EXEC PGM=CFTMI24B,REGION=32M
    //STEPLIB DD DISP=SHR,DSN=&LOAD
    //TMPCAT DD DISP=SHR,DSN=&TMPCAT
    //CEEDUMP DD SYSOUT=\*
    //CFTOUT DD SYSOUT=\*
    //CFTIN DD DATA,SYMBOLS=(JCLONLY)
    MIGR TYPE=CAT,DIRECT=TOCAT,IFNAME=DD:TMPCAT,
    OFNAME='&CFTCAT'
    /\*
    ```

<span id="Upgrade"></span>

Upgrade version rollback
------------------------

The following procedure enables you to rollback to the previous state if you have applied a SP or patch. This is useful if there is an incident during the application of a PTF, or when validating a patch.

1. Use the A13RBACK
1. Use the A13RBACK to restore the executable file library, USS Copilot and USS Secure Relay environment. This JCL executes the following three JCLs:
    -   A13RSTOR: Restores the Transfer CFT Load library.
    -   A13UCOPR: Restores the Transfer CFT USS Copilot environment.
    -   A13UXSRR: Restores the Transfer CFT USS Secure Relay environment.
