{
    "title": "Application integration",
    "linkTitle": "Application integration",
    "weight": "140"
}This section provides information on using the following services and features in {{< TransferCFT/axwayvariablesComponentShortName  >}}:

[About the CFTUTIL interface](../c_intro_userinterfaces/about_cftutil) - Describes the CFTUTIL command line utility used in Transfer CFT.

[Folder monitoring](intro_folder_monitor/folder_monitor_uconf) - Describes how to set up folder monitoring for Transfer CFT directories. When activated in Transfer CFT, folder monitoring periodically checks the status of files contained in a defined set of directories to see if there are transfer candidates.

[Using a shared disk as the data transfer medium](copy_a_file) - Describes how to copy a file to a different location without actually sending the file, while still retaining standard Transfer CFT tracking.

[Using synchronous communication](synch_comm_tcpip_intro) - Synchronous communication provides a real-time response when sending data from a client to the Transfer CFT server. This response indicates that the client command was acknowledged by Transfer CFT and is listed in the catalog.

[Using APIs](../cft_intro_install/about_this_document_zos/using_apis) - Application Programming Interfaces are a set of functions
that create application services.
Each of these services is described in the following sections:

- REST APIs
- Web services
- JPI
- Transfer services
- Synchronous communication
- Catalog querying services
- Delivered API templates described per operating system
- Services in C
- Services in COBOL

[Managing exits](managing_exits)
- Provides the information about exit
task concepts, exit
task architecture, types of exits and how to implement.

[Force heterogeneous mode for a group of files](../admin_intro/uconf/uconf_heterogeneous_mode) - Describes how to disable the default settings in case you want to ensure that groups of files are transferred using only the heterogeneous mode.

[Script execution scheduling](../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftcron) - Describes the CRONJOB feature, which allows Transfer CFT to execute jobs at predetermined
dates and times. These jobs can perform functions such as periodically
scan one or more directories and issue a SEND command.

Defining maximum simultaneous transfers - Describes how to optimize the number of parallel transfers, the number of transfer sessions, and how parameter dependencies affect transfer rates.

[Command Guide](../c_intro_userinterfaces/command_summary/typographical_conventions) - This section provides a comprehensive listing of {{< TransferCFT/axwayvariablesComponentShortName  >}} commands,
typographical conventions, command syntax and parameters.
