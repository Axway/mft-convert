{
    "title": "Configuration parameters",
    "linkTitle": "Configuration parameters",
    "weight": "190"
}This section presents the **multi-node** parameters and their default values. The column ****Modify**** indicates a strong recommendation that you should not modify this value if ****No**** is indicated.


| Parameters | Description | Default | Values | Modify  |
| --- | --- | --- | --- | --- |
| cft.multi_node.enable | Enables/disables the multi-node feature. | No | Yes, No | Yes  |
| cft.multi_node.max | Maximum number of nodes. | 8 | integer from 0 to 8 | No  |
| cft.multi_node.<br /> cftcomlock.fname | Lock file for the main communication media file task in multi-node | $(cft.runtime_dir)/data/cftcom.lck | fname | Yes  |
| cft.multi_node.<br /> cftcom.dispatcher_policy  | Specifies the dispatching policy.<br/> - round_robin: Random dispatching across all nodes occurs.<br/> - node_affinity: Creates a one to one link between a partner and a node. Transfer requests for a given partner will always be performed by the same node. | round_robin  | round_robin,<br/> node_affinity | Yes  |
| cft.multi_node.<br /> sharedidt.fname | Shared file for global IDT calculation in multi-node | $(cft.runtime_dir)/data/cftsidt | fname | Yes  |
| cft.multi_node.<br /> sharedidt.enable | Use global IDT calculation method | No | Yes, No | Yes  |
| cft.multi_node.<br /> shared.filesystem.type | Used to select appropriate consistency enforcement strategy.<br/> If {{< TransferCFT/axwayvariablesComponentShortName  >}} is using NFSv4, you must enter the value <code>nfs </code>in lower case. | unknown | unknown, posix, nfs, cifs | Yes  |
| cft.multi_node.<br /> transfer_recovery_timeout | Timeout in seconds for transfer recovery process (seconds) | 30 | integer | Yes  |
| cft.multi_node.<br /> transfer_recovery_retry_delay | Delay in seconds for transfer recovery retry (seconds) | 20 | integer | Yes  |
| cft.multi_node.<br /> connection_retry_delay | Delay in seconds for connection retry between nodes (seconds) | 10 | integer | Yes  |
| cft.multi_node.<br /> hostnames | List of hosts which handle the multi-node architecture |   | list | No  |
| cft.multi_node.<br /> hostnames.&lt;hostname&gt;.host | Address (FQDN or IP address) of the host |   | string | Yes  |
| cft.multi_node.hostnames.<br /> &lt;hostname&gt;.pid | Process ID of Copilot (copsmng) in multi-node |   |   | No  |
| cft.multi_node.<br /> hostnames.&lt;hostname&gt;.state | Copilot (copsmng) status in multi-node | STOPPED | INITIALIZING, STARTING, RUNNING, STOPPING, STOPPED, ERROR | No  |
| cft.multi_node.<br /> hostnames.&lt;hostname&gt;.<br /> copui_pid | Process ID of UI server (copui) in multi-node |   |   | No  |
| cft.multi_node.<br /> hostnames.&lt;hostname&gt;.<br /> copui_client_socket | Windows socket passing for UI server (copui) in multi-node |   | integer | No  |
| cft.multi_node.<br /> hostnames.&lt;hostname&gt;.<br /> copui_notification_port | Notification port for UI server (copui) in multi-node |   | integer | No  |
| cft.multi_node.nodes | Number of nodes | 2 | integer from 2 to $(cft.multi_node.max) | No  |
| cft.multi_node.<br /> nodes.&lt;node_id&gt;.nodestate | Node status | DISABLED | DISABLED,<br/> ENABLED_STOPPED,<br/> ENABLED_STARTED | No  |
| cft.multi_node.<br /> nodes.&lt;node_id&gt;.state | {{< TransferCFT/axwayvariablesComponentShortName  >}} status | STOPPED | INITIALIZING,<br/> STARTING, RUNNING, STOPPING, STOPPED,<br/> ERROR | No  |
| cft.multi_node.<br /> nodes.&lt;node_id&gt;.pid | CFTMAIN process ID |   | integer | No  |
| cft.multi_node.<br /> nodes.&lt;node_id&gt;.hostname | Hostname of the server where the node is running on |   | string | No  |
| cft.multi_node.<br /> nodes.&lt;node_id&gt;.host | Host address of the server where the node is running on. |   | string | No  |
| cft.multi_node.<br /> nodes.&lt;node_id&gt;.prx_port | Internal node listening port |   | integer | No  |
| cft.multi_node.<br /> nodes.&lt;node_id&gt;.disabling | Sets flag when disabling {{< TransferCFT/axwayvariablesComponentShortName  >}}. | No | Yes, No | No  |
| cft.multi_node.listen_port_range  | Defines a port range to use for listening points dedicated to inter-node communication in multi-node, where the range is at least 4 x (number of nodes).<br/> If you are using a firewall, you must use a port range that you can customize in your firewall to accept incoming connections. | NA<br/> (system value is used) |   |   |


Connection dispatcher parameters
--------------------------------


| Parameters | Description | Default | Values | Modify  |
| --- | --- | --- | --- | --- |
| copilot.<br /> connection_dispatcher.enable | Enable client registering connection dispatching | Yes (multi-node), No otherwise | Yes, No | Yes  |
| copilot.<br /> connection_dispatcher.control.port | Connection dispatcher TCP control port |   | integer | Yes  |
| copilot.<br /> connection_dispatcher.retry_delay | Client registering to connection dispatcher retry delay in seconds | 5 | integer | Yes  |
| copilot.<br /> connection_dispatcher.retry_timeout | Client registering to connection dispatcher timeout in seconds | 300 | integer | Yes  |
| copilot.<br /> connection_dispatcher.unix.af_unix_fname | Unix socket path name for connection dispatching | $(cft.runtime_dir)/run/S_$<br /> (cft.short_hostname)DISPATCH | fname | Yes  |
| copilot.<br /> connection_dispatcher.nt.local_port | Local TCP port for connection dispatching | 1800 | integer | Yes  |


### Node manager parameters


| Parameters | Description | Default | Values | Modify  |
| --- | --- | --- | --- | --- |
| copilot.<br /> node_manager.watchperiod | Interval between checking the status of two Transfer CFT nodes. The Copilot watchdog uses the double of this value to ensure shared file system lease lock is not met. The value must therefore be less than the NFS lease time. | 10 | integer | Yes  |


****Related topics****

- [Multi-node commands](../multi_node_commands)
- [Managing multi-node]()
- [UCONF: Protocols and networks](../../admin_intro/uconf/uconf_protocols_and_networks)
