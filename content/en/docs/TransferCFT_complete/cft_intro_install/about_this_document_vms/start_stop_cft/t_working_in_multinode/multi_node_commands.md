{
    "title": "Multi-node commands",
    "linkTitle": "Multi-node commands",
    "weight": "270"
}This topic describes how to mange Transfer CFT nodes, and related actions such as restarting a stopped node, or rebalancing after a fail over.

cftinit
-------

The cftinit command initializes Transfer CFT internal datafiles. If nothing is specified, then all internal datafiles are initialized, both common and specific.

### Syntax

`cftinit [options] <filename>[<filename>] […]`

#### Options

-c&#124;-common only common internal datafiles are initialized (PARM, PART, main COM)

-n&#124;-node only node specific internal datafiles are initialized (CATALOG, COM, LOG)

### Usage

`cftinit conf/cft-tcp.conf con/cft-tcp-part.conf`

All internal datafiles are initialized using provided configuration files.

`cftinit –c conf/cft-tcp.conf con/cft-tcp-part.conf`

Common internal datafiles are initialized using provided configuration files.

`cftinit –n 2`

Specific internal datafiles for node 2 are initialized (cftcata02, cftcom02, cftlog02).

copsmng
-------

The `copsmng `command starts the Copilot (Node Manager, Connection Dispatcher, UI server)

### Syntax

`copsmng`

### Usage

`copsmng`

copstop
-------

The `copstop `command stops the Copilot (Node Manager, Connection Dispatcher, UI server). When stopping a Copilot on a host, all nodes running on this host are stopped and re-started on the other hosts in the cluster.

### Syntax

`copstop`

### Usage

`copstop`

cft start
---------

The `cft start `command starts one or all nodes. If no node is specified, all nodes are started.

> **Note**
>
> Note: Node managers must be started first.

### Syntax

`cft start [options]`

#### Options

The -n&#124;-node &lt;node_id&gt; starts the node &lt;node_id&gt;.

### Usage

`cft start`

All nodes are started by the node managers.

`cft start –n 0`

Node 0 is started by a node manager.

cft stop
--------

The `cft stop` command stops one or all nodes. If no node is specified, all nodes are stopped.

### Syntax

`cft stop [options]`

#### Options

The -n&#124;-node &lt;node_id&gt; stops the node &lt;node_id&gt;.

### Usage

`cft stop`

Stops all nodes.

****cft stop –n 0****

Stops node 0.

cft restart
-----------

The` cft restart` command re-stars one or all nodes. If no node is specified all nodes are re-started.

> **Note**
>
> Note: Node managers must be started first.

### Syntax

`cft restart [options]`

#### Options

The `-n&#124;-node <node_id>` re-starts the node &lt;node_id&gt;.

The `-ln&#124;-local_node` re-starts all nodes hosted locally that are running on the host from where the command is performed.

### Usage

`cft restart`

All nodes are re-started by the node managers.

`cft restart –n 0`

Node 0 is re-started by a node manager.

`cft restart –ln`

All nodes hosted locally are re-started by the node manager.

cft add_host
-------------

The `cft add_host` command adds a new host entry in the configuration. The following UCONF parameters are set:

- uconf:cft.multi_node.hostnames
- uconf:cft.multi_node.hostnames.&lt;hostname&gt;.host = &lt;host_address&gt;

### Syntax

`cft add_host –hostname <hostname> –host <host_address>`

### Usage

`cft add_host –hostname srv0 –host srv0.domain.int`

cft add_node
-------------

The `cft add_node` command adds a new node to the Transfer CFT cluster. The number of nodes is incremented (uconf: cft.multi_node.nodes = N+1) . The internal datafiles associated with the new node are initialized and the node state is set to DISABLED (uconf:cft.multi_node.nodes.&lt;node_id&gt;.nodestate).

### Syntax

`cft add_node`

### Usage

`cft add_node`

cft remove_node
----------------

The cft remove_node command removes the node identified by the higher node id in the Transfer CFT cluster. To remove a node, the node state must be both DISABLED (uconf:cft.multi_node.nodes.&lt;node_id&gt;.nodestate) and STOPPED (uconf:cft.multi_node.nodes.&lt;node_id&gt;.state).

The node number is decremented (uconf: cft.multi_node.nodes = N+1) , and any internal datafiles associated with the node are removed.

### Syntax

`cft remove_node –n <the_higher_node_id>`

### Usage

`cft remove_node –n 3`

cft enable_node
----------------

The `cft enable_node` command enables the specified node. The node state is set from DISABLED to ENABLED_STOPPED (uconf:cft.multi_node.nodes.&lt;node_id&gt;.nodestate).

### Syntax

`cft enable_node -n -<node_id>`

### Usage

`cft enable_node -n -<node_id>`

cft disable_node
-----------------

The `cft disable_node` command disables the specified node. The parameter uconf:cft.multi_node.nodes.&lt;node_id&gt;.disabling is set to Yes.

- The node unregisters its listening points from the connection dispatcher so that it does not received incoming requests.
- Outgoing requests coming from APIs are no longer dispatched to this node.
- Once the catalog related to the node is empty, the node state is set to DISABLED and the node stops.

### Syntax

`cft disable_node -n -<node_id>`

### Usage

`cft disable_node -n -<node_id>`

cftping
-------

The `cftping `command checks the status of one or all enabled nodes. By default the cftping checks status of all nodes.

Return values:

- 0: all enabled nodes are stopped
- 1: all enabled nodes are running
- 2: not all enabled nodes are running

### Syntax

`cftping [options]`

#### Options

-n&#124;-node &lt;node_id&gt; checks the status of the node &lt;node_id&gt;

-v verbose mode

-p shows PID (Process IDs) of all CFTMAIN processes

-h shows the help

Listlog
-------

Use the CFTUTIL `listlog `command to display the log content, which can be defined according to certain criteria, such as date or node. Additionally, you can filter the log according to multiple criteria, or view a log that is merged for several nodes in cluster mode.

Cftutil listlog LINES=-200, cftutil listlog node=2

Display/Listcat
---------------

Use the CFTUTIL `display `or CFTUTIL `listcat `to show catalog transfer records. In multi-node, these commands aggregate all catalog internal datafiles to show catalog transfer records as a unique catalog.

> **Note**
>
> Note: The first character in the IDTU corresponds to the node number.

Listnode
--------

The CFTUTIL `listnode `displays the status of the Transfer CFT cluster, including information about hosts and nodes that are part of the Transfer CFT multi-node architecture.

****Example****

In this example four nodes are running on four different hosts.

![](/Images/TransferCFT/examplelistnode.png)
