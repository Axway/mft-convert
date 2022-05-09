{
    "title": "sysupgrade command reference",
    "linkTitle": "sysupgrade command reference",
    "weight": 80,
    "date": "2019-10-07",
    "description": "Reference to `sysupgrade` commands."
}

To perform an upgrade, API Gateway provides a script called `sysupgrade`. This script is located in the following directory:

```
NEW_INSTALL_DIR/apigateway/upgrade/bin
```

{{< alert title="Note" color="primary" >}}For a description of all available command options and default settings, run `sysupgrade`
with the `--help`
option. You can also run each command with the`--help` option (for example, `sysupgrade`` export --help`).{{< /alert >}}

## `export` command

You must run the `export` command first. This contacts the old API Gateway installation using the old Admin Node Manager, and exports configuration data to the `export` output directory. The old installation Admin Node Manager can be running on the same node on which you are running the `export` command, or it can be running on a remote host.

This command also copies local files from the old installation into the `export` output directory. For this reason, it must run locally on each node in a multi-node system.

### `export` command rules

The following rules apply to the `export` command:

* You must run `export` first, and without errors, before running `upgrade`.
* The old installation of the API Gateway system must be running for the `export` command to succeed.
* In a multi-node system:
    * You must run `export` on all nodes, and before running `apply` on any node.
    * It does not matter which node you choose to run `export` on first.

### `export` command options

The following table summarizes the `export` command options:

| Option            | Description        | Required                                              |
|-------------------|-----------------|----------------------------------------------------------|
| `--help`            | Display help for the `export` command only.       | -      |
| `--old_install_dir` | Old API Gateway installation directory. For example:`/home/axway/Axway-7.4.1/apigateway`   | Yes  |
| `--anm_host`        | Specify the host name of the Admin Node Manager in the old API Gateway installation you are using to export the data. Specify the topology host name of the first Admin Node Manager host you upgrade, and use the same value on every node. | Only required if multiple Admin Node Managers in the topology being upgraded. |
| `--force`           | Force another export when the `export` command has already run successfully. When you specify `--force`, the output from all commands is cleaned and `export` runs again.   | No |
| `--username`        | Specify the Admin Node Manager user name.   | Yes (prompted if not specified).  |
| `--password`        | Specify the Admin Node Manager password.     | Yes (prompted if not specified).  |

### Sample `export` commands

On a single-node system, or a multi-node system with one Admin Node Manager, `export` is typically run as follows:

```
./sysupgrade export --old_install_dir /home/axway/Axway-7.3.1/apigateway
```

On a multi-node system with multiple Admin Node Managers, `export` is typically run as follows:

```
./sysupgrade export --old_install_dir /home/axway/Axway-7.3.1/apigateway --anm_host NodeA
```

To rerun `export` on a node where it has already run successfully (single-node or multi-node with one Admin Node Manager):

```
./sysupgrade export --old_install_dir /home/axway/Axway-7.3.1/apigateway --force
```

To run `export`and specify admin credentials on the command line (single-node or multi-node with one Admin Node Manager):

```
./sysupgrade export --old_install_dir /home/axway/Axway-7.3.1/apigateway --username admin --password my_text
```

### `export` command output

The `export` command outputs are as follows:

| Output                    | Location                                               |
|---------------------------|--------------------------------------------------------|
| `export` log file         | `Axway-7.7/apigateway/upgrade/bin/out/logs/export.log` |
| `export` output directory | `Axway-7.7/apigateway/upgrade/bin/out/export`          |

## `upgrade` command

You must run the `upgrade` command after `export`, and without errors, before running `apply`.

`upgrade` is an offline command that first performs validation on the exported data. After validation, it upgrades the exported data into a format suitable for import into an API Gateway version 7.7 installation. The upgraded data is written to the `upgrade` output directory (in a different location to the exported data).

If any warnings or errors occur during `upgrade`, you are prompted to examine the upgrade log file.

If any errors occur, you cannot run the `apply` command.

### `upgrade` command rules

The following rules apply to the `upgrade` command:

* You must first run `export` successfully on the local node before you can run `upgrade`.
* You must run `upgrade` on the local node without generating errors, before you can run `apply`.
* You do not need to have the old installation of API Gateway running to run `upgrade`, but you can leave the old API Gateway installation running to minimize downtime.
* In a multi-node system:
    * You must run `upgrade` on all nodes.
    * It does not matter on which node you run `upgrade` first.

