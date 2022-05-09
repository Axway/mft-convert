{
    "title": "Troubleshoot active passive file systems",
    "linkTitle": "Troubleshoot NFS option",
    "weight": "230"
}****Problem****

Deploying flows from {{< TransferCFT/suitevariablesCentralGovernanceName  >}} to {{< TransferCFT/suitevariablesTransferCFTName  >}} in an active/passive cluster fails with "Read timed out" error even though there are no connectivity issues.

****Error in the opnode.log file:****

`ERROR Error while setting the configuration for product Transfer CFT - CIB_37810_GCS_GL : An error occurred while deploying the configuration: Could not parse webservices response from Transfer CFT. Problem: Read timed out`

`com.axway.cmp.admin.api.v2.ProductException: Error while setting the configuration for product Transfer CFT - CIB_37810_GCS_GL : An error occurred while deploying the configuration: Could not parse webservices response from Transfer CFT. Problem: Read timed out`

****Resolution****

The {{< TransferCFT/suitevariablesTransferCFTName  >}} application got stuck when trying to lock the runtime/data/cftparm as the NFS server thinks the file is locked by another application.

To remedy, you can specify the mount `nolock` option. {{< TransferCFT/suitevariablesTransferCFTName  >}} continues to lock files, however, the lock requests are not sent to the NFS server. This avoids issues when switching from the active to the passive node.
