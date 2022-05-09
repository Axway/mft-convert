{
    "title": "Upgrade and migration overview",
    "linkTitle": "Overview",
    "weight": 10,
    "date": "2019-10-07",
    "description": "Introduction to API Gateway upgrade and migration, and recommended best practices for test upgrades."
}

Before you proceed with an upgrade, note the following:

* Upgrading or installing API Gateway or API Manager on Windows is not supported.
* Upgrading or migrating from any previous version of API Gateway requires a new Axway license key.
* Throughout this guide the term *old installation* is used to refer to the earlier version of API Gateway (the version being upgraded), while the term *new installation* is used to refer to the 7.7 version of API Gateway.

## Upgrade and migration process

The upgrade and migration process involves exporting data from your old installation, upgrading the data, and applying the upgraded data to your new installation. Each step must complete successfully before you can proceed to the next step.

{{< alert title="Note" color="primary" >}}After you have installed API Gateway 7.7, ensure you have applied the latest available service pack before starting the upgrade. {{< /alert >}}

![Overview of upgrade process](/Images/UpgradeGuide/APIgw_ETLUpgrade.png)

API Gateway provides the `sysupgrade` command to upgrade your API Gateway system and migrate your data. This script provides feedback at each step, which enables you to resolve any issues before proceeding to the next step. This ensures that all possible issues are identified and resolved before you apply the upgraded data to your new installation.

![Overview of upgrade steps](/Images/UpgradeGuide/APIgw_upgrade_overview.png)

An upgrade always requires the following steps:

1. Install API Gateway 7.7 in a new directory.
2. Apply the latest available service pack for API Gateway 7.7.
3. Export the data from the old installation.
4. Validate and upgrade the data. Resolve any issues identified by the upgrade process before you proceed to the next step.
5. Apply the upgraded data to the 7.7 installation.
6. Verify that the upgrade completed successfully.

### Upgrade commands

To perform an upgrade, you must always run the following `sysupgrade` commands in the following order:

| Step | Command   | Description                                                                                                                                                                                                                |
|------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | `export`  | Export data from the old API Gateway installation on the local node.                                                                                                                                                       |
| 2    | `upgrade` | Validate data exported from the old installation, and upgrade it to version 7.7. You can keep your old installation running during this step, so you can fix any issues reported in the logs without service interruption. |
| 3    | `apply`   | Create the new API Gateway processes on the local node, and import the upgraded data into the 7.7 installation.                                                                                                            |

{{< alert title="Tip" color="primary" >}}You can also use the `status` command at any stage to see which commands have run, and which command will run next (see [Utility commands](#utility-commands)).{{< /alert >}}

For more details on performing an upgrade, see [Upgrade from API Gateway 7.5.x or 7.6.x](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/).

### Utility commands

The `sysupgrade` script also provides utility commands. These optional commands can be useful when performing more complex upgrades:

| Command  | Description                                                                                                                                                                            |
|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `status` | Get the status of the `sysupgrade` process on the local node. For example, which commands have run (`export`, `upgrade`, or `apply`), and which command will run next.
  Running the `status` command is recommended before you run each command, especially in a multi-node upgrade where it is easy to lose track of what upgrade step or machine you are on.  |
| `clean`  | Reset the new installation on the current node back to a factory installation. This enables you to restart the `sysupgrade` process.                                                   |

For details on all commands and options, see [sysupgrade command reference](/docs/apim_installation/apigw_upgrade/upgrade_script/).

## Best practices for test upgrades

To perform a test upgrade, run the `export` and `upgrade` commands on all nodes in your API Gateway domain. You can do this while the old API Gateway installation is still running, because the `export` and `upgrade` commands do not modify the old installation. This way you can identify and resolve any issues with the upgrade without any service interruption. For example:

1. Run the `export` command on Node1.
2. Run the `upgrade` command on Node1.
3. Analyze the warnings and errors on Node1, and take any necessary actions. You must rerun `export` and `upgrade` after updating your configuration to resolve any warnings or errors. You might need to rerun `export` and `upgrade` multiple times on Node1 to ensure that all issues are resolved.
4. Repeat steps 1, 2, and 3 for Node2, Node3, and so on, until all issues are resolved on all nodes.

    As long as `apply` has not been run on any node, you can rerun `export` and `upgrade` as many times as necessary without affecting other nodes. Rerunning `export` and `upgrade` on a node does not mean they must be rerun on other nodes in the system.

    When you are happy with the results of the test upgrade on all nodes, you can shut down the old installation and apply the upgraded data to the new 7.7 installation. For example:

5. Run the `apply` command on Node1, Node2, and so on, until the upgrade is complete.

For a detailed example of a multi-node upgrade from 7.5.x or 7.6.x, see [Multi-node upgrade example](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/#multi-node-upgrade-example).

## Resolve upgrade errors and warnings

The `export` and `upgrade` commands identify issues in the configuration that might cause problems during the upgrade. Errors indicate problems that you must resolve before proceeding with an upgrade, while warnings indicate issues that might require action. By identifying and resolving issues at this stage, you can avoid unexpected issues when using the `apply` command to complete the upgrade. For more details on errors and warnings, see [sysupgrade error reference](/docs/apim_installation/apigw_upgrade/upgrade_errors/).