### `upgrade` command options

The following table summarizes the `upgrade` command options:

| Option          | Description                      | Required                                           |
|-----------------|----------------------------------|----------------------------------------------------|
| `--help`          | Display help for the `upgrade` command only. | -     |
| `--force`         | Force another upgrade when the `upgrade` command has already run successfully. When you specify `--force`, the output from `upgrade` and `apply` (if you have already run it) are cleaned, and `upgrade` runs again. | No  |
| `--tracelevel`    | Level of logging output. Set to DEBUG, INFO, or VERBOSE to increase the output in the log files. The default level is INFO.  | No   |
| `--cass_host`    | Specify the address of the Apache Cassandra node that will receive data during the `apply` step.| No. Defaults to localhost.                         |
| `--cass_port`     | Specify the Apache Cassandra client port that will receive data during the `apply` step.  | No. Defaults to the Cassandra client default port. |
| `--cass_username` | Specify the Apache Cassandra user name. | No. Defaults to the Cassandra default user name.   |
| `--cass_password` | Specify the Apache Cassandra password.  | No. Defaults to the Cassandra default password.    |
| `--no_cassandra`  | If you specify this option, `sysupgrade` checks all group configurations for any Apache Cassandra-backed KPS collections. If any are found, you must update the old installation to remove them or move them to a non-Cassandra data source. You must then rerun `sysupgrade export` and `sysupgrade upgrade --no_cassandra`. If you do not specify this option, an Apache Cassandra host is added to all group configurations. The host details come from the options `--cass_host`, `--cass_port`, `--cass_username`, and `--cass_password`. If not specified, the host and port default to `localhost:9042`. | No. |

### Sample `upgrade` commands

On a single-node system, or a multi-node system with one or multiple Admin Node Managers, the `upgrade` command is typically run as follows:

```
./sysupgrade upgrade
```

You can also run this command for a standalone, single-node, Apache Cassandra setup (for example, in a development environment).

To upgrade on a node where the Apache Cassandra server is remote or listening on an address that is not `localhost`:

```
./sysupgrade upgrade --cass_host 192.0.2.0
```

To upgrade if you are not using Apache Cassandra:

```
./sysupgrade upgrade --no_cassandra
```

To rerun `upgrade` on a node where it has already run successfully:

```
./sysupgrade upgrade --force
```

### `upgrade` command output

The `upgrade` command outputs are as follows:

| Output                     | Location                                                 |
|----------------------------|----------------------------------------------------------|
| `upgrade`
 log file                    | `Axway-7.7/apigateway/upgrade/bin/out/logs/upgrade.log` |
| `upgrade` output directory | `Axway-7.7/apigateway/upgrade/bin/out/upgrade`          |

## `apply` command

The `apply` command first updates any databases used for OAuth or KPS (if an update is required). It then creates and starts the version 7.7 Node Manager on the local machine, followed by any API Gateway instances that run locally. `apply` also imports KPS data into the API Gateway instances if required.

In a single-node system, when `apply` runs successfully, `sysupgrade` is complete.

{{< alert title="Note" color="primary" >}}`apply` does not update the metrics database. For more details, see [Upgrade API Gateway Analytics](/docs/apim_installation/apigw_upgrade/upgrade_analytics/) or [Upgrade metrics database for API Manager](/docs/apim_installation/apigw_upgrade/upgrade_metrics/).{{< /alert >}}

In a multi-node system, you must run `apply` on each node. The first node that you run `apply` on must be an Admin Node Manager. When you run `apply` on all subsequent nodes, it registers new Admin Node Managers, Node Managers, and API Gateway instances using the version 7.7 Admin Node Manager running on the first node where the `apply` command ran. The first Admin Node Manager to be upgraded holds the domain CA private key and certificate.

{{< alert title="Tip" color="primary" >}}When `apply` runs on a subsequent node, it tries to clean any topology entries relating to itself using the first Admin Node Manager, if any entries exist. This ensures it can always be created successfully, even if `apply` ran previously on that node.{{< /alert >}}

### `apply` command rules

The following rules apply to the `apply` command:

