{
    "title": "Connecting to the Transfer CFT UI  - UNIX",
    "linkTitle": "Defining user rights ",
    "weight": "240"
}Before you can start {{< TransferCFT/axwayvariablesComponentShortName  >}} from the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI server, the Copilot server must be started. Additionally, you require rights to log on to this server.

<span id="Define rights before logging on the CFT Navigator server"></span>

Define rights before logging on the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI server
--------------------------------------------------------------------------------------------------------


| copilot.misc.createprocessasuser<br/> value = | With PassPort AM | Without PassPort AM  |
| --- | --- | --- |
| **yes**<br/> Root privileges are required for CFTSU. | With PassPort AM<br/> Check user method: operating system login<br/> Create Process: Unix User In this case, the system user must exist in PassPort AM for security rights. |   |
| **yes**<br/> Root privileges are required for CFTSU. |   | No PassPort AM<br/> Check user method: operating system login<br/> Create Process: Unix user |
| **no** (Unix default)  | With PassPort AM<br/> Check user method: AM login<br/> Create Process: {{< TransferCFT/axwayvariablesComponentShortName  >}} UI user (Copilot)<br/> In this case, users must be defined in PassPort AM for login and security rights. |   |
| **no** (Unix default)  |   | No PassPort AM<br/> Users must be created in the local {{< TransferCFT/axwayvariablesComponentShortName  >}} internal datafile (xfbadmusr, see [xfbadmusr utilitiy](../../use_cft_utilities#xfbadmusr1)) |


 

Related topics

[About PassPort AM](../../../../../internal_a_m_start_here/about_passport_am)

[UCONF parameters](../../../../../admin_intro/uconf/uconf_parameters)
