---

    title: About Solaris  Sun cluster 
    linkTitle: Solaris Sun cluster
    weight: 220

---
This sub-book describes the Transfer CFT installation and configuration
with a Solaris Sun cluster. This topic
describes the prerequisites and reference materials for installing a Solaris
Sun cluster.

## Prerequisites

System requirements:

- Solaris 10

Software requirements:

- Sun Cluster 3.0
    or 3.1
- Transfer CFT

## Transfer CFT and Sun cluster integration

Axway used Generic Data Services (GDS) to test Transfer CFT integration
with Sun Cluster.

The choice of GDS was based on:

- Rapid and direct
    integration with Sun Cluster
- Relative ease of
    configuration of GDS, requiring only three scripts: start, stop and test
- Transfer CFT already
    includes the scripts capable of executing the operations called by GDS

Sun Cluster operates under the **root** user. The scripts **cftstartFailover**,
**cftstopFailover** and **cftprobeFailover** were developed to execute
the **cftstart**, **cftstop** and **cftping** commands under
the user ID for the Transfer CFT UNIX account user. These scripts are
available in the sub-book Transfer CFT
scripts.

To complete the setup of this installation, you must supply GDS with
the list of listening addresses for Transfer CFT TCP/IP and the shared
system.

****<span id="Sun_Cluster_reference_documentation"></span>Sun Cluster reference
documentation****

<http://docs.sun.com/?q=sun+cluster>
