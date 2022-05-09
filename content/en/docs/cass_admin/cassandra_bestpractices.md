---
title: Apache Cassandra best practices
linkTitle: Best practices
weight: 3
date: 2019-06-05T00:00:00.000Z
description: >
  Follow the best practices in this section to achieve a stable Apache Cassandra
  environment, and to prevent data integrity and performance issues.
---
## Before you start

Complete all of these tasks before you start Apache Cassandra.

### Clock synchronization and health check

The clocks of the system across all Cassandra cluster machines and the clocks of all client machines (API Gateway hosts) must be synchronized to one (1) millisecond precision.

Failing to synchronize the clocks will result in:

* Faults in data synchronization.
* Failure to start or configure the machines correctly.

The clock synchronization requires the use of a time service, such as NTP (Network Time Protocol), to ensure that the time is synchronized across all machines in the cluster.

You must also perform a health check of the clock drift between nodes on a regular basis.

### User account

* You must create a specific UNIX user account for the Cassandra database.
* This user account must own all Cassandra related files, and it must be used to run the Cassandra process.
* This guide refers to this user account as `cassandra_user`.

### Required system tuning

Cassandra executes basic performance and tuning checks at startup, and it writes warning messages to the console and to the system log file when issues are found.

These are examples of warnings for a misconfigured Cassandra host:

```
WARN Unable to lock JVM memory (ENOMEM). This can result in part of the JVM being swapped out, especially with mmapped I/O enabled. Increase RLIMIT_MEMLOCK or run Cassandra as root.
WARN jemalloc shared library could not be preloaded to speed up memory allocations.
WARN Cassandra server running in degraded mode. Is swap disabled? : true, Address space adequate? : true, nofile limit adequate? : false, nproc limit adequate? : false
```

Perform the following procedures to ensure that each Cassandra machine meets the basic tuning requirements.

{{< alert title="Note" color="primary" >}}
The following commands apply for Red Hat 7.x. If you are using another Linux distribution, consult your system administrator.
{{< /alert >}}

#### Install jemalloc

Ensure that `jemalloc` is installed.

1. Run the command `rpm -q jemalloc` to check if jemalloc is installed.
2. If jemalloc is not installed, run the following command to install it from `epel`:

```
sudo rpm -iv https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum install -y jemalloc
```

#### Turn swap off

Ensure that you turn `swap` off.

Apache Cassandra recommends that `swap` is disabled. If your company policies or production environment requires `swap` to be `on` for other processes, you must ensure that the Cassandra process is not `swapped out` at any time.

1. The line `cat/proc/swaps` in the `/etc/fstab` file should show `NO entries`. If entries are present, execute the following command to disable all swap entries currently active:

   ```
   sudo swapoff -a sad
   ```
2. Delete all swap entries in `/etc/fstab` to ensure that swap is not enabled again when the machine is restarted.

#### Disable logging to stdout

In a production environment, it is recommended to disable the stdout logging appender because the default Cassandra configuration writes logs to stdout, which adds extra load to the pipe.

To disable the stdout logging appender, edit the `conf/logback.xml` file and delete the following line:

```
<appender-ref ref="STDOUT" />
```

#### Set limits for user account

Set the minimum limits for the user account.

If running with a console or `init.d`, create a `/etc/security/limits.d/cassandra.conf` file, and add the following lines to it (replace `cassandra_user` with the relevant user account).

```
<cassandra_user> - memlock unlimited
<cassandra_user> - nofile 100000
<cassandra_user> - nproc 32768
<cassandra_user> - as unlimited
```

If running via a system service, ensure that the following lines are present in the `[SERVICE]` section of the Cassandra service file:

```
LimitMEMLOCK=infinity
LimitNOFILE=100000
LimitNPROC=32768
LimitAS=infinity
```

### Clean up Cassandra repair history

By default Apache Cassandra 3.11.x does *not* clean `nodetool repair` trace history. This can cause the `system_distributed` keyspace to increase in size over time. The extent of the issue can be seen by running the following command to see how much space is being consumed by `system_distributed`:

```
du -md 1 <cassandra_root>/data/data/ | sort -n
```

To prevent this issue, set a `7 day TTL` on the repair history tables and remove any existing data:

1. Execute the following using `cqlsh` on one of the Cassandra nodes:

   ```cql
   ALTER TABLE system_distributed.repair_history WITH default_time_to_live = 604800;
   TRUNCATE system_distributed.repair_history;
   ALTER TABLE system_distributed.parent_repair_history WITH default_time_to_live = 604800;
   TRUNCATE system_distributed.parent_repair_history;
   ```

   {{< alert title="warning" color="warning" >}}Cassandra will not keep the TTL modification on system tables when restarting, so you must execute these commands at each cassandra restart.{{< / alert >}}
2. Run the following against all Cassandra nodes to clean up the snapshots generated by the truncate:

   ```
   nodetool clearsnapshot system_distributed
   ```

## Record thread dumps in Cassandra logs

To record thread dumps in Cassandra logs, verify that the `$CASSANDRA_HOME/conf/cassandra-env.sh` file have executable permissions, and add the following commands to the end of the file.

```
JVM_OPTS="$JVM_OPTS -XX:+UnlockDiagnosticVMOptions"
JVM_OPTS="$JVM_OPTS -XX:+LogVMOutput"
JVM_OPTS="$JVM_OPTS -XX:LogFile=${CASSANDRA_HOME}/logs/system.log"
```

### Further information

See also [Perform essential Apache Cassandra operations](/docs/cass_admin/cassandra_ops/).
