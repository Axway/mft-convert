{
    "title": "Minimal downtime upgrade",
    "linkTitle": "Minimal downtime upgrade",
    "weight": 40,
    "date": "2019-10-07",
    "description": "Perform a minimal downtime upgrade to API Gateway 7.7."
}

{{% alert title="Caution" color="warning" %}}
Zero downtime upgrade (ZDU) is not always achievable for systems with high complexity or restrictions. The following instructions are designed to help you upgrade to API Gateway 7.7 with as little downtime as possible.
{{% /alert %}}

The standard process to upgrade to API Gateway 7.7 involves a short period of downtime during the _apply_ phase. With most `.fed` files, this downtime should not be more than a few minutes. However, with bigger `.fed` files of large and complex configurations, the downtime can be longer.

This topic describes an approach you can take, and some sample scripts you can use as a reference, to achieve a minimal downtime upgrade to API Gateway 7.7.

This approach involves the use of a load balancer to ensure that available API Gateways can always process traffic. If you are using a DevOps framework, the sample scripts provide an example for a basic high availability (HA) deployment and an NGINX load balancer to help you understand the required steps. The sample scripts provide an example only, and although the scripts are somewhat configurable, you must adapt them for your specific needs.

You can download the `API Gateway and API Manager 7.7 Zero Downtime Upgrade Scripts` package from [Axway Support](https://support.axway.com/). The package includes scripts for Linux.

{{< alert title="Note" color="primary" >}}
The sample scripts should only be used for API Gateways moving from version 7.5.2, or higher, to 7.7.

To minimize the impact on any shared data, such as quota counts, ensure to complete your upgrade in a single attempt.
{{< /alert >}}

## Reference configuration

The reference configuration is a three-node topology configured as follows:

* Two Admin Node Managers, one on Node A and the other on Node C.
* One Node Manager on Node B.
* Each node runs a single API Gateway instance that are grouped as follows:
    * `Gateway Group` containing API Gateway instances on Node A and Node B.
    * `GatewayManager Group` containing a single API Gateway instance running API Manager.

![Illustration on the ZDU scripts reference configuration](/Images/UpgradeGuide/APIgw_ZDU_ref_conf.png)

## Description of the ZDU script package

The ZDU script package (for example, `APIGateway_7.7_Package_ZDUScripts_linux-x86-64_BNYYYYMMDDn.zip`) contains the following folders and files:

* `README.md`: This file explains how the ZDU scripts work.
* `bin`: This folder contains the `zdupgrade` shell script for use on Linux.
* `config`: This folder contains a `topology.json` file for a sample configuration.
* `scripts`: This folder contains the individual sample scripts that the `zdupgrade` script uses. You can adapt these scripts to your specific needs.

### Sample scripts

The following diagram shows the architecture.

![ZDU scripts architecture](/Images/UpgradeGuide/zdu_arch.png)

The following table describes the purpose of each sample script.

| Script                | Description                                                                                                                                                                              |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `command_builder.py`  | Generates the Linux commands to be run (for example, `sysupgrade` commands, commands to start or stop processes).                                                                        |
| `command_executor.py` | Executes commands on a node (for example, run a `sysupgrade` step, start or stop a process, check if a process is running, roll back). Requires `command_builder` and `command_handler`. |
| `command_handler.py`  | Handles how commands are run on a node (for example, how to connect to remote nodes to run commands).                                                                                    |
| `conductor.py`        | Orchestrates the minimal downtime upgrade process (for example, connect to all nodes, run sysupgrade steps on each node in sequence, disconnect from all nodes, display a summary).         |
| `definitions.py`      | Defines variables used throughout the other scripts.                                                                                                                                     |
| `load_balancer.py`    | Placeholder script to implement load balancer logic (for example, to enable or disable traffic on a node).                                                                               |
| `logging_handler.py`  | Provides logging capabilities.                                                                                                                                                           |
| `options_parser.py`   | Parses and validates command-line options.                                                                                                                                               |
| `rollback_helper.py`  | Analyzes errors generated during the ZDU process and determines if a roll-back is needed.                                                                                                |
| `summary.py`          | Provides a summary of ZDU results per node and per upgrade phase.                                                                                                                        |
| `topology_builder.py` | Builds the topology information from the configuration file.                                                                                                                             |
| `upgrade_steps.py`    | Implements the ZDU process steps on each node.                                                                                                                                           |

See the comments included in each script for more detailed information.

## Use the ZDU scripts

This section describes how to use the sample ZDU scripts.

### Prerequisites

The `zdupgrade` scripts are intended to be run on a control node (for example, a local machine). This control node is responsible for connecting to and upgrading each of the nodes in the topology (remote nodes) in sequence.

The prerequisites for the node on which you intend to run the `zdupgrade` scripts (the control node) are as follows:

* You must use the same operating system on the control node and the remote nodes.
* You must install Python 2.6 or later (Python 2.7 recommended) and the `pip` package manager. For more details, see the [Python documentation](https://www.python.org/doc/). We recommend that you do not install Python under a path that contains spaces.
* On Linux, you must install cryptography dependencies. For more details, see [Cryptography Installations](https://cryptography.io/en/latest/installation.html).
* You must install the `paramiko` and `enum34` Python modules and their dependencies.
* You must download and install the ZDU sample script package from [Axway Support](https://support.axway.com/).
* You must have an RSA key for password-less login to the remote nodes over SSH. For more information, see [Configure SSH server](#configure-ssh-server).

The prerequisites for the remote nodes are as follows:

* You must install API Gateway version 7.7 on each node.

    The scripts expect the old installation location to be the same on each node and the new installation location to be the same on each node.

* You must have all required licenses for the API Gateway components on each node.
* The old installation Node Managers and API Gateways must be running.
* You must [configure a SSH server](#configure-ssh-server) on all nodes.

#### Configure SSH server

To enable a user to log in to a remote node without a password, you must configure a SSH server on each remote node.

Additionally, follow these steps:

1. Use `ssh-keygen` to generate a RSA key for the SSH user, for example:

    ```
    ssh-keygen -t rsa
    ```

    Alternatively, if you already have an existing SSH key, you can convert it to an RSA key, for example:

    ```
    ssh-keyscan -t rsa localhost
    ```

2. Copy the RSA key to each of the remote nodes, for example:

    ```
    ssh-copy-id -i <ssh user home>/.ssh/id_rsa.pub <ssh user>@<node>
    ```

3. Ensure that the RSA key is the first in the list of known hosts for each remote node.

For more information on using OpenSSH, go to <https://www.openssh.com/>.

### Run the zdupgrade script

This section describes how to install and run the `zdupgrade` script. It also describes the log files produced by the script.

#### Install the package

To install the package, unzip the package you downloaded from [Axway Support](https://support.axway.com/) to a directory on your local machine (for example, `/opt/zdu`).

#### Run with default options

To run the `zdupgrade` script with the default options, enter the following commands:

```
cd opt/zdu/bin
./zdupgrade --old_install_dir OLDINSTALLDIR --new_install_dir NEWINSTALLDIR
```

* `OLDINSTALLDIR` is the location of the old API Gateway installation.
* `NEWINSTALLDIR` is the location of the new API Gateway 7.7 installation.

The scripts expect the old installation location to be the same on each node and the new installation location to be the same on each node.

The `--old_install_dir` and `--new_install_dir` options are case sensitive.

The scripts use the reference topology unless you specify a topology file using the `--config` option. A sample topology file is provided in `config/topology.json`, however you must update this file to match your configuration. Do not use the `topology.json` file in your API Gateway installation instead of this file as they have a different purpose and format. For more information, see [Specify topology configuration to use](#specify-topology-configuration-to-use).

#### View script options

To see the mandatory and optional arguments for the `zdupgrade` script, enter the following commands:

```
cd opt/zdu/bin
./zdupgrade --help
```

#### Log files

When you run the `zdupgrade` script, it generates two types of log files :

* ZDU script log
* Node-specific log

The ZDU script log is a timestamped log file in the `/zdu/logs` directory. This log file tracks the progress of the `zdupgrade` script steps, and in case of a failure, logs the error message on what went wrong so you can fix any issues in the upgrade. This log file also contains a summary table for the upgrade, which you can use to quickly identify if the upgrade was successful, or if it was not successful you can see on which node and at what step the upgrade failed.

The following shows an example summary for a successful upgrade:

```
Summary table (Status.SUCCESS):
    +------------+------------+------------+------------+------------+------------+------------+
    | MULTI-NODE ZERO DOWN-TIME UPGRADE SUMMARY                                                |
    +------------+------------+------------+------------+------------+------------+------------+
    | NODE       | EXPORT     | UPGRADE    | PRE-APPLY  | APPLY      | POST-APPLY | ROLLBACK   |
    +------------+------------+------------+------------+------------+------------+------------+
    | Node-A     | PASSED (0) | PASSED (0) | PASSED (0) | PASSED (0) | PASSED (0) | *        |
    +------------+------------+------------+------------+------------+------------+------------+
    | Node-B     | PASSED (0) | PASSED (0) | PASSED (0) | PASSED (0) | PASSED (0) | *        |
    +------------+------------+------------+------------+------------+------------+------------+
    | Node-C     | PASSED (0) | PASSED (0) | PASSED (0) | PASSED (0) | PASSED (0) | *        |
    +------------+------------+------------+------------+------------+------------+------------+
```

The following shows an example summary for a failed upgrade:

```
Summary table (Status.UPGRADE_FAILED):
    +--------------+--------------+--------------+--------------+--------------+--------------+--------------+
    | MULTI-NODE ZERO DOWN-TIME UPGRADE SUMMARY                                                              |
    +--------------+--------------+--------------+--------------+--------------+--------------+--------------+
    | NODE         | EXPORT       | UPGRADE      | PRE-APPLY    | APPLY        | POST-APPLY   | ROLLBACK     |
    +--------------+--------------+--------------+--------------+--------------+--------------+--------------+
    | Node-A       | PASSED (0)   | PASSED (0)   | *          | *          | *          | *          |
    +--------------+--------------+--------------+--------------+--------------+--------------+--------------+
    | Node-B       | PASSED (0)   | PASSED (0)   | *          | *          | *          | *          |
    +--------------+--------------+--------------+--------------+--------------+--------------+--------------+
    | Node-C       | PASSED (0)   | FAILED (100) | *          | *          | *          | *          |
    +--------------+--------------+--------------+--------------+--------------+--------------+--------------+
```

A return status code is included in the summary (for example, `Status.UPGRADE_FAILED` indicates that the `sysupgrade upgrade` step failed). The full list of return status codes are explained in the comments of the `definitions.py` script but are reproduced here for convenience.

| Return Status Code  | Value |
|---------------------|-------|
| SUCCESS             | 0     |
| EXPORT_FAILED      | 70    |
| UPGRADE_FAILED     | 71    |
| PRE_APPLY_FAILED  | 72    |
| APPLY_FAILED       | 73    |
| POST_APPLY_FAILED | 74    |
| ROLLBACK_FAILED    | 75    |
| FATAL_ERROR        | 80    |

The node-specific logs are in the `/zdu/logs/<node name>` directory. The node-specific logs contain the output and possible errors for the commands run on that particular node. When troubleshooting a failure, you can check the node-specific log file to identify the exact source of the error.

For example, for the upgrade failure on Node-C above, the Node-C log file shows the following:

```
########## SYSUPGRADE_UPGRADE ##########
------------- ZDU_EXEC_CMD -------------
command [
    cd /opt/Axway-7.5.3/apigateway/upgrade/bin
    /opt/Axway-7.5.3/apigateway/upgrade/bin/sysupgrade upgrade --cass_host localhost --cass_port 9160
]
ERROR: This is a multi-node upgrade and a localhost Cassandra server: localhost:9160 has been specified.
You must use a network accessible address. Run sysupgrade upgrade -h for details.

------------- ZDU_EXEC_CMD -------------
########## SYSUPGRADE_UPGRADE ##########
```

### Customize the zdupgrade sample scripts

This section provides some examples of how you might customize the sample scripts for your own use, and provides suggested steps for modifying the sample scripts.

Perform the following steps to modify the sample scripts:

1. Change to the `zdu/scripts` directory.
2. Open the sample script you want to change in your preferred editor.
3. Edit the script to suit your configuration. You can also enable or disable the script as needed.
4. Save the modified script.
5. Change to the `zdu/bin` directory, and run the `zdupgrade` script with the desired options.
6. Change to the `zdu/logs` directory and check the results. If further adjustments to the script are needed, repeat steps 2 to 6.

#### Specify topology configuration to use

You must specify your topology configuration to the `zdupgrade` script using the `--config <path_to_the_user_topology_file>` option.

If you do not specify any topology configuration using the `--config` option, the default topology file (`config/topology.json`) is not used, and the [reference configuration](#reference-configuration) is used.

Alternatively, you can implement the method `__getTopology(anmHost)` in `topology_builder.py` to return the correct topology configuration for your environment.

{{< alert title="Note" color="primary" >}}The intention of the `__getTopology` method is to autodiscover the topology from the Admin Node Manager host, meaning that a topology configuration file would not be required. {{< /alert >}}

#### Escape argument values

The argument values are not escaped before they are used to build the upgrade commands to be executed in the nodes. If required, you must modify the `command_builder.py` script to escape the values accordingly. For example, on Linux, you could place values containing spaces in quotes.

#### Customize roll-back operation

If the upgrade fails at any stage for any reason, the `zdupgrade` script can perform a roll-back operation and return the configuration to the state it was before running the `zdupgrade` script.

Use the `rollback_helper.py` script to decide if a roll-back is required depending on the upgrade phase and its result. The results for the previously executed upgrade phases are also available from the `node.result` parameter.

You must resolve and fix any failures during the roll-back procedure based
on the feedback from the ZDU scripts and logs.

You can customize the error handling routine in the
script to ignore some errors and let the upgrade proceed to
the next node. In this case, you must fix the errors in the failing nodes and
take care of reconfiguring the new system to match the old configuration if
required.

#### Enable or disable traffic

The `load_balancer.py` script is a placeholder to enable or disable traffic to a node through a load balancer if required. You must implement the `enable` and `disable` methods in this script to suit your own load balancing solution.

This script is already called as part of the ZDU processing with a dummy implementation.

#### Customize startup and shutdown times

You can customize the variables that control the time to wait for processes to start or stop, depending on how long it takes your system to start up or shut down. These variables are set in `command_executor.py`. The defaults in seconds are:

```
STARTUP_TIME = 30
SHUTDOWN_TIME = 10
```

### Troubleshooting

This section details some common problems that you might encounter when running the `zdupgrade` scripts, and provides suggested workarounds.

#### Script fails if the Python path contains spaces

The `zdupgrade` scripts do not expect to have spaces in the Python path. For example, the script fails if Python is installed in `/opt/My Python/bin/`.

We recommend that you do not install Python under a path that contains spaces.

Alternatively, you can change the scripts to surround any variables with quotes to avoid the error.
