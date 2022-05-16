---

    title: About  AIX and IBM HACMP
    linkTitle: UNIX High Availability
    weight: 200

---
This sub-book describes how to install Transfer CFT and configure using
AIX with IBM HACMP. This
topic lists prerequisites and provides an overview of the
system.

## Prerequisites

System:

- AIX 5.3 and higher

Software:

- HACMP 4.4.1
- Transfer CFT

### Preparing the environment

The test environment comprises two nodes: hacmp1 and hacmp2.

For the virtual IP address, we defined the virtual IP of the Transfer
CFT server for HACMP on our DNS. It is also possible to declare the virtual
IP on each node of the cluster, in the /etc/hosts
file. For our tests, we added the following line to this file:

`172.17.50.57    cft-ip`

This virtual cft-ip address must be declared on all the cluster nodes
(hacmp1 and hacmp2 for our tests). Use it for our Transfer CFT service.

### Shared File Systems

On our cluster, the shared file system is <span class="code">`/cft-vg.`</span> (group volume
cftvg).

You can use several separate file systems to install the configuration,
the scripts, the files to be sent, and the files to be received.

### Setting up the Transfer CFT scripts for HACMP

The scripts cftstartFailover and cftstopFailover are copied
to the shared filesystem:

- cftstartFailover:This script creates a backup and recreates
    the logs and accounting files. It then restarts Transfer CFT.

<!-- -->

- cftstopFailover:This script tries a normal Transfer CFT stop procedure,
    then cleans the environment.

These scripts are available in the topic Transfer
CFT Scripts of this document.

### Transfer CFT installation

The process of installing Transfer CFT is the same as for installing
Sun Cluster.
