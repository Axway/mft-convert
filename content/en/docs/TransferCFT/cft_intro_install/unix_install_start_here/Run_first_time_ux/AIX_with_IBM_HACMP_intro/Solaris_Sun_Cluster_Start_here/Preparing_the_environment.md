---

    title: Installation prerequisites
    linkTitle: Installation prerequisites
    weight: 240

---
This topic describes the environmental prerequisites for installing
a Transfer CFT Solaris Sun cluster.

## Prerequisites

### Virtual IP address

The virtual IP address of the Transfer CFT service for the Sun Cluster
is defined for each node of the cluster in the <span class="code">`/etc/hosts`</span> file.
For our tests, we added the following line to this file: `172.17.50.50 cft-ip`

### Shared file systems

In our development and test cluster, the shared file system is: <span class="code">`/global/cft`</span>.

You can use several separate file systems to install the configuration.
These can include the GDS scripts, the files to be sent, and the files
to be received.

### Setting up the Transfer CFT scripts for Sun Cluster

The scripts cftstartFailover, cftstopFailover, and cftprobeFailover
are copied to the shared file system:

- cftstartFailover: This script creates a backup and then recreates the
    logs and accounting files. It restarts Transfer CFT when it has completed
    the task.

<!-- -->

- cftstopFailover: This script to attempts a normal Transfer CFT stop
    procedure, and cleans the environment.

<!-- -->

- cftprobeFailover: This script tests for the presence of Transfer CFT,
    and cleans the environment according to the cftping
    response.
