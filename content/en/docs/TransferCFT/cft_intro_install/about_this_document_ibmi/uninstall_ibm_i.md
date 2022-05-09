{
    "title": "Uninstall Transfer CFT",
    "linkTitle": "Uninstall Transfer CFT",
    "weight": "200"
}To uninstall {{< TransferCFT/axwayvariablesComponentLongName  >}} in an IBM i environment, delete the following libraries:

- CFTPROD
- CFTPGM

In the HFS partition remove:

- `/home/cft/cft332/install `
- `/home/cft/cft332/runtime `

> **Note**
>
> Note: The user performing the uninstall must have delete rights for the CFTPGM and CFTPROD libraries, and the files in IFS /home/user/cft3x.
