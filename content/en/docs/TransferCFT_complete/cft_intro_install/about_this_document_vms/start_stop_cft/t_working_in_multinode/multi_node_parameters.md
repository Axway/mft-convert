{
    "title": "Multi-node unified configuration parameter",
    "linkTitle": "Multi-node unified configuration parameter",
    "weight": "290"
}This topic presents the multi-node uconf parameters and their default values. The column ****Modify**** indicates a strong recommendation that you should not modify this value if ****No**** is indicated.


| Parameters | Description | Default value | Values | Modify  |
| --- | --- | --- | --- | --- |
| cft.multi_node.enable | Enable/disable the multi-node feature | No | Yes, No | Yes  |
| cft.multi_node.max | Maximum number of nodes | 8 | integer from 0 to 8 | No  |
| cft.multi_node.cftcomlock.fname | Lock file for the main communication media file task in multi-node | $(cft.runtime_dir)/data/cftcom.lck | fname | Yes  |
| cft.multi_node.cftcom.dispatcher_policy  | Specify the dispatching policy<br/> -round_robin: Random dispatching across all nodes occurs.<br/> -node_affinity: Creates a one to one link between a partner and a node. Transfer requests for a given partner will always be performed by the same node. | round_robin  | round_robin, node_affinity  | Yes  |
| cft.multi_node.sharedidt.fname | Shared file for global IDT calculation in multi-node | $(cft.runtime_dir)/data/cftsidt | fname | Yes  |
| cft.multi_node.sharedidt.enable | Use global IDT calculation method | No | Yes, No | Yes  |
| cft.multi_node.shared.filesystem.type | Used to select appropriate consistency enforcement strategy.<br/> If Transfer CFT is using NFSv4, you must enter the value nfs in lower case. | unknown | unknown, posix, nfs, cifs | Yes  |
| cft.multi_node.transfer_recovery_timeout | Timeout in seconds for transfer recovery process (seconds) | 30 | integer | Yes  |
| cft.multi_node.transfer_recovery_retry_delay | Delay in seconds for transfer recovery retry (seconds) | 20 | integer | Yes  |
| cft.multi_node.connection_retry_delay | Delay in seconds for connection retry between nodes (seconds) | 10 | integer | Yes  |
| cft.multi_node.hostnames | List of hosts which handle the multi-node architecture |   | list | No  |
| cft.multi_node.hostnames.&lt;hostname&gt;.host | Address (FQDN or IP address) of the host |   | string | Yes  |
| cft.multi_node.hostnames.&lt;hostname&gt;.pid | Process ID of Copilot (copsmng) in multi-node |   |   | No  |
| cft.multi_node.hostnames.&lt;hostname&gt;.state | Copilot (copsmng) status in multi-node | STOPPED | INITIALIZING, STARTING, RUNNING, STOPPING, STOPPED, ERROR | No  |
| cft.multi_node.hostnames.&lt;hostname&gt;.copui_pid | Process ID of UI server (copui) in multi-node |   |   | No  |
| cft.multi_node.hostnames.&lt;hostname&gt;.copui_client_socket | Windows socket passing for UI server (copui) in multi-node |   | integer | No  |
| cft.multi_node.hostnames.&lt;hostname&gt;.copui_notification_port | Notification port for UI server (copui) in multi-node |   | integer | No  |
| cft.multi_node.nodes | Number of nodes | 2 | integer from 2 to $(cft.multi_node.max) | No  |
| cft.multi_node.nodes.&lt;node_id&gt;.nodestate | Node status | DISABLED | DISABLED, ENABLED_STOPPED, ENABLED_STARTED | No  |
| cft.multi_node.nodes.&lt;node_id&gt;.state | Transfer CFT status | STOPPED | INITIALIZING, STARTING, RUNNING, STOPPING, STOPPED, ERROR | No  |
| cft.multi_node.nodes.&lt;node_id&gt;.pid | CFTMAIN process ID |   | integer | No  |
| cft.multi_node.nodes.&lt;node_id&gt;.hostname | Hostname of the server where the node is running on |   | string | No  |
| cft.multi_node.nodes.&lt;node_id&gt;.host | Host address of the server where the node is running on. |   | string | No  |
| cft.multi_node.nodes.&lt;node_id&gt;.prx_port | Internal node listening port |   | integer | No  |
| cft.multi_node.nodes.&lt;node_id&gt;.disabling | Flag set when Transfer CFT is disabling | No | Yes, No | No  |


Connection dispatcher parameters
--------------------------------


| Parameters | Description | Default value | Values | Modify  |
| --- | --- | --- | --- | --- |
| copilot.connection_dispatcher.enable | Enable client registering connection dispatching | Yes (multi-node), No otherwise | Yes, No | Yes  |
| copilot.connection_dispatcher.control.port | Connection dispatcher TCP control port |   | integer | Yes  |
| copilot.connection_dispatcher.retry_delay | Client Registering to connection dispatcher retry delay in seconds | 5 | integer | Yes  |
| copilot.connection_dispatcher.retry_timeout | Client Registering to connection dispatcher timeout in seconds | 300 | integer | Yes  |
| copilot.connection_dispatcher.unix.af_unix_fname | Unix Socket path name for connection dispatching | $(cft.runtime_dir)/run/S_$(cft.short_hostname)DISPATCH | fname | Yes  |
| copilot.connection_dispatcher.nt.local_port | Local TCP port for connection dispatching | 1800 | integer | Yes  |


### Node manager parameters


| Parameters | Description | Default value | Values | Modify  |
| --- | --- | --- | --- | --- |
| copilot.node_manager.watchperiod | Interval between checking the status of two Transfer CFT nodes | 10 | integer | Yes  |


**** ****
