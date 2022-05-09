{
    "title": "Managing multi-node  ",
    "linkTitle": "Managing multi-node",
    "weight": "280"
}This topic describes how to set up and manage your multi-node environment. It describes:

Starting the Transfer CFT cluster
---------------------------------

### Start all node managers

For each host perform the command: `copsmng`

### Start all nodes

On one of the hosts in the cluster, perform the command: `cft start`

Stopping the Transfer CFT cluster
---------------------------------

### Stop all nodes

On one of the hosts in the cluster, perform the command:` cft stop`

### Stop all node managers

For each host, perform the command: `copstop`

Add a node to the Transfer CFT cluster
--------------------------------------

In this example the Transfer CFT cluster accounts at the beginning two nodes, node 0 and node 1.

### Add a node

Execute the following command to add a new node: `cft add_node`

The node 2 is created. The cluster is composed of three nodes: node 0, node 1 and node 2. All associated files associated with node 2 are initialized and its node state is set to DISABLED.

> **Note**
>
> Note: When adding a node, you must add the corresponding new license key for that node in the license-key file &lt;CFTDIRRUNTIME&gt;/conf/cft.key:one_key_per_line.

### Enable a node

Once the new node has been added, you can now enable it using the command: `cft enable_node -n 2`

### Start a node

The node 2 can be started using the command:` cft start -n 2`

Removing a node from the Transfer CFT cluster
---------------------------------------------

> **Note**
>
> Note: Only the last node can be removed.

### Disable the last node

The last node must be fenced before removing a node. To fence a node before removing it, enter: `cft disable_node –n <node_id>`

The node runs as long as its catalog is not empty. Once the catalog is empty, the node state is set to DISABLED and the node stops automatically.

### Remove the last node

After fencing and stopping the *last node*, you can remove it. Enter:` cft remove_node –n <node_id>`

Rebalancing after a fail over
-----------------------------

Once the failed node manager is running again, you can rebalance the cluster by re-starting one or multiple nodes.

In this example there are two hosts (host00 and host01), and two nodes (node00 and node01). Node00 is running on host00, and node01 is running on host01.

1. Host00 and node00 experience a fail over.
1. The host01 node manager re-starts the node00 locally.
1. Node00 and node01 run on host01.
1. Host00 and its node manager are manually re-started.
1. From one of the Transfer CFT cluster hosts, host00 or host01, execute the command: `cft restart –n 0`
1. The host00 node manager restarts the node00 locally.

****Step results****: Node00 is running on host00, and node01 running on host01.