* You must run `upgrade` successfully on the local node before you can run `apply`.
* The `apply` command completes the `sysupgrade` procedure on the local node.
* In a single-node system, you must shut down all processes in the old API Gateway installation before running `apply`.
* In a multi-node system:
    * You must run `apply` on all nodes.
    * The first node that `apply` runs on must be an Admin Node Manager.
    * You must shut down the old API Gateway installation on all nodes before running `apply` on any node.
    * `apply` must complete on the first Admin Node Manager node before you run `apply` on subsequent nodes. You can specify the first Admin Node Manager using `--anm_host`. You must use the same `--anm_host` value on each node. You are only required to specify `--anm_host` in a multi-node system with multiple Admin Node Managers.
    * Before you can run the `apply` command on all subsequent nodes, you must run the version 7.7 Admin Node Manager on the node you first ran `apply` on. This is the host specified by `--anm_host`. The `apply` command checks that the Admin Node Manager is running and is version 7.7.

### `apply` command options

The following table summarizes the `apply` command options:

| Option              | Description                  | Required                       |
|---------------------|------------------------------|--------------------------------|
| `--help`              | Display help for the `apply` command only. | -  |
| `--anm_host`          | Specify the topology host name of the first Admin Node Manager that you are upgrading. You must use the same value on every node.  | Only required if multiple Admin Node Managers in upgraded topology. |
| `--domain_passphrase` | Specify your API Gateway domain CA private key passphrase. You can also set this after upgrading with the `managedomain --change_domain_passphrase` option.| Yes (prompted if not specified). |
| `--sign_alg`          | Signing algorithm for topology SSL certificates in the new API Gateway system (for example, `sha1`, `sha256`, `sha384`, or `sha512`).| No. Defaults to `sha256`.   |
| `--subj_alt_name`     | Specify subject alternative names for Node Manager topology SSL certificates. You can specify multiple subject alternative names. For example: `--subj_alt_name "DNS.0=node1.example.com" --subj_alt_name "DNS.1=host1.example.com" --subj_alt_name "IP.0=10.4.6.7.3" --subj_alt_name "email.0=user1.example.com" --subj_alt_name "email.1=user1.example.com" --subj_alt_name "email.2=user1.example.com"` For more details, see the OpenSSL documentation: <https://www.openssl.org/docs/manmaster/apps/x509v3_config.html.> | No. Defaults to values suitable for the local host (for example, `DNS.1 = node1.axway.com, IP.1 = 10.4.5.6`). |
| `--force`             | Force another apply when the `apply` command has already run successfully. When you specify `--force`, the output from `apply` is cleaned and `apply` runs again.  | No   |
| `--username`         | Specify the Admin Node Manager user name.  | Yes (prompted if not specified).  |
| `--password`          | Specify the Admin Node Manager password. | Yes (prompted if not specified). |
| `--skip_db_upgrade`   | Skip the upgrade of Key Property Store (KPS) and OAuth tables in external databases. Only use this option if a database upgrade has already been performed (for example, by a database administrator). | No |
| `--tracelevel`        | Level of logging output. Set to `DEBUG`, `INFO`, or `VERBOSE` to increase the output in the log files. The default level is `INFO`.  | No   |

### Sample `apply` commands

On a single-node system, or a multi-node system with one Admin Node Manager, the `apply` command is typically run as follows:

```
./sysupgrade apply
```

On a multi-node system with multiple Admin Node Managers, the `apply` command is typically run as follows:

```
./sysupgrade apply --anm_host NodeA
```

To rerun `apply` on a node where it has already run successfully (single-node or multi-node with one Admin Node Manager):

```
./sysupgrade apply --force
```

To run `apply`and specify admin credentials on the command line (single-node or multi-node with one Admin Node Manager):

```
./sysupgrade apply --username admin --password my_text1 --domain_passphrase my_text2
```

To run `apply` and specify a non-default signing algorithm for the topology SSL certificates (single-node or multi-node with one Admin Node Manager):

```
./sysupgrade apply --sign_alg sha512
```

To run `apply` and specify non-default subject alternative names and a non-default CN for the Node Manager topology SSL certificate (single-node or multi-node with one Admin Node Manager):

```
./sysupgrade apply --subj_alt_name "DNS.0=node1.axway.com" --subj_alt_name "DNS.1=host1.axway.com" --subj_alt_name "IP.0=10.4.6.7.3" --subj_alt_name "email.0=user1.axway.com" --subj_alt_name "email.1=user1.axway.com" --subj_alt_name "email.2=user1.axway.com" --domain_name MyTestDomain
```

