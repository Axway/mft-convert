{
    "title": "Upgrade metrics database for API Manager",
    "linkTitle": "Upgrade metrics database for API Manager",
    "weight": 50,
    "date": "2019-10-07",
    "description": "Upgrade your metrics database to version 7.7."
}

If you are using a metrics database with an earlier version of API Gateway for monitoring in API Manager or third party tools, you must follow the steps in this section to upgrade the database. Data in your old metrics database will be preserved and the upgraded database will be fully compatible with version 7.7.

{{< alert title="Note" color="primary" >}}For details on upgrading a metrics database for use with API Gateway Analytics, see [Upgrade API Gateway Analytics](/docs/apim_installation/apigw_upgrade/upgrade_analytics/) instead.{{< /alert >}}

For frequently asked questions about upgrading your metrics database, see [API Gateway Analytics and metrics database upgrades](/docs/apim_installation/apigw_upgrade/upgrade_faq/#api-gateway-analytics-and-metrics-database-upgrades).

## Summary of steps

The following summarizes the steps to upgrade your metrics database, depending on what version you are upgrading from.

1. Back up your old metrics database.
2. Run `managedomain` to enable metrics for Node Managers, if this is not already done. You must also run `managedomain` to update the database URL if you created a new database (for example, as part of a rollback strategy).

## Rollback strategy

If you want to be able to revert back to your old version of API Gateway, the best approach is to create a new metrics database for version 7.7. The old versions can then be relaunched without changes. Where appropriate, this topic details additional tasks that you need to perform to implement this rollback strategy.

## Back up the metrics database in your old installation

You must backup the metrics database being used in the old installation before upgrading. For more details on backing up your database, see your database user documentation.

### SQL Restore for rollback

To roll back, you must perform the relevant SQL Restore operation to overwrite the newly created blank database (for example, `MetricsNew`) with the data and schema from the old metrics database.

## Enable metrics using managedomain

{{< alert title="Note" color="primary" >}}If you have created a new database as part of a rollback strategy (for example, `MetricsNew`), you must perform this step to specify the database URL of the newly created database to `managedomain`.{{< /alert >}}

If you have not already done so, you must use the `managedomain` tool to enable the Node Manager to process event logs from your API Gateway host, and to write metrics data to the metrics database.

All API Gateway instances running on the host node generate transaction event log files. These files are all written to the same folder, and are collectively processed and aggregated by the Node Manager on the host, and then written to the metrics database. The metrics database provides the data for the graphical charts in the **Monitoring** view in API Manager, or for third-party tools such as Splunk.

To enable metrics in the new installation:

1. You must ensure that the new API Gateway service is running. Additionally, if you have a multi-node domain with multiple Node Managers, you must ensure that metrics is enabled for each Node Manager.
2. Enable metrics using the `managedomain` utility in the following directory:

    ```
    INSTALL_DIR/apigateway/posix/bin
    ```

    The following example shows how to modify the host to enable metrics and specify the database settings:

    ```
    ./managedomain --edit_host  --metrics_enabled y --metrics_dburl jdbc:mysql://127.0.0.1:3306/Metrics --metrics_dbuser root --metrics_dbpass secretpass
    ```

    The following is an example of the output:

    ```
    There is only one Node Manager in this domain so it must remain as an Admin Node Manager
    Testing Database connectivity for : jdbc:mysql://127.0.0.1:3306/Metrics, user : root
    Metrics database connectivity succeeded
    Metrics generation enabled. All other specified metrics settings updated.
    Metrics settings updated successfully. Please reboot Node Manager on completion of this program.
    Completed successfully.
    ```

3. Restart the Node Manager service. When the Node Manager service has restarted, the upgrade of metrics database is complete.

### managedomain options

The following table describes the `managedomain` options used:

| Option                 | Description                                                                                |
|------------------------|--------------------------------------------------------------------------------------------|
| `--edit_host`                     | Modify the host settings.                                                 |
| `--metrics_enabled`                    | Flag to enable or disable metrics. Set to `y` to enable metrics, or set to `n` to disable. |
| `--metrics_dburl`                    | The JDBC connection string for the metrics database.            |
| `--metrics_dbuser`                    | The database user name.                                                  |
| `--metrics_dbpass`                    | The password associated with the database user name.                  |

### Database URL for rollback

If you have created a new database as part of a rollback strategy, the database URL must contain the name of the new database (for example, `jdbc:mysql://127.0.0.1:3306/MetricsNew`).

## Perform a rollback

If you have implemented the rollback strategy described in the preceding sections, follow these steps to revert back to the old version of API Gateway:

1. Stop the API Gateways and the Node Managers in the new installation.
2. Restart the API Gateways and the Node Managers in the old installation.
