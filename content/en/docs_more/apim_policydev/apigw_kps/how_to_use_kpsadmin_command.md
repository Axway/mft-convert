{
"title": "Manage KPS using kpsadmin",
  "linkTitle": "Manage using kpsadmin",
  "weight": "50",
  "date": "2020-01-06",
  "description": "Learn how to use the kpsadmin tool in interactive and scriptable command modes."
}
The `kpsadmin` command-line tool provides KPS management functions, independent of data source. For example, this includes KPS data backup, restore, encryption, and diagnostics.

The `kpsadmin` tool is especially useful in a development environment. In a production environment, you should also use data source-specific tools and administration procedures for data backup, restore, security, optimization, monitoring and so on.

{{< alert title="Caution" color="warning" >}}You must use `kpsadmin` operations with caution. Ensure that you have a verified backup before you run destructive operations such as clear, restore, and re-encrypt. You should always try out these options in a development environment first. Depending on data volumes, the key property store re-encryption or restore operations can take some time. These actions should be undertaken during a maintenance window. {{< /alert >}}

## Start `kpsadmin`

From a command prompt, enter `kpsadmin`. For example:

```
INSTALL_DIR/posix/bin/kpsadmin
```

If you do not specify a command operation (for example, `kpsadmin backup` or `restore`), `kpsadmin` enters its default interactive menu mode. For details on available operations in scriptable command mode, see [Run kpsadmin operations in scriptable command mode](#run-kpsadmin-operations-in-scriptable-command-mode).

In default interactive mode, you are first prompted to enter your admin credentials to authenticate to the Admin Node Manager. These are the credentials that you supplied when installing API Gateway.

### Start in verbose mode

To run `kpsadmin` in verbose mode, use the `-v` option. `kpsadmin` will then show all REST messages that are exchanged with API Gateway. This is useful for debugging. For example:

```
kpsadmin -v
```

For details on available options, enter `kpsadmin -h`, or see [kpsadmin command options](#kpsadmin-command-options).

## Select `kpsadmin` operations in interactive mode

This section describes the `kpsadmin` operations that are available in default interactive mode. When you first select an operation in interactive mode, you must enter the following:

* API Gateway group to use
* KPS Admin API Gateway in that group that handles KPS requests.

{{< alert title="Note" color="primary" >}}`kpsadmin` requires you to specify an API Gateway in a group. This is known as the *KPS Admin API Gateway*, which fields KPS requests only. Any API Gateway in the group can be used. For example, you might change this if you want to check that data is available from any API Gateway in the group.{{< /alert >}}

* KPS collection to use in the group
* KPS table to use in the collection

You can change this selection at any time.

### KPS table operations

The `kpsadmin` table operations are as follows:

* **Create Row**: Create a row in the selected table.
* **Read Row**  : Read a row by primary key in the selected table.
* **Update Row**: Update a row in the selected table. The row is specified by primary key.
* **Delete Row**: Delete a row in the selected table. The row is specified by primary key.
* **List Rows** : List all rows in the table.

### KPS table administration operations

The `kpsadmin` operations for table administration are as follows:

* **Clear**: Clear all rows in the table.
* **Backup**: Back up the table data. The generated backup UUID is required when restoring the data.
* **Restore**: Restore table data. The table must be empty before you restore.
* **Re-encrypt**: Re-encrypt encrypted data in the table. This option should only be used if group-level re-encryption fails.
* **Recreate**: Recreate a table. This is useful in development if you wish to change the table structure. This procedure involves dropping and recreating the table, so all existing data will be lost. The steps are as follows:

  1. Back up (optional): Backup the data if necessary using `kpsadmin`.
  2. Deploy the correct configuration: First redeploy the correct configuration using Policy Studio. This may result in some KPS deployment errors. The changes you have made may no longer match     the stored data structure.
  3. Recreate the table with the correct configuration: Select the Recreate option using `kpsadmin`.
  4. Restore (optional): Restore the data using `kpsadmin`. If you have made key or index changes, the data should import directly. If you have made more extensive changes (for example, renaming fields or changing types), you must upgrade the data to match the new table structure.
* **Table Details**: Display information about a table and its properties.

### KPS collection administration operations

The `kpsadmin` operations for collection administration are as follows:

* **Clear All** : Clear all data in all tables in the collection.
* **Backup All**: Back up all data in all tables in the collection.
* **Restore All**: Restore all data in all tables in the collection.
* **Re-encrypt All**: Re-encrypt all data in all tables in the collection. This option should only be used if group-level re-encryption fails.
* **Collection Details**: Display information about all tables in the collection.

### API Gateway group administration operations

The `kpsadmin` operations for API Gateway group administration are as follows:

* **Clear All**: Clear all data in all collections in the group.                                                          * **Backup All**: Back up all data in all collections in the group.
* **Restore All**: Restore all data in all collections in the group.
* **Re-encrypt All**: Re-encrypt all data in all collections in the group. Use this option when the encryption passphrase has been changed for the API Gateway group. The tables will be offline after a passphrase change. You must use this option to re-encrypt the data. You must enter the old API Gateway passphrase to proceed. Data is re-encrypted using the current API Gateway passphrase. You must restart the API Gateway instance(s) in the group.
* **Collection Details**: Display information about all collections in the group.

### Cassandra administration operations

The `kpsadmin` operations for Cassandra administration are as follows:

* **Show Configuration**: Show the current configuration for the KPS storage service (Apache Cassandra).
* **Run Diagnostic Checks**: Run diagnostic checks including HA configuration checks. You must specify if this is a single or multi-datacenter configuration.

### General administration operations

The `kpsadmin` operations for general administration are as follows:

* **Change Table**: Change the currently selected table.
* **Change Collection**: Change the currently selected collection.
* **Change Group or API Gateway** Refresh the configuration, and change the currently selected API Gateway group and KPS Admin API Gateway.
* **Debug Mode**: Enable or disable debug mode. To enable, you must enter the API Gateway group passphrase. Encrypted data in KPS tables is then shown in the clear. This can be useful for debugging issues on API Gateway.

### Example of switching a data source in interactive mode

This example shows how to switch from Cassandra storage to file storage.

#### Step 1: Backup collection data using `kpsadmin`

To copy the current data in the collection to the new data source, back up the collection data using `kpsadmin` option `21) Backup All`.

The backup UUID is highlighted in the following example:

![kpsadmin Backup All operation](/Images/APIGatewayKPSUserGuide/backupcollection.png)

#### Step 2: Create a new data source

To create the new data source, perform the following steps:

1. In the Policy Studio tree, select **Key Property Stores > Samples**.
2. Select the collection **Data Sources** tab.
3. Click **Add > Add File** at the bottom right.

   ![KPS collection Data Sources tab](/Images/APIGatewayKPSUserGuide/kpscollections.png)
4. Enter a file data source **Name** and **Description**.
5. Enter a **Directory Path** (for example, `${VINSTDIR/kps/samples`).

   {{< alert title="Tip" color="primary" >}}You can include `${VINSTDIR}` or `${VDISTDIR}` to indicate the gateway instance directory or install directory respectively. Make sure to use `/` on Linux. If the directory does not exist, it is automatically created.{{< /alert >}}
6. Select the collection **Properties** tab.
7. Change the collection **Default data source** to use the new data source:

   ![Default data source](/Images/APIGatewayKPSUserGuide/0300001C.png)
8. Click the **Deploy** button in the Policy Studio toolbar to deploy the configuration

#### Step 3: Restore collection data using `kpsadmin`

If you have made a backup in [Backup collection data using kpsadmin](#step-1-backup-collection-data-using-kpsadmin), to restore the collection data, perform the following steps:

1. Using `kpsadmin`, select option `22) Restore All`.
2. Enter the backup UUID noted in step 1.
3. Enter the KPS backup passphrase. Leave this blank if none was set.

![Restore collection data using kpsadmin](/Images/APIGatewayKPSUserGuide/restorecollection.png)

## Run `kpsadmin` operations in scriptable command mode

You can also control `kpsadmin` directly from the command line or from a script by specifying command operations (for example, `kpsadmin backup` or `restore`). If an operation is not specified, `kpsadmin` enters its default interactive menu mode.

You must also specify a user name and password, and an API Gateway group, KPS collection, or KPS table. For example:

```
./kpsadmin --username admin --password changeme --group "myGroup" --name "myGateway" backup
```

### `kpsadmin` command operations

The available `kpsadmin` command operations in this mode are:

| Operation                 | Description                                                                                                                                      |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| `clear`                   | Clear all data in the specified table, collection, or group.                                                                                     |
| `backup`                  | Back up all data in the specified table, collection, or group.                                                                                   |
| `restore`                 | Restore all data in the specified table, collection, or group.                                                                                   |
| `reencrypt`               | Re-encrypt all data in the specified table, collection, or group.                                                                                |
| `details`                 | Display information about the specified table, collection, or group.                                                                             |
| `list_rows`               | List all rows in the specified table, collection, or group.                                                                                      |
| `cassandra_configuration` | Show the current configuration for the KPS storage service (Apache Cassandra).                                                                   |
| `diagnostics`             | Run diagnostic checks including HA configuration checks. You must specify the `--mdc` option only when this is a multi-datacenter configuration. |

If this kind of `kpsadmin` command invocation succeeds, `0` is returned. If it fails, `1` is returned. This can be captured on Linux bash shell using `$?` (for example, `echo $?` will work on both platforms). On Linux, `0` means success, `1` means error.

You can specify the user name and password on the command line or using a secure script. For details on how to script user name and password input for API Gateway scripts, see the `managedomain` command reference at [managedomain command reference](/docs/apim_reference/managedomain_ref/)

### `kpsadmin` command options

The full `kpsadmin` command options are:

| Option                        | Description                                                                                  |
| ----------------------------- | -------------------------------------------------------------------------------------------- |
| `-h, --help`                  | Show help message and exit.                                                                  |
| `-u, --username=USERNAME`     | Specify the current Admin Node Manager username.                                             |
| `-p, --password=PASSWORD`     | Specify the current Admin Node Manager password.                                             |
| `-v, --verbose`               | Enable verbose mode for debugging. This shows all REST messages exchanged with API Gateway.  |
| `-g, --group=GROUP`           | Specify the API Gateway group to target.                                                     |
| `-n, --name=INSTANCE`         | Specify the KPS Admin API Gateway instance that fields all KPS requests for the group.       |
| `-c, --collection=COLLECTION` | Specify the KPS collection to target.                                                        |
| `-t TABLE, --table=TABLE`     | Specify the KPS table to target.                                                             |
| `--uuid=UUID`                 | Specify the UUID required when backing up or restoring data.                                 |
| `--mdc`                       | When using the `diagnostics` command, specify that this is a multi-datacenter configuration. |
| `--passphrase`                | KPS backup passphrase. Leave it blank if none was set.                                       |
| `--oldpassphrase`             | Old KPS passphrase. Leave it blank if none was set.                                       |

### Example `kpsadmin` scriptable commands

This section shows some example `kpsadmin` operations in scriptable command mode.

#### Back up and restore

To back up and restore an API Gateway group from a staging environment to a production environment, perform the following steps:

1. Specify the `kpsadmin backup` command:
2. You must copy the files from the staging `backup` directory to the production `backup` directory and note the UUID. This is output by `kpsadmin` and is also a prefix on the exported filenames.
3. Specify the `kpsadmin clear` command:
4. Specify the `kpsadmin restore` command with the UUID noted earlier and the KPS backup passphrase. Leave the passphrase blank if none was set.

#### Re-encrypt the KPS data

After an encryption passphrase change and deployment, you must re-encrypt the KPS data. To re-encrypt at the group level:

```
./kpsadmin --username admin --password changeme --oldpassphrase "" --group "Staging" --name "Gateway1" reencrypt
```

Leave the old passphrase blank if none was set.

#### Show KPS table details

To show table details:

```
./kpsadmin --username admin --password changeme --group "Staging" --name "Gateway1" --collection "My Collection" --table "My Table" details
```

#### Show KPS collection details

To show collection details:

```
./kpsadmin --username admin --password changeme --group "Staging" --name "Gateway1" --collection "My Collection" details
```

#### Show Apache Cassandra configuration

To show Cassandra configuration:

```
./kpsadmin --username admin --password changeme --group "Staging" --name "Gateway1" cassandra_configuration
```

#### Run diagnostics checks

To output diagnostic information for a group:

```
./kpsadmin --username admin --password changeme --group "Staging" --name "Gateway1" diagnostics
```

To output diagnostic information for a group in a Cassandra multi-datacenter system:

```
./kpsadmin --username admin --password changeme --group "Staging" --name "Gateway1" --mdc diagnostics
```

## Re-encrypt KPS data

You can use the `kpsadmin` re-encrypt option to re-encrypt previously encrypted KPS data. When you use this option, a backup of the data is first made in the following directory:

```
INSTALL_DIR/apigateway/groups/<group-id>/<instance-id>/conf/kps/backup
```

The data is decrypted with the old encryption passphrase, which you must supply. The data is then re-encrypted with the current encryption passphrase, which API Gateway already knows.

{{< alert title="Caution" color="warning" >}}You must use the re-encrypt option only when:

* The encryption passphrase has been changed for an API Gateway group configuration.
* This change has been deployed to all API Gateways in the group.
* You see `INFO` messages in all API Gateway trace logs as follows:

  ```
  INFO Loading KPS configuration.
  INFO Checking for passphrase changes...
  INFO Passphrase change has been detected for the following table(s).
  INFO Use kpsadmin to re-encrypt data and passphrase test.
  INFO Table(s) will remain in admin mode until this is done.
  ```

{{< /alert >}}

You should re-encrypt the data using the group-level `28) Re-encrypt All` interactive option, or the `kpsadmin --group reencrypt` scriptable command. Do not use the table or collection level re-encrypt options. These should only be used if group level encryption fails. You will then need to re-encrypt at collection and/or table level.

After re-encryption, you must restart all API Gateways in the group.