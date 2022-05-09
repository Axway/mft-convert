{
    "title": "Transfer CFT configuration",
    "linkTitle": "Transfer CFT configuration",
    "weight": "260"
}This section describes the specific features of the {{< TransferCFT/axwayvariablesComponentShortName  >}} OpenVMS configuration, and the reserved characters and configuration commands.

Some configuration commands include specific features that enable {{< TransferCFT/axwayvariablesComponentShortName  >}} to be more effectively integrated into a OpenVMS environment. These features affect certain reserved characters, the network configurations, and the definition of the files to be transferred.

CFTUTIL, Copilot, and API default values
----------------------------------------

The following table lists the default values that are used for {{< TransferCFT/axwayvariablesComponentShortName  >}} files.


| Object  | Specific value  |
| --- | --- |
| Parameter file | CFTPARM |
| Partner file | CFTPART |
| Catalog file | CFTCATA |
| Log file | CFTLOG |
| Communication file | CFTCOM |
| Statistics file | CFTACCNT |
| Alternate statistics file | CFTACCNTA |
| Default media | File |


Reserved characters for {{< TransferCFT/axwayvariablesComponentShortName  >}} configuration
------------------------------------------------------------------------------------------------

Some characters have a unique meaning in commands and {{< TransferCFT/axwayvariablesComponentShortName  >}} configuration.


| Notation  | Object  | Specific Value  |
| --- | --- | --- |
| char_file | Logical name prefix | No specific character, as the logical names are processed transparently by RMS |
| char_mask | Wildcard character | % |
| char_unit | Separator character (volume) | No volume concept on OpenVMS |
| char_symb | Symbolic variable prefix | &amp; |
| file_symb | Character introducing a file name passed to CFTUTIL as a parameter | Either # or @ |

