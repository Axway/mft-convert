---
title: Configure API Gateway Analytics
linkTitle: Configure API Gateway Analytics
weight: "2"
date: 2019-10-03
description: Update an API Gateway Analytics configuration (for example, the API Gateway Analytics port, database connection, and user credentials) before starting API Gateway Analytics. 
---
Use the `configureserver` script (recommended) to guide you through all the required steps, or use Policy Studio to configure the API Gateway Analytics configuration file.

## Prerequisites

The prerequisites for configuring API Gateway Analytics are as follows.

### Install API Gateway

Because API Gateway Analytics reports on transactions processed by API Gateway in real time, you must ensure that API Gateway is installed. For more details, see the [API Gateway Installation Guide](/docs/apim_installation/apigtw_install/).

To view API Gateway metrics in API Gateway Analytics, you must also enable the recording of metrics. For more details, see [Configure API Gateway with the metrics database](/docs/apimanager_analytics/metrics_gw_config).

### Install API Gateway Analytics

You must ensure that API Gateway Analytics is installed. For more details, see [Install API Gateway Analytics](/docs/apim_installation/apigtw_install/install_analytics/).

### Configure a database

You must ensure that a JDBC-compliant database is installed to store the API Gateway monitoring and transaction data. For more details, see [Install and configure a metrics database](/docs/apim_installation/apigtw_install/metrics_db_install/).

## Update API Gateway Analytics configuration

By default, API Gateway Analytics is configured to read message metrics from a MySQL database stored on the local machine. You can use the `configureserver` command to configure API Gateway Analytics to use an alternative database, change the user credentials on the default database connection, or use a different listening port.

### Update configuration on the command line

Perform the following steps to run `configureserver` in interactive mode:

1. Change to the following directory:

    ```
    INSTALL_DIR/analytics/posix/bin
    ```

2. Run the `configureserver` command.
3. Enter the port on which the API Gateway Analytics server will listen. Defaults to `8040`. If you have another process already using this port on the machine on which API Gateway Analytics is installed, configure API Gateway Analytics to listen on a different port.
4. Enter the database connection URL. Defaults to `jdbc:mysql://127.0.0.1:3306/reports`.
5. The following are examples of connection URLs for the supported databases, where `reports` is the name of the database and `DB_HOST` is the IP address or host name of the machine on which the database is running:
    * Oracle: `jdbc:oracle:thin:@DB_HOST:1521:reports`
    * Microsoft SQL Server: `jdbc:sqlserver://DB_HOST:1433;DatabaseName=reports;integratedSecurity=false;`
    * MySQL: `jdbc:mysql://DB_HOST:3306/reports`
    * MariaDB: `jdbc:mariadb://DB_HOST:3306/reports`
    * IBM DB2: `jdbc:db2://DB_HOST:50000/reports`
6. Enter the database user name. Defaults to `root`.
7. Enter the database password.
8. Enter whether API Gateway Analytics generates PDF-based reports. Defaults to `N`, which means that PDF reports are not generated. When set to `Y`, API Gateway Analytics generates PDF reports that include the same metrics displayed in the API Gateway Analytics window (for example, number of client requests, requests per service, and so on).
9. Enter the user name to connect to the API Gateway Analytics process that generates PDF reports. Defaults to an administrator user.

    {{< alert title="Note" color="primary" >}}This is not the operating system user. This is the user that connects to the API Gateway Analytics web server process, which generates the PDF reports. You can add new users under the **Environment Configuration > Users and Groups** node in Policy Studio.{{< /alert >}}

10. Enter the password to connect to the API Gateway Analytics process that generates PDF reports.
11. Enter the directory to which generated PDF reports are output (for example, `/home/reports`).
12. Enter whether to send generated PDF reports to email recipients. You will require an SMTP account with which to send the reports. Defaults to `N`.

The following command shows some example output in interactive mode:

```
/opt/Axway-7.7/analytics/posix/bin> configureserver
Connecting to configuration at : federated:file:////opt/Axway-7.7/analytics/conf/fed/configs.xml
Listening port [8040]:
Configuring Database: Default Database Connection
Database URL [jdbc:mysql://127.0.0.1:3306/reports]:
Database user name [root]:
Database password []: *****
Enable report generation (Y, N) [N]: y
Report generation process connects as user name [admin]:
Report generation process connects using password []: ********
Report output directory []: c:\reports
Email reports (Y, N) [N]: y
Default email recipient []: joe@example.com
Email from []: apigateway@axway.com
Choose SMTP connection type:
    0) None
    1) SSL
    2) TLS/SSL
Choice [0]:
SMTP host []: localhost
SMTP port [25]:
SMTP user name []: jbloggs
SMTP password []: *********
Delete report file after emailing (Y, N) [Y]:
Press enter to exit...
```

