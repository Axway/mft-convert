{
    "title": "Troubleshoot an upgrade",
    "linkTitle": "Troubleshooting",
    "weight": 150,
    "date": "2019-10-07",
    "description": "Advice on troubleshooting the API Gateway upgrade process and the `sysupgrade` commands."
}

## Out of memory error when running upgrade

Problem
: When upgrading a large configuration the upgrade command fails with an out of memory error.

Solution
: Some configurations are very large and need memory to be increased above the default values.

To increase the memory, perform the following steps:

1. Edit the `-Xmx` setting in the `apigateway/system/conf/jvm.xml` file to specify a value (for example, `2048`) instead of the default. The value required depends on the size of your configuration.

    The following example shows how to change it to `2048`.

    Default `-Xmx` value:

    ```
    <if property="maxHeap">
    <VMArg name="-Xmx${maxHeap}"/>
    </if>
    ```

    `-Xmx` value of `2048`:

    ```
    <if property="maxHeap">
    <VMArg name="-Xmx2048"/>
    </if>
    ```

2. Rerun the `upgrade` command.

## Apply was run before export was run on all nodes

Problem
: You want to upgrade the remaining nodes in a multi-node domain, but you have already run `export`, `upgrade`, and `apply` on the first Admin Node Manager in the domain.

Solution
: In this case, you did not follow the rule that `export` must run on all nodes in a multi-node domain before `apply` is run on any node.

This means that you have API Gateway processes from the new installation running on the first Admin Node Manager, (for example, NodeA), but have other nodes (for example, NodeB and NodeC) that are still running API Gateway processes from the old installation.

To upgrade NodeB and NodeC, perform the following steps:

1. Shut down the API Gateway processes in the new installation on NodeA and then start up the API Gateway processes in the old installation on NodeA.
2. Ensure that the API Gateway processes from the old installation are still running on NodeB and NodeC.
3. Run `export` on NodeB.
4. Run `upgrade` on NodeB.
5. Run `export` on NodeC.
6. Run `upgrade` on NodeC.
7. Shut down the API Gateway processes in the old installation on all nodes.

    {{< alert title="Tip" color="primary" >}}If you need to bring up the processes in the new installation on NodeA more quickly, you can shut down the processes in the old installation on all nodes after `export` is run on NodeB and NodeC.{{< /alert >}}

8. Start the API Gateway processes on the new 7.7 installation on NodeA.
9. Run `apply` on NodeB.
10. Run `apply` on NodeC.

## Group inconsistency errors

Problem
: Group inconsistency error at export, but groups are not inconsistent in the old installation.

Solution
: This error occurs if a remote Node Manager is down. Restart the remote Node Manager and rerun the `export` command.

## KPS data missing after upgrade

Problem
: After running `apply` on all nodes, some KPS data appears to be missing.

Solution
: Use the `import-kps` command. This command is available in your API Gateway installation directory (for example, `/opt/Axway-7.7/apigateway/upgrade/bin`).

The following table summarizes the `import-kps` command options:

| Option                 | Description         | Required                                  |
|------------------------|---------------------|-------------------------------------------|
| `--help`               | Display help for the `import-kps` command only. | - |
| `--anm_host`           | Specify the topology host name of the Admin Node Manager in the old API Gateway installation you are using to export the data. We recommend that you specify the first Admin Node Manager host that you are upgrading, and you must use the same value on every node. | Only required if multiple Admin Node Managers in upgraded topology. |
| `--username`           | Specify the Admin Node Manager user name. | Yes (prompted if not specified). |
| `--password`           | Specify the Admin Node Manager password. | Yes (prompted if not specified). |

## API Gateways missing from topology after upgrade

Problem
: You ran `apply` successfully on all nodes, but API Gateways are missing from your topology.

Solution
: If you are upgrading a topology with multiple Admin Node Managers, you must pass the host name of the first Admin Node Manager you upgraded to the `apply` command *on all nodes*.

For example, if you have three nodes as follows:

* **NodeA**: Admin Node Manager and API Gateway1 in Group1
* **NodeB**: Node Manager and API Gateway1A in Group1
* **NodeC**: Admin Node Manager and API Gateway2 in Group2

If you run `apply` incorrectly as follows:

* **NodeA**: `sysupgrade apply --anm_host NodeA`
* **NodeB**: `sysupgrade apply --anm_host NodeA`
* **NodeC**: `sysupgrade apply --anm_host NodeC`

Everything appears to upgrade successfully. However, you now have two separate domains in your new 7.7 installation. If you log in to `https://NodeA:8090`, you will see API Gateway1 and API Gateway1A. If you log in to `https://NodeC:8090`, you will see API Gateway2.

To resolve this issue, follow these steps:

1. Stop all API Gateway processes in the new installation on NodeC.
2. Rerun `apply` on NodeC as follows:

    ```
    sysupgrade apply --anm_host NodeA
    ```

3. When this command completes, NodeC joins the domain where NodeA is the first Admin Node Manager.

{{< alert title="Tip" color="primary" >}} Rerunning apply on NodeC is the easiest solution in this case. Alternatively, you could rerun `apply ``--anm_host NodeC` on NodeA and on NodeB.{{< /alert >}}
