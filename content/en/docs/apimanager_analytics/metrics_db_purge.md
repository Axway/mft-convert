---
title: Purge the metrics database
linkTitle: Purge the metrics database
weight: "5"
date: 2019-10-07
description: Use the `dbpurger` command to connect to your metrics database and purge old data. This command also enables you to retain a specified amount of data, and to archive all data.
---

## Run the dbpurger command

For API Gateway Analytics metrics, you can run the `dbpurger` command from the following directory:

```
INSTALL_DIR/analytics/posix/bin
```

For API Gateway and API Manager metrics, you can run the `dbpurger` command from the following directory:

```
INSTALL_DIR/apigateway/posix/bin
```

### dbpurger options

You can specify the following options to the `dbpurger` command:

| Option                                   | Description    |
|------------------------------------------|--------------------------------------------|
| `-h, --help`                             | Displays help message and exits. |
| `-p PASSPHRASE, --passphrase=PASSPHRASE` | Specifies the configuration passphrase (leave blank for zero length).   |
| `--dbname=DBNAME`                        | Specifies the database name (mutually exclusive with `dburl`, `dbuser`, and `dbpass` options). |
| `--dburl=DBURL`                          | Specifies the database URL.     |
| `--dbuser=DBUSER`                        | Specifies the database user.   |
| `--dbpass=DBPASS`                        | Specifies the database passphrase.  |
| `--archive`                              | Archive all data.  |
| `--out=OUT`                              | Archive all data in the specified directory.   |
| `--purge`                                | Purge data from the database. You must also specify the `--retain` option.   |
| `--retain=RETAIN`                        | Specifies the amount of data to retain (for example, `30days`, `1month`, or `1year`). You must specify this option with the `--retain` option. |

## Example dbpurger commands

This section shows examples of running `dbpurger` in default interactive mode and of specifying command-line options.

### Run dbpurger in interactive mode

The following example shows the output when running the `dbpurger` command in interactive mode. This example archives all data, retains three months of data, and purges older data from the database:

```
>dbpurger
Choosing:Default Database Connection
Archive database (Y, N) [N]:y
Archive path [./archive]:Purge an amount of data from the database (Y, N) [N]:y
Amount of data to retain (e.g. 1year, 3months, 7days) [3months]:
Wrote archive:./archive/process_groups.xml
Wrote archive:./archive/processes.xml
Wrote archive:./archive/metric_types.xml
Wrote archive:./archive/audit_log_sign.xml
Wrote archive:./archive/time_window_types.xml
Wrote archive:./archive/audit_log_points.xml
Wrote archive:./archive/audit_message_payload.xml
Wrote archive:./archive/transaction_data.xml
Wrote archive:./archive/metric_groups.xml
Wrote archive:./archive/metric_group_types.xml
Wrote archive:./archive/metrics_alerts.xml
Wrote archive:./archive/metrics_data.xml
Purging data older than:Wed Jun 27 15:26:00 BST 2012
Purging table:audit_log_sign... deleted 0 rows
Purging table:transaction_data... deleted 0 rows
Purging table:audit_message_payload... deleted 7 rows
Purging table:audit_log_points... deleted 16 rows
Purging table:metrics_alerts... deleted 4 rows
Purging table:metrics_data... deleted 703 rows
```

### Specify dbpurger command options

The following example shows the output when specifying options the `dbpurger` command. This example retains 30 days of data, and purges older data from the database:

```
dbpurger --dburl=jdbc:mysql://127.0.0.1:3306/reports --dbuser=root --dbpass=fred --purge --retain=30days
```

You can run `dbpurger` without a password by specifying the name of the database connection. For example:

```
dbpurger --dbname="Default Database Connection" --archive --out=archive.dat
```
