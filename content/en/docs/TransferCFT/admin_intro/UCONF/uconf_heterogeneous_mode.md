{
    "title": "Force heterogeneous mode for a group of files",
    "linkTitle": "Forcing heterogeneous mode",
    "weight": "310"
}In {{< TransferCFT/axwayvariablesComponentShortName  >}} both homogeneous and heterogeneous mode are enabled by default. However, you may want to ensure that groups of files are transferred using only the heterogeneous mode. The UCONF configuration parameter` cft.server.force_heterogeneous_mode` allows you to do this, effectively disabling homogeneous mode even if the partner is configured for homogeneous exchanges.

For more information on sending groups of files and heterogeneous mode exchanges, see [Sending a group of files](../../../concepts/send_command/send_group_of_files_cl).

To force heterogeneous mode:

1. Access the unified configuration utility using either [command line](../uconf_w_cftutil) or the [GUI](../uconf_userinterface).
1. Set the following parameter to enable forced heterogeneous exchanges for group file transfers.

********Unix/Windows********


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.server.force_heterogeneous_mode  | No  | Force heterogeneous mode for group file transfers. This parameter replaces the deprecated environment variable: CFTSFMCPY.<br/> Possible values:<br/> • Yes: Force heterogeneous mode exchanges (override homogeneous mode)<br/> • No: Standard heterogeneous and homogeneous functioning |


****Related topics****

- [Sending a group of files](../../../concepts/send_command/send_group_of_files_cl)
- Environmental variables
- [File management functions (Windows)](../../../cft_intro_install/windows_install_start_here/windows_install_start_here/specific_system_functions/file_management_functions)