### `apply` command output

The `apply` command outputs are as follows:

| Output                   | Location                                               |
|--------------------------|--------------------------------------------------------|
| `apply` log file         | `Axway-7.7/apigateway/upgrade/bin/out/logs/apply.log` |
| `apply` output directory | `Axway-7.7/apigateway/upgrade/bin/out/apply`          |

## `status` command

The `status` command gets the local node status for `sysupgrade`, For example, which commands (`export`, `upgrade`, and `apply`) have run, and which command should be run next.

{{< alert title="Note" color="primary" >}}Before commencing any step, it is recommended that you run the `status` command. This is particularly important in a multi-node upgrade where it is easy to lose track of what step or machine you are on.{{< /alert >}}

For example, the output of the `status` command for each stage of the upgrade process is as follows:

```
./sysupgrade status
System Upgrade Status
---------------------
Current status: not started
Next step: sysupgrade export

./sysupgrade export –-old_install_dir=/path/to/old/apigateway
./sysupgrade status
System Upgrade Status
---------------------
Current status: export complete
Next step: sysupgrade upgrade

./sysupgrade upgrade
./sysupgrade status
System Upgrade Status
---------------------
Current status: export and upgrade complete
Next step: sysupgrade apply


./sysupgrade apply
./sysupgrade status
System Upgrade Status
---------------------
Current status: system upgrade complete
```

## `clean` command

The `clean` command enables you to restart the `sysupgrade` process. The command resets the new API Gateway 7.7 installation to factory settings. Any exported data, upgraded data, or recreated topology is moved to a backup directory. You can then rerun all the commands without using the `--force` option.

The `clean` command does the following:

1. Moves the `sysupgrade` output from the `Axway-7.7/apigateway/upgrade/bin/out` directory to a backup directory.
2. Removes the `Axway-7.7/apigateway/groups` directory from the new API Gateway 7.7 installation.
3. Moves Node Manager directories to a backup directory:
    * `apigateway/conf` is moved to `apigateway/upgrade_backup/old-nm_conf-<date>_<time>`
    * `apigateway/system/conf` is moved to `apigateway/upgrade_backup/old-nm_systemconf-<date>_<time>`
    * `apigateway/ext/lib` is backed up to `apigateway/upgrade_backup/old-nm_extlib-<date>_<time>`
4. Restores the original Node Manager directories:
    * `apigateway/conf` is restored from `apigateway/upgrade_backup/original_nm_conf` (but existing `apigateway/conf/licenses` is retained)
    * `apigateway/system/conf` is restored from `apigateway/upgrade_backup/original_nm_systemconf`
    * `apigateway/ext/lib` is restored from `apigateway/upgrade_backup/original_nm_extlib`

When you run `clean` for the first time:

* Original `apigateway/conf` is backed up to: `apigateway/upgrade_backup/original_nm_systemconf`
* Original `apigateway/system/conf` is backed up to: `apigateway/upgrade_backup/original_nm_systemconf`
* Original `apigateway/ext/lib` is backed up to: `apigateway/upgrade_backup/original_nm_extlib`

### `clean` command rules

The following rules apply to the `clean` command:

* You do not have to run any other step before running `clean`.
* In a multi-node upgrade, you must run `clean` on all nodes to clean the entire system.

### `clean` command options

The following table summarizes the `clean` command options:

| Option                 | Description          | Required |
|------------------------|----------------------|----------|
| `--help`                     | Display help for the `clean` command only.                    | -     |
| `--tracelevel`               | Level of logging output. Set to `DEBUG`, `INFO`, or `VERBOSE` to increase the output in the log files. The default level is `INFO`.       | No       |

### Sample `clean` command

On a single-node system, or a multi-node system with one or multiple Admin Node Managers, the `clean` command is typically run as follows:

```
./sysupgrade clean
```

## Get help on sysupgrade commands

For a description of all available command options and default settings, run `sysupgrade` with the `--help` option.

You can also run each command with the`--help` option. For example, `sysupgrade export --help`, `sysupgrade upgrade --help`, and so on.

## Further information

For warnings and errors generated by `sysupgrade` script, see [sysupgrade error reference](/docs/apim_installation/apigw_upgrade/upgrade_errors/).