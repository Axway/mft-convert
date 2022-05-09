{
    "title": "Working in a multi-node environment",
    "linkTitle": "Multi-node architecture",
    "weight": "240"
}This section describes how to enable multi-node functioning in your Transfer CFT environment.

1. Use the unified configuration parameter to enable the multi-node feature. To enable, set value to `yes`.
1. To add a host, enter the command:

    `$CFT add_host -hostname <hostname> -host <host_address>`

1. Activate the Transfer CFT node that you will use.
1. Start the node that you activated above in Step 3.
1. To start the Copilot server, enter:

    `$COPSTART`

1. You can use the ****CFT.COM**** script to manage your Transfer CFT environment, as described in the following table.

```
Command                   Actions
  start                    Start all nodes.
  start -n <node_id>       Start a specific node.
  stop                    Stop all nodes.
stop -n <node_id> Stop a specific node.
  stop -ln                 Stop only the local node.
restart                    Restart all nodes.
restart -n <node_id>       Restart a specific node.
restart -ln                Restart only the local nodes.
add_node                   Add a new node.
remove_node -n <node_id>   Remove a node.
add_host -hostname <hostname> Add a new host
as: -host <host_address>.
enable_node -n <node_id>   Enable a specific node.
disable_node -n <node_id>   Disable a specific node.
```

****Example****

```
For the following example host:
  $                    CFT add_host -hostname ia64 -host 127.0.0.1
To activate and start a node pass the commands:
  $CFT enable_node -n 0
  $CFT start_node -n 0
To add ONE node, use the command:
  $CFT add_node
To disable and stop a node use the commands:
  $CFT stop_node -n 0
  $CFT disable _node -n 0
After stopping all nodes, you can stop COPILOT. Enter:
  $COPSTOP
```
