{
    "title": "Multi-node commands",
    "linkTitle": "Multi-node commands",
    "weight": "230"
}This topic describes how to manage Transfer CFT nodes, and related actions such as restarting a stopped node, or rebalancing after a fail over.

cftinit
-------

The `cftinit` command initializes Transfer CFT internal datafiles. If nothing is specified, then all internal datafiles are initialized, both common and specific

#### Options

-c&#124;-common only common internal datafiles are initialized (PARM, PART, main COM)

-n&#124;-node only node specific internal datafiles are initialized (CATALOG, COM, LOG)

**Usage**

```
EXEC PCFTUTL,PG=CFT,PARM=’CFTINIT &CFTENV..SAMPLE(CFTPARM)’
```

Common internal datafiles are initialized using provided configuration files.

```
CFTINIT –N 2
```

Specific internal datafiles for node 2 are initialized (CATALOG.N02,COM.N02,LOG1.N02,LOG2.N02C).

`JCL`

`..INSTALL(MNINIT)`

cftcopl
-------

The executable `CFTCOPL` starts the Copilot (Node Manager and UI server).

**Syntax**

`EXEC PGM=CFTCOPL`

**JCL**

`..INSTALL(MNRMNG)`

The files such as COM, CATALOG, or LOG are not assigned in the JCL.

copstop
-------

The executable `COPSTOP` stops the Copilot (Node Manager and UI server). When stopping a Copilot on a host, all nodes running on this host are stopped and re-started on the other hosts in the cluster.

**Syntax**

`copstop`

**Usage**

`COPSTOP`

**JCL**

`..INSTALL(COPSTOP)`

start
-----

The `start `command starts one or all nodes. If no node is specified, all nodes are started.

> **Note**
>
> Note: You must first start the node manager.

**Syntax**

`start [options]`

#### Options

The -n&#124;-node &lt;node_id&gt; starts the node &lt;node_id&gt;.

**Usage**

`EXEC PCFTUTL,PG=CFT,PARM=’start’`

All nodes are started by the node managers.

`EXEC PCFTUTL,PG=CFT,PARM=’start –n 0’`

Node 0 is started by a node manager.

**JCL**

`..INSTALL(MNSTART)`

stop
----

The `stop` command stops one or all nodes. If no node is specified, all nodes are stopped.

**Syntax**

`stop [options]`

#### Options

The -n&#124;-node &lt;node_id&gt; stops the node &lt;node_id&gt;.

**Usage**

`EXEC PCFTUTL,PG=CFT,PARM=’stop’`

Stops all nodes.

`EXEC PCFTUTL,PG=CFT,PARM=’stop –n 0’`

Stops node 0.

**JCL**

`..INSTALL(MNSTOP)`

restart
-------

The` restart` command restarts one or all nodes. If no node is specified all nodes are restarted.

> **Note**
>
> Note: You must first start the node manager.

**Syntax**

`restart [options]`

**Options**

The `-n&#124;-node <node_id>` re-starts the node &lt;node_id&gt;.

The `-ln&#124;-local_node` re-starts all nodes hosted locally that are running on the host from where the command is performed.

**Usage**

`EXEC PCFTUTL,PG=CFT,PARM=’restart’`

All nodes are re-started by the node managers.

`EXEC PCFTUTL,PG=CFT,PARM=’restart –n 0’`

Node 0 is re-started by a node manager.

`EXEC PCFTUTL,PG=CFT,PARM=’restart –ln’`

All nodes hosted locally are re-started by the node manager.

**JCL**

`..INSTALL(MNRESTAR)`

add_host
---------

The `add_host` command adds a new host entry in the configuration. The following UCONF parameters are set:

- `cft.multi_node.hostnames`
- `cft.multi_node.hostnames.<hostname>.host = <host_address>`

**Syntax**

`add_host –hostname <hostname> –host <host_address>`

**Usage**

`EXEC PCFTUTL,PG=CFT,PARM=’add_host –hostname srv0 –host srv0.domain.int’`

**JCL**

`..INSTALL(MNAHOST)`

add_node
---------

The `add_node` command adds a new node to the Transfer CFT cluster. The number of nodes is incremented (uconf: cft.multi_node.nodes = N+1). The internal datafiles associated with the new node are initialized and the node state is set to DISABLED (uconf:cft.multi_node.nodes.&lt;node_id&gt;.nodestate).

