{
    "title": "Manage multi-node  ",
    "linkTitle": "Manage multi-node",
    "weight": "250"
}This section describes how to set up and manage your multi-node environment.

Start the Transfer CFT cluster
------------------------------

### Start all node managers

For each host submit the JCL: **` MNRMNG`**

### Start all nodes

On *one* of the hosts in the cluster, submit the JCL: **`start JCL MNSTART`**

Stop the Transfer CFT cluster
-----------------------------

### Stop all nodes

On *one* of the hosts in the cluster, submit the JCL:**` stop JCL MNSTOP`**

### Stop all node managers

For each host, submit the JCL: **`copstop JCL COPSTOP`**

Add a node to the Transfer CFT cluster
--------------------------------------

In this example the Transfer CFT cluster accounts at the beginning two nodes, node 0 and node 1.

### Add a node

Execute the following command to add a new node: **`add_node (JCL MNANODE)`**

The node 2 is created. The cluster is composed of three nodes: node 0, node 1 and node 2. All associated files associated with node 2 are initialized and its node state is set to DISABLED.

> **Note**
>
> Note: When adding a node, you must add the corresponding new license for that node in a license-key file ..UPARM(PRODKEY) by default.

### Enable a node

Once the new node has been added, you can now enable it using the command:**` enable_node -n 2 (JCL MNENABLE)`**

### Start a node

The node 2 can be started using the command:**` start -n 2 (JCL MNSTART)`**

Remove a node from the Transfer CFT cluster
-------------------------------------------

> **Note**
>
> Note: Only the last node can be removed.

### Disable the last node

You must fence the last node before removing it, as follows.

Enter: **`disable_node –n <node_id> (JCL MNDISABL)`**

The node runs as long as its catalog is not empty. Once the catalog is empty, the node state is set to DISABLED and the node stops automatically.

### Remove the last node

After fencing and stopping the *last node*, you can remove it.

Enter:**`  remove_node –n <node_id> (JCL MNREMOVE)`**

<span id="Rebalanc"></span>

Rebalance after a fail over
---------------------------

Once the failed node manager is running again, you can rebalance the cluster by re-starting one or multiple nodes.

In this example there are two hosts (host00 and host01), and two nodes (node00 and node01). Node00 is running on host00, and node01 is running on host01.

1. Host00 and node00 experience a failover.
1. The host01 node manager re-starts the node00 locally.
1. Node00 and node01 run on host01.
1. Host00 and its node manager are manually re-started.
1. From one of the Transfer CFT cluster hosts, host00 or host01, execute the command:**` restart –n 0 (JCL MNRESTAR)`**
1. The host00 node manager restarts the node00 locally.

**Step results**: Node00 is running on host00, and node01 running on host01.
