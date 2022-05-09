{
"title": "Frequently asked questions",
  "linkTitle": "Frequently asked questions",
  "weight": 140,
  "date": "2019-10-07",
  "description": "Get answers to frequently asked questions about upgrade."
}

## All upgrades

The following FAQs are specific to all upgrades.

### What happens if you change the old API Gateway installation after running `export`?

If you make any changes to the old API Gateway installation after running `export` and you do not need these changes to be included in the new 7.7 installation, you do not need to take any action.

If you do want the changes to be included in the new 7.7 installation, you must rerun `export`, possibly on all nodes, depending on the changes made. For example, if you deploy a new configuration to a group of API Gateways, you must rerun `export` on all nodes that run instances in that group.

{{< alert title="Note" color="primary" >}}If you rerun `export`, you must rerun all subsequent steps (`upgrade` and `apply`). If you rerun `export`, and therefore `upgrade` and `apply`, on the first Admin Node Manager, you have cleaned your topology, so you must rerun `apply` on all other nodes.{{< /alert >}}

### Why would you rerun `upgrade`?

You must rerun `upgrade` if:

* You have rerun `export`.
* The previous attempt to run `upgrade` failed with errors.

{{< alert title="Note" color="primary" >}}When upgrading very large configurations, the default memory settings might not be sufficient for `upgrade` to run successfully. See [Out of memory error when running upgrade](/docs/apim_installation/apigw_upgrade/upgrade_troubleshoot/#out-of-memory-error-when-running-upgrade).{{< /alert >}}

### Why would you rerun `export`?

You must rerun `export` if:

* The previous attempt to run `export` failed (for example, because the old installation processes were not running).
* You have made a change to the old installation that you also want to apply to the new installation.
* You updated the configuration in the old installation to resolve a warning or error reported when you ran `upgrade`.

{{< alert title="Note" color="primary" >}}If you rerun `export`, you must rerun all subsequent steps (`upgrade` and `apply`).{{< /alert >}}

### How to increase timeouts for `apply`

Increasing the timeout for node managers to prevent errors being returned when you run the [apply](/apim_installation/apigw_upgrade/upgrade_script/#apply-command) command during an upgrade.

1. Stop all 7.7 `vshell` processes.
2. Back up previous 7.7 upgrade directories:

    ```
    mv apigateway/upgrade_backup apigateway/upgrade_backup.old
    mv apigateway/upgrade/bin/out apigateway/upgrade/bin/out.old
    ```

3. Reset topology if it's not already clean:

    ```
     ./managedomain --reset --username admin --password changeme
    ```

4. Amend the following templates configurations with the desired value for `maxTransTimeout` in `PrimaryStore.xml` before running `sysupgrade` steps on each node/host.

    * **Node Manager**: `apigateway/system/conf/templates/`
    * **Node Manager**: `apigateway/system/conf/templates/VordelNodeManager/`
    * **API Gateway**: `apigateway/system/conf/templates/FactoryConfiguration-VordelGateway.fed`
    * **API Gateway**: `apigateway/system/conf/templates/FactoryConfiguration-VordelGateway/`

    {{< alert title="Note" color="primary" >}}On Linux systems, you can install the unzip package, which is very useful to check timeouts in `.fed` archive files. For example,

    zipgrep maxTransTimeout FactoryConfiguration-VordelGateway.fed
    {{< /alert >}}

5. Run the [export](/apim_installation/apigw_upgrade/upgrade_script/#export-command) command on all nodes.

6. Amend `maxTransTimeout` to the desired value in the exported configuration `PrimaryStore.xml`:

    * **Node Manager**: `apigateway/upgrade/bin/out/export/esnm/conf/fed/`. For Admin Node Managers, it might be require to apply factor based on the maximum number of instances in a group.
    * **API Gateway**: `apigateway/upgrade/bin/out/export/esgroups/groups/group-2/f5304aa6-a0f5-4643-88b7-8cef992924e4/`. Group and configuration IDs will vary.
    * **API Gateway**: `apigateway/upgrade/bin/out/export/esgroups/groups/group-2/f5304aa6-a0f5-4643-88b7-8cef992924e4.fed`. Group and configuration IDs will vary.

7. Run the [export](/apim_installation/apigw_upgrade/upgrade_script/#export-command) and [upgrade](/docs/apim_installation/apigw_upgrade/upgrade_script/#upgrade-command) commands on all nodes.

8. If topology has multiple Admin node managers, run the [apply](/apim_installation/apigw_upgrade/upgrade_script/#apply-command) step for the Admin node manager nodes first.

### Why would you rerun `apply`?

You must rerun `apply` if:

* You have rerun `export` or `upgrade`.
* The previous attempt to run `apply` failed with errors (for example, if you specified the wrong `--anm_host` parameter).

### Why would you run `clean`?

You might choose to run `clean` if:

* Issues occurred during the upgrade process and you want to restart the upgrade.
* You need to restart the upgrade to capture new changes made to the old API Gateway installation.

{{< alert title="Note" color="primary" >}}The `export --force` command also triggers a full clean of the `export`, `upgrade`, and `apply` outputs, but in addition it also attempts to run `export`; while the `clean` command only cleans the outputs.{{< /alert >}}

## Single-node upgrades

The following FAQs are specific to single-node upgrades.

### What happens if you rerun `export` when you have already run `apply`?

In a single-node domain, if you have previously run `export` successfully and try to rerun `export`, you are prompted to rerun with the `--force` option. This cleans the `export`, `upgrade`, and `apply` outputs, and then reruns `export` on the node. You must then rerun `upgrade` and `apply` on the node. The `--force` option is not required for `upgrade` or `apply` as the outputs have already been cleaned.

{{< alert title="Note" color="primary" >}}Before you rerun `export`, you must ensure that any processes in the new 7.7 installation are stopped, and that the processes in the old installation are running.{{< /alert >}}

### What happens if you rerun `upgrade` when you have already run `apply`?

In a single-node domain, if you have previously run `upgrade` successfully, and try to rerun, you are prompted to rerun with the `--force` option. This cleans the `upgrade` and `apply` command outputs, and then reruns `upgrade` on the node. You must then rerun `apply` on the node. The `--force` option is not required for `apply` as the output has already been cleaned.

{{< alert title="Note" color="primary" >}}The `upgrade` command runs offline and does not require any API Gateway processes to be running.{{< /alert >}}

### What happens if you rerun `apply`?

In a single-node domain, if you have previously run `apply` successfully, and try to rerun, you are prompted to rerun `apply` with the `--force` option. This cleans the `apply` command output, and then reruns `apply` on the node.

{{< alert title="Note" color="primary" >}} If any API Gateway processes are running in the old or new installation, you are prompted to stop them before `apply --force` can succeed.{{< /alert >}}

### What happens if you run `clean`?

If you run `clean` on a single-node domain, you are back to the start of the `sysupgrade` process. To redo the upgrade, you must:

1. Run `export`. Before you run `export`, you must ensure that all processes in the new 7.7 installation are stopped, and that all processes in the old installation are running.
2. Run `upgrade`.
3. Shut down all processes in the old installation.
4. Run `apply`.

## Multi-node upgrades

The following FAQs are specific to multi-node upgrades. The example topology referenced in these FAQs is as follows:

![Example multi-node topology](/Images/UpgradeGuide/APIgw_upgrade%20topology.png)

### Which is the first Admin Node Manager?

In a multi-node domain, the Admin Node Manager that has the domain CA private key file is considered the first Admin Node Manager. To determine if an Admin Node Manager is the first Admin Node Manager, check if the following file exists on the node:

`apigateway/groups/certs/private/domain.p12`

In the old installation, the first Admin Node Manager is always the first Node Manager created in the domain. In the new installation, the first Admin Node Manager is always the first node on which you run `apply`, and that node must be an Admin Node Manager in the old API Gateway domain.

{{< alert title="Tip" color="primary" >}}The first Admin Node Manager does not have to be on the same node in the old and new API Gateway installations, but this is the simplest approach. For example, in a three-node domain with Admin Node Managers on NodeA and NodeC (and none on NodeB), where NodeA is the first Admin Node Manager in the old installation, the first Admin Node Manager can be on NodeC in the new installation (if you run `apply` on NodeC first). However, the first Admin Node Manager cannot be on NodeB in the new installation, as NodeB was not an Admin Node Manager in the old installation.{{< /alert >}}

### What happens if you rerun `export` on the first Admin Node Manager and have already run `apply`?

In a multi-node domain, if you have previously run `export` successfully on the first Admin Node Manager node (for example, NodeA) and try to rerun `export`, you are prompted to rerun with the `--force` option. This cleans the `export`, `upgrade`, and `apply` outputs on NodeA, and then reruns `export` on NodeA. Running `export --force` on the first Admin Node Manager in a multi-node domain also cleans your topology and domain CA private key and certificate.

{{< alert title="Note" color="primary" >}}Before you rerun `export`, you must ensure that all processes in the new 7.7 installation are stopped *on all nodes*, and that all processes in the old installation are running *on all nodes*.{{< /alert >}}

After running `export --force` on the first Admin Node Manager (for example, NodeA) in a multi-node domain, you must perform the following steps:

1. Rerun `upgrade` on NodeA. You might also need to rerun `export` and `upgrade` on other nodes. We recommend using the `status` command on all other nodes to ensure that all nodes are at the same stage before proceeding.
2. Shut down all processes in the old installation on all nodes.
3. Rerun `apply` on NodeA.
4. Rerun `apply` on all other nodes, in any order. This step is necessary because running `export --force` on NodeA cleans your topology and domain CA private key and certificate.

### What happens if you rerun `export` on a node that is not the first Admin Node Manager and have already run `apply`?

In a multi-node domain, if you have previously run `export` successfully on a node that is not the first Admin Node Manager node (for example, NodeC) and try to rerun `export`, you are prompted to rerun with the `--force` option. This cleans the `export`, `upgrade`, and `apply` outputs on NodeC, and then reruns `export` on NodeC. NodeC can be an Admin Node Manager or a Node Manager.

{{< alert title="Note" color="primary" >}}Before you rerun `export`, you must ensure that all processes in the new 7.7 installation are stopped *on all nodes*, and that all processes in the old installation are running *on all nodes*.{{< /alert >}}

After running `export --force` on a node that is not the first Admin Node Manager node (for example, NodeC) in a multi-node domain, you must perform the following steps:

1. Rerun `upgrade` on NodeC.
2. Shut down all processes in the old installation on all nodes.
3. Start up the processes in the new 7.7 installation on other nodes, especially the Admin Node Manager on NodeA, as this must be running before you can proceed to the next step.
4. Rerun `apply` on NodeC. Because you ran `apply` previously on NodeC, this rerun triggers removal of NodeC entries in the topology on the Admin Node Manager on NodeA, before NodeC is registered.

You do not need to rerun commands on other nodes as a result of running `export --force` on NodeC.

### What happens if you rerun `upgrade` on the first Admin Node Manager and have already run `apply`?

In a multi-node domain, if you have previously run `upgrade` successfully on the first Admin Node Manager node (for example, NodeA) and try to rerun `upgrade`, you are prompted to rerun with the `--force` option. This cleans the `upgrade` and `apply` outputs on NodeA, and then reruns `upgrade` on NodeA. Running `upgrade --force` on the first Admin Node Manager in a multi-node domain also cleans your topology and domain CA private key and certificate.

{{< alert title="Note" color="primary" >}}The `upgrade` command runs offline and does not require any API Gateway processes to be running.{{< /alert >}}

After running `upgrade --force` on the first Admin Node Manager (for example, NodeA) in a multi-node domain, you must perform the following steps:

1. Shut down all processes in the old installation on all nodes. You might also need to rerun `upgrade` on other nodes. We recommend using the `status` command on all other nodes to ensure that all nodes are at the same stage before proceeding.
2. Rerun `apply` on NodeA.
3. Rerun `apply` on all other nodes, in any order. This step is necessary because running `upgrade --force` on NodeA cleans your topology and domain CA private key and certificate.

### What happens if you rerun `upgrade` on a node that is not the first Admin Node Manager and have already run `apply`?

In a multi-node domain, if you have previously run `upgrade` successfully on a node that is not the first Admin Node Manager node (for example, NodeC) and try to rerun `upgrade`, you are prompted to rerun with the `--force` option. This cleans the `upgrade` and `apply` outputs on NodeC, and then reruns `upgrade` on NodeC. NodeC can be an Admin Node Manager or a Node Manager.

{{< alert title="Note" color="primary" >}}The `upgrade` command runs offline and does not require any API Gateway processes to be running.{{< /alert >}}

After running `upgrade --force` on a node that is not the first Admin Node Manager node (for example, NodeC) in a multi-node domain, you must perform the following steps:

1. Shut down all processes in the old installation on all nodes.
2. Shut down all processes in the new 7.7 installation on NodeC (for example, any processes started by a previous `apply`).
3. Start up the processes in the new 7.7 installation on other nodes, especially the Admin Node Manager on NodeA, as this must be running before you can proceed to the next step.
4. Rerun `apply` on NodeC. Because you ran `apply` previously on NodeC, this rerun triggers removal of NodeC entries in the topology on the Admin Node Manager on NodeA, before NodeC is registered.

You do not need to rerun commands on other nodes as a result of running `upgrade --force` on NodeC.

### What happens if you rerun `apply` on the first Admin Node Manager?

In a multi-node domain, if you have previously run `apply` successfully on the first Admin Node Manager node (for example, NodeA) and try to rerun `apply`, you are prompted to rerun with the `--force` option. This cleans the `apply` output on NodeA and then reruns `apply` on NodeA. Running `apply --force` on the first Admin Node Manager in a multi-node domain also cleans your topology and domain CA private key and certificate.

{{< alert title="Note" color="primary" >}} If any API Gateway processes are running in the old or new installation, you are prompted to stop them before `apply --force` can succeed.{{< /alert >}}

After running `apply --force` on the first Admin Node Manager (for example, NodeA) in a multi-node domain, you must perform the following steps:

1. Leave all processes in the new 7.7 installation running on NodeA.
2. Shut down all processes in the new 7.7 installation on all other nodes (the processes in the old installation should already be down).
3. Rerun `apply` on all other nodes, in any order.

### What happens if you rerun `apply` on a node that is not the first Admin Node Manager?

In a multi-node domain, if you have previously run `apply` successfully on a node that is not the first Admin Node Manager (for example, NodeC) and try to rerun `apply`, you are prompted to rerun with the `--force` option. This cleans the `apply` outputs on NodeC, and then reruns `apply` on NodeC. NodeC can be an Admin Node Manager or a Node Manager.

{{< alert title="Note" color="primary" >}} If any API Gateway processes are running in the old or new installation, you are prompted to stop them before `apply --force` can succeed.{{< /alert >}}

You do not need to rerun commands on other nodes as a result of running `apply --force` on NodeC.

### What happens if you run `clean` on the first Admin Node Manager?

In a multi-node domain, if you run `clean` successfully on the first Admin Node Manager (for example, NodeA), this cleans the `export`, `upgrade`, and `apply` outputs on NodeA. It also cleans your topology and domain CA private key and certificate.

To redo the upgrade on NodeA, you must perform the following:

1. Run `export` on NodeA. Before you run `export`, you must ensure that all processes in the new 7.7 installation are stopped *on all nodes*, and that all processes in the old installation are running *on all nodes*.
2. Run `upgrade` on NodeA. You might also need to rerun `export` and `upgrade` on other nodes. We recommend using the `status` command on all other nodes to ensure that all nodes are at the same stage before proceeding.
3. Shut down all processes in the old installation on all nodes.
4. Run `apply` on NodeA.
5. Run `apply` on all other nodes, in any order. This step is necessary because running `clean` on NodeA cleans your topology and domain CA private key and certificate.

### What happens if you run `clean` on a node that is not the first Admin Node Manager?

In a multi-node domain, if you run `clean` successfully on a node that is not the first Admin Node Manager (for example, NodeC), this cleans the `export`, `upgrade`, and `apply` outputs on NodeC. Your topology and domain CA private key and certificate still exist on another Admin Node Manager node (for example, NodeA).

To redo the upgrade on the node you have cleaned, you must perform the following steps:

1. Run `export` on NodeC. Before you run `export`, you must ensure that all processes in the new 7.7 installation are stopped *on all nodes*, and that all processes in the old installation are running *on all nodes*.
2. Run `upgrade` on NodeC.
3. Shut down all processes in the old installation on all nodes.
4. Start up the processes in the new 7.7 installation on other nodes, especially the Admin Node Manager on NodeA, as this must be running before you can proceed to the next step.
5. Run `apply` on NodeC. If you ran `apply` previously on NodeC, this rerun triggers removal of NodeC entries in the topology on the Admin Node Manager on NodeA, before NodeC is registered.

You do not need to rerun commands on other nodes as a result of running `clean` on NodeC.

## API Gateway Analytics and metrics database upgrades

The following FAQs are specific to upgrading API Gateway Analytics and your metrics database.

### What should you upgrade first - API Gateway or API Gateway Analytics?

If you are upgrading from 7.4.0 and later versions, you should upgrade API Gateway Analytics before upgrading API Gateway using the `sysupgrade` command.

### Do you need to run `managedomain` to enable metrics?

If you are upgrading from a 7.4.0 or later API Gateway domain where the Node Managers are configured to write to the database, you do not need to run `managedomain` for each Node Manager. This is because the configuration is migrated when the API Gateways are upgraded.