### Update configuration using command-line options

You can also run the `configureserver` command with various options (`--port`, `--dburl`, `--emailfrom`, `--emailto`, `--smtphost`, and so on). For example, the following command configures the database connection without emailing reports:

```
configureserver --dburl=jdbc:mysql://127.0.0.1:3306/reports --dbuser=root --dbpass=changeme --no-email
```

The following command specifies to email reports and the associated SMTP settings:

```
configureserver --dburl=jdbc:mysql://127.0.0.1:3306/reports --dbuser=root --dbpass=changeme –-email --emailto=joe@example.com --emailfrom=apigateway@axway.com --smtptype=NONE --smtphost=192.168.0.174 --smtpport=25 --smtpuser=jbloggs --smtppass=changeme     --generate --gpass=changeme --gtemp=c:\reports
```

For descriptions of all available options, enter the `configureserver --help` command.

{{< alert title="Note" color="primary" >}} The `configureserver` script does not permit the euro character (`€`) when specifying username and password options. However, you can specify the pound (`£`) and dollar (`$`) characters instead.{{< /alert >}}

### Update configuration in Policy Studio

The recommended way to configure API Gateway Analytics is using the `configureserver` command, which guides you through the required settings. However, you can also use the Policy Studio to configure specific settings in your API Gateway Analytics configuration file. For example, to configure the `reports` database, perform the following steps:

1. In your Policy Studio installation directory, run the `policystudio` command.
2. When Policy Studio starts up, select **File > New Project**.
3. In the New Project dialog, enter a name for the project and click **Next**.
4. Select **From existing configuration** and click **Next**.
5. Browse to the directory containing the API Gateway Analytics configuration file (`configs.xml`), for example:

    ```
        INSTALL_DIR/analytics/conf/fed/
    ```

6. Select **Environment Configuration > External Connections** in the Policy Studio tree, and expand the **Database Connections** tree node.
7. Right-click the **Default Database Connection** tree node, and select **Edit**.
8. The Database Connection dialog enables you to configure the database connection details. By default, the connection is configured to read metrics data from the `reports` database. Edit the details for the **Default Database Connection** on this dialog.

    For example, you should enter a non-default database user name and password. To connect to a database other than the default local database, right-click **Database Connections** in the tree, and select **Add a Database Connection**.

    You can verify that your database connection is configured correctly by clicking the **Test Connection** button on the Configure Database Connection dialog.

## Ensure that metrics have been enabled for your API Gateway host

You must use the `managedomain` tool to enable writing of metrics from the API Gateway instances on your host to the metrics database. This enables the Node Manager to process event logs from your API Gateway instances, and to write metrics data to the metrics database.

The following example uses the interactive `managedomain --menu` command:

```
Select option: 2
Select a host:
1) LinuxMint01
2) Enter host name
Enter selection from 1-2 [2]: 1
Hit enter to continue...
Enter a new host name [LinuxMint01]:
Enter a new Node Manager name [Node Manager on LinuxMint01]:
Enter a new Node Manager port [8090]:
There is only one Node Manager in this domain so it must remain as an Admin Node Manager
Do you want to create an init.d script for this Node Manager [n]:
Do you want to reset the passphrase for the Node Manager on this host ? [n]:
Do you wish to edit metrics configuration (y or n) ? [n]: y
Do you wish to enable metrics (y or n) ? [y]: y
Enter metrics database URL [jdbc:mysql://127.0.0.1:3306/reports]:
Enter metrics database username [root]:
Enter metrics database plaintext password [*******]:
Testing Database connectivity for : jdbc:mysql://127.0.0.1:3306/reports, user : root
Metrics database connectivity succeeded
Metrics generation enabled. All other specified metrics settings updated.
Metrics settings updated successfully. Please reboot Node Manager on completion of this program.
Completed successfully.
```

For more details, see [Configure API Gateway with the metrics database](/docs/apimanager_analytics/metrics_gw_config).
