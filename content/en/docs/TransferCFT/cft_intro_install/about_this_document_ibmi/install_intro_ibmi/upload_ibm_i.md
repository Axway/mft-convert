{
    "title": "Upload and restore the installation files",
    "linkTitle": "Upload and restore installation files",
    "weight": "180"
}This section describes the upload and restore options available for Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}} for the IBM i platform to perform prior to installation, using FTP and the RSTLIB command.

Before starting a Transfer CFT session, you must add the value \*none in the initial program call to call a screen menu directly. Otherwise, the session cannot start.

> **Note**
>
> Note: CFTPGM is the standard name for the programs library.

Begin the installation process by uploading the Transfer CFT installation package, in binary mode, to the IBM i system:

1. Log in with the `CFTINST `user.
1. Create a temporary library:  
    ```
    CRTLIB CFTTMP
    ```
1. Create a SAVF file:  
    ```
    CRTSAVF FILE(CFTTMP/CFT39)
    ```
1. Upload the installation package to the SAVF in binary mode using FTP:  
    ```
    binary
    cd CFTTMP
    put Transfer_CFT_os400.bin CFT39
    ```

<!-- -->

1. Restore the SAVF file:  
    ```
    RSTLIB SAVLIB(CFTPG) DEV(\*SAVF) SAVF(CFTTMP/CFT39) OPTION(\*NEW) RSTLIB(CFTTMP)
    ```
