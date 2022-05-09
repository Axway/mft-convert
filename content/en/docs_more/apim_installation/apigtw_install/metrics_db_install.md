{
"title": "Install and configure a metrics database",
  "linkTitle": "Install and configure a metrics database",
  "weight": "26",
  "date": "2019-10-07",
  "description": "Create and configure a database for monitoring in API Gateway Analytics, API Manager, and third-party tools."
}
API Gateway stores and maintains monitoring and transaction data in a JDBC-compliant database, which can be read by API Gateway Analytics, API Manager, and third-party monitoring tools.

## Prerequisites

The prerequisites for setting up the database are as follows.

### Install a third-party JDBC database

You must install a JDBC-compliant database to store the monitoring and transaction data. Axway provides setup scripts for the following databases:

* MySQL
* MariaDB
* Microsoft SQL Server
* Oracle
* IBM DB2

You must ensure that you have the correct credentials to execute the setup scripts and to access the database for operations on the tables created by the scripts.

For details on supported database versions, see [Databases](/docs/apim_installation/apigtw_install/system_requirements/#databases). For details on how to install your chosen third-party JDBC database, see your database product documentation.

### Install API Manager or API Gateway Analytics

* You must install API Manager to use it to view the monitoring data in the metrics database. For more details, see [Install API Manager](/docs/apim_installation/apigtw_install/install_api_mgmt).
* You must install API Gateway Analytics to use it to view the monitoring data in the metrics database. For more details, see [Install API Gateway Analytics](/docs/apim_installation/apigtw_install/install_analytics/).

You do not need to install API Gateway Analytics to view monitoring data in API Manager only.

## Add third-party JDBC driver files

You must add the JDBC driver files for your chosen third-party database to your API Gateway and Policy Studio installations as appropriate.

If you are using MariaDB, you must use the MariaDB JDBC driver (MariaDB Connector/J 2.7.2) with the MariaDB database connection URL, for example, `jdbc:mariadb://DB_HOST:3306/reports`.

If you are using MySQL 8, only the later 5.1.x series of JDBC drivers (from 5.1.47 upwards) will work. If you wish to use a later MySQL JDBC driver version, for example, 8.x, then the MySQL Driver class name in the JDBC Drivers section of the entity store must be updated from `org.gjt.mm.mysql.Driver` to `com.mysql.cj.jdbc.Driver`.

### Add JDBC drivers to API Gateway

To add the third-party JDBC driver files for your database to API Gateway, perform the following steps:

1. Add the binary files for your database driver as follows:

   * Add `.jar` files to `INSTALL_DIR/apigateway/ext/lib`
   * Add `.so` files to the `INSTALL_DIR/apigateway/platform/lib`
2. Restart API Gateway.

### Add JDBC drivers to Policy Studio

To add third-party binaries to Policy Studio, perform the following steps:

1. Select **Window > Preferences > Runtime Dependencies** from the Policy Studio main menu.
2. Click **Add** to select a JAR file to add to the list of dependencies.
3. Click **Apply** when finished. A copy of the JAR file is added to the `plugins` directory in your Policy Studio installation.
4. Click **OK**.
5. Restart Policy Studio using the `policystudio -clean` command.

### Add JDBC drivers to API Gateway Analytics

To add the third-party JDBC driver files for your database to API Gateway Analytics
(if installed), perform the following steps:

1. Add `.jar` files to the `INSTALL_DIR/analytics/ext/lib` directory.
2. Add `.so` files to the `INSTALL_DIR/analytics/platform/lib` directory.
3. Restart API Gateway Analytics.

## Create the third-party database

API Gateway Analytics and API Manager monitoring read message metrics from a third-party JDBC database and display this information in a visual format to administrators. This is the same database in which API Gateway stores its message metrics and audit trail data. You must first create this database using the third-party database of your choice:

* MySQL
* MariaDB
* Microsoft SQL Server
* Oracle
* IBM DB2

For details on how to do this, see the product documentation for your chosen third-party database. The following example shows creating a MySQL or MariaDB database:

```
mysql> CREATE DATABASE reports;
Query OK, 1 row affected (0.00 sec)
```

In this example, the metrics database is named `reports`, but you can use any appropriate name.

### Set transaction isolation to READ COMMITTED

For all supported databases, to ensure atomicity and consistency, you must ensure that the transaction isolation level is set to `READ COMMITTED`. This setting is recommended whether you are installing for the first time or upgrading.

{{< alert title="Note" color="primary" >}}Read-committed transaction isolation mode is the default mode for Oracle, Microsoft SQL Server and IBM DB2, but not for MySQL or MariaDB. If you are using MySQL or MariaDB, you must change to read-committed transaction isolation mode after installation and before you start the server for the first time.{{< /alert >}}

For more details, see the product documentation for your chosen third-party database.

## Configure the database connection for API Gateway Analytics

When you have created the metrics database, you must update your API Gateway Analytics configuration with the database details using the `configureserver` script. For more details, see [Configure API Gateway Analytics](/docs/apimanager_analytics/analytics_config).

## Configure the database connection for API Manager

To configure the API Gateway external connection to the database in Policy Studio, select **Environment Configuration > External Connections > Database Connections > Add a Database Connection**. For details, see [Configure the database connection](/docs/apim_policydev/apigw_external_connections/common_db_conf/#configure-the-database-connection).

## Set up the database tables

When you have created the metrics database and configured the database connection, the next step is to set up the database tables.

For API Gateway Analytics monitoring run the `dbsetup` command from the following API Gateway directory:

```
INSTALL_DIR/analytics/posix/bin
```

For API Manager monitoring, run the `dbsetup` command from the following API Gateway directory:

```
INSTALL_DIR/apigateway/posix/bin
```

The following example command shows setting up new database tables:

```
$ dbsetup
New database
Schema successfully upgraded to:002-leaf
```

When you specify command-line arguments to `dbsetup`, the script does not run interactively. Run `dbsetup` without any options to create the database tables.

## Specify options to dbsetup

You can specify the following options to the `dbsetup` command:

| Option                                   | Description                                                                                                                                                                                                                 |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-h, --help`                             | Displays help message and exits.                                                                                                                                                                                            |
| `-p PASSPHRASE, --passphrase=PASSPHRASE` | Specifies the configuration passphrase (blank for zero length).                                                                                                                                                             |
| `--dbname=DBNAME`                        | Specifies the database name (mutually exclusive with `--dburl`,`--dbuser`, and `--dbpass`).                                                                                                                                 |
| `--dburl=DBURL`                          | Specifies the database URL.                                                                                                                                                                                                 |
| `--dbuser=DBUSER`                        | Specifies the database user.                                                                                                                                                                                                |
| `--dbpass=DBPASS`                        | Specifies the database password. You must enclose passwords that contain special characters in single quotation marks. For example: `./dbsetup --dburl=mysql://127.0.0.1:3306/reports --dbuser=root --dbpass='AcmeCorp!23'` |
| `--reinstall`                            | Forces a reinstall of the database, dropping all data.                                                                                                                                                                      |
| `--stop=STOP`                            | Stops the database upgrade after the named upgrade.                                                                                                                                                                         |

### dbsetup examples

The following are some examples of using `dbsetup` command options.

#### Connect to a named database

You can use the `--dbname` option to connect to a named database connection configured under the **External Connections** node in the Policy Studio tree. For example:

```
$ dbsetup --dbname=Oracle
Current schema version:001-initial
Latest schema version:002-leaf
Schema successfully upgraded to:002-leaf
```

#### Connect to a database URL

You can use the `--dburl` option to manually connect to a database instance directly using a URL. For example:

```
$ dbsetup --dburl=jdbc:mysql://localhost/reports --dbuser=root --dbpass=admin
Current schema version:001-initial
Latest schema version:002-leaf
Schema successfully upgraded to:002-leaf
```

Example: Setup for TLS with MySQL Server.

JDBC drivers can support additional parameters as part of the JDBC URL string. Which options are supported by a respective JDBC driver depends on the vendor and version.

In a MySQL database, the TLS parameters used by a client, like API Manager or Node Manager, to connect to a server can be enforced to ensure that database connections will only be established if the database server offers the required capabilities.

The following example JDBC string shows a TLS configuration for AWS RDS MySQL to require TLS 1.2. The server certificate must not be checked for a valid trust chain:

```
jdbc:mysql://dba3s-np-rds-apimgateway-dev-int-mysql.cpqdyezqyf7p.eu-central-1.rds.amazonaws.com:3306/apimetrics?useSSL=true&requireSSL=true&verifyServerCertificate=false
```

For more information, see [Connection URL Syntax](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-reference-url-format.html) and [Configuration Properties for Connector/J - Security](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-connp-props-security.html).

#### Install a database

You can also use the `--dburl` option to set up a newly created database instance where none already exists. For example:

```
$ dbsetup --dburl=jdbc:mysql://localhost/reports --dbuser=root --dbpass=admin
New database
Schema successfully upgraded to:002-leaf
```

#### Reinstall a database

You can use the `--reinstall` option to wipe and reinstall a database. For example:

```
$ dbsetup --dburl=jdbc:mysql://localhost/reports --dbuser=root --dbpass=admin --reinstall
Re-installing database...
Schema successfully upgraded to:002-leaf
```

## SQL database schema scripts

As an alternative to using the `dbsetup` command, API Gateway also provides separate SQL schema scripts to set up the database tables for each of the supported databases. However, these scripts set up new tables only, and do not perform any upgrades of existing tables. These scripts are provided in the `INSTALL_DIR/analytics/system/conf/sql` or `INSTALL_DIR/apigateway/system/conf/sql` directory in the following sub-directories:

* `/mysql`
* `/mariadb`
* `/mssql`
* `/oracle`
* `/db2`

You can run the SQL commands in the `analytics.sql` file in the appropriate directory for your database. The following example shows creating the tables for a MySQL database:

```
mysql> \. INSTALL_DIR/apigateway/system/conf/sql/mysql/analytics.sql
Query OK, 0 rows affected, 1 warning (0.00 sec)
Query OK, 0 rows affected, 1 warning (0.00 sec)
...
```