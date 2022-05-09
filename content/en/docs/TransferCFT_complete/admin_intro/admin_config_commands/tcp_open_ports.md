{
    "title": "TCP port availability ",
    "linkTitle": "TCP port availability",
    "weight": "290"
}This page lists the various {{< TransferCFT/axwayvariablesComponentLongName  >}} ports used for communication between processes, for both single and multi-node installations, which can be used for health checks or transfer-related operations.

Single node deployment
----------------------


| Usage  | Process  | Configurable  | Configuration Parameter  | Network Interface  |
| --- | --- | --- | --- | --- |
| Listening port for file transfer protocol  | CFTTCPS  | Yes  | CFTPROT:SAP  | See CFTNET:HOST  |
| Listening port for Synchronous Communication Media  | CFTTCOMS  | Yes  | CFTCOM:PORT  | See CFTCOM:HOST  |
| Inter-process communication  | CFTPRX  | No  | N/A: port provided by the operating system  | 127.0.0.1  |
| Copilot inter-process communication  | copsmng  | No  | N/A: port provided by the operating system  | 127.0.0.1  |
| Copilot Web server  | copui  | Yes  | copilot.general.serverport  | copilot.general.serverhost  |
| Copilot Web server for Central Governance / Flow Manager  | copui  | Yes  | copilot.general.ssl_serverport  | copilot.general.serverhost  |
| Copilot Web server inter-process communication  | copsproc  | No  | N/A: port provided by the operating system  | 127.0.0.1  |
| Copilot REST API Web server  | coprests  | Yes  | copilot.restapi.serverport  | copilot.general.serverhost  |
| Secure Relay Master Agent  | masteragent.jar  | Yes  | secure_relay.ma.comm_port  | secure_relay.ma.host  |
| Secure Relay heartbeat service  | CFTPRX  | Yes  | secure_relay.ma.heartbeat_service.port  | secure_relay.ma.heartbeat_service.host  |


Multi-node deployment
---------------------


| Usage  | Process  | Configurable  | Configuration Parameter  | Network Interface  |
| --- | --- | --- | --- | --- |
| Listening port for Synchronous Communication Media  | copcoms  | Yes  | CFTCOM:PORT  | See CFTCOM:HOST  |
| Listening port for Synchronous Communication Media  | CFTTCOMS  | Yes  | cft.multi_node.listen_port_range  | 0.0.0.0  |
| Inter-node communication for transfer recovery  | CFTPRX  | Yes  | cft.multi_node.listen_port_range  | 0.0.0.0  |
| Copilot inter-process communication  | copsmng  | No  | N/A: port provided by the operating system  | 127.0.0.1  |
| Copilot Web server  | copui  | Yes  | copilot.general.serverport  | copilot.general.serverhost  |
| Copilot Web server for Central Governance / Flow Manager  | copui  | Yes  | copilot.general.ssl_serverport  | copilot.general.serverhost  |
| Copilot Web server inter-process communication  | copsproc  | No  | N/A: port provided by the operating system  | 127.0.0.1  |
| Copilot REST API Web server  | coprests  | Yes  | copilot.restapi.serverport  | copilot.general.serverhost  |
| Node manager inter-process communication  | copnman  | Yes  | cft.multi_node.listen_port_range  | 0.0.0.0  |
| Connection Dispatcher local port (Windows, z/OS only)  | copcod  | Yes  | copilot.connection_dispatcher.local_port  | 127.0.0.1  |
| Secure Relay Master Agent  | masteragent.jar  | Yes  | secure_relay.ma.comm_port + node_number  | secure_relay.ma.host  |
| Secure Relay heartbeat service  | CFTPRX  | Yes  | secure_relay.ma.heartbeat_service.port  | secure_relay.ma.heartbeat_service.host  |