**Syntax**

`add_node`

**Usage**

`EXEC PCFTUTL,PG=CFT,PARM=’add_node’`

**JCL**

`..INSTALL(MNANODE)`

remove_node
------------

The cft remove_node command removes the node identified by the higher node id in the Transfer CFT cluster. To remove a node, the node state must be both DISABLED (uconf:cft.multi_node.nodes.&lt;node_id&gt;.nodestate) and STOPPED (uconf:cft.multi_node.nodes.&lt;node_id&gt;.state).

The node number is decremented (uconf: cft.multi_node.nodes = N+1) , and any internal datafiles associated with the node are removed.

**Syntax**

`remove_node –n <the_higher_node_id>`

**Usage**

`EXEC PCFTUTIL,PG=CFT,PARM=’remove_node –n 3’`

**JCL**

`..INSTALL(MNREMOVE)`

enable_node
------------

The `enable_node` command enables the specified node. The node state is set from DISABLED to ENABLED_STOPPED (uconf:cft.multi_node.nodes.&lt;node_id&gt;.nodestate).

**Syntax**

`enable_node -n -<node_id>`

**Usage**

`EXEC PCFTUTIL,PG=CFT,PARM=’enable_node -n -<node_id>’`

**JCL**

`..INSTALL(MNENABLE)`

disable_node
-------------

The `disable_node` command disables the specified node. The parameter uconf:cft.multi_node.nodes.&lt;node_id&gt;.disabling is set to Yes.

- The node unregisters its listening points from the connection dispatcher so that it does not received incoming requests.
- Outgoing requests coming from APIs are no longer dispatched to this node.
- Once the catalog related to the node is empty, the node state is set to DISABLED and the node stops.

**Syntax**

`disable_node -n -<node_id>`

**Usage**

`EXEC PCFTUTIL,PG=CFT,PARM=’disable_node -n -<node_id>’`

**JCL**

`..INSTALL(MNDISABL)`

cftping
-------

The `cftping `command checks the status of one or all enabled nodes. By default the cftping checks status of all nodes.

Return values:

- 0: all enabled nodes are stopped
- 1: all enabled nodes are running
- 2: not all enabled nodes are running

**Syntax**

`cftping [options]`

**Options**

-n&#124;-node &lt;node_id&gt; checks the status of the node &lt;node_id&gt;

-v verbose mode

-p shows PID (Process IDs) of all CFTMAIN processes

-h shows the help

**JCL**

`..INSTALL(MNPING)`

listlog
-------

Use the CFTUTIL `listlog `command to display the log content, which can be defined according to certain criteria, such as date or node. Additionally, you can filter the log according to multiple criteria, or view a log that is merged for several nodes in cluster mode.

display/listcat
---------------

Use the CFTUTIL `display `or CFTUTIL `listcat `to show catalog transfer records. In multi-node, these commands aggregate all catalog internal datafiles to show catalog transfer records as a unique catalog.

> **Note**
>
> Note: The first character in the IDTU corresponds to the node number.

listnode
--------

The CFTUTIL **`listnode `**displays the status of the Transfer CFT cluster, including information about hosts and nodes that are part of the Transfer CFT multi-node architecture.

**JCL**

`..INSTALL(MNLNODE)`

**Example**

In this example four nodes are running on four different hosts.

```
--------------------------------------------------------------
Host z-zos111b 10.128.60.15 Copilot RUNNING (SOP700ZN J07928)
--------------------------------------------------------------
   Node id  Node state     CFT state CFT      JOB   Disabling
   ------- --------------- ------------ --------------- -----
   Node 01 ENABLED_STARTED  RUNNING  SOP700T3 J07936 No
   Node 02 ENABLED_STARTED   RUNNING SOP700T4 J07938 No
--------------------------------------------------------------
Host z-zos19 10.128.60.12 Copilot RUNNING (SOP700ZN J09205)
---------------------------------------------------------------
   Node id   Node state    CFT state CFT     JOB   Disabling
   ------- --------------- ------------ --------------- -------
   Node 00 ENABLED_STARTED  RUNNING SOP700T3 J09211 No
   Node 03 ENABLED_STARTED RUNNING SOP700T4 J09214 No
```
