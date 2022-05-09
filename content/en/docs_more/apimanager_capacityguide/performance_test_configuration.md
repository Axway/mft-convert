---
title: Performance test configuration
linkTitle: Performance test configuration
weight: "2"
date: 2019-09-26
description: Describes the configuration of the systems used in performance testing. These include single node with local Cassandra, multi-node high availability with local Cassandra, and multi-node high availability with remote Cassandra.
---

The API Management performance tests were run both single node configuration and multi-node high availability (HA) configuration. Apache Cassandra 2.2.12 instances were used as the backing data store. In the HA configuration, both local Cassandra and remote Cassandra were tested.

All tests were run on bare metal hardware connected with a 10 Gigabit Ethernet switch on a dedicated network in the test lab. Hyper-threading was not used in any of the configurations.

## Single node configuration

There were three systems involved in the single node configuration:

* **Traffic generator**: JMeter client running multiple threads to simulate multiple clients
* **System under test**: API Gateway or API Manager instance
* **Back-end service**: NGINX web server

{{< alert title="Note" color="primary" >}}Only one Cassandra instance was used.{{< /alert >}}

The traffic from the traffic generator was passed to the system under test that sent the traffic to the back-end service.

![Diagram illustrating the single node test configuration.](/Images/CapacityPlanningGuide/CPG_single_node.png)

### Single node hardware profiles

The hardware profiles of the three machines were as follows:

| System            | Machine      | OS                           | CPU                                                                               | RAM   |
|-------------------|--------------|------------------------------|-----------------------------------------------------------------------------------|-------|
| Traffic generator | JMeter 3.0   | Red Hat Enterprise Linux 7.2 | Dell R420 - 2 Intel® Xeon® processor E5-2400 @ 2.2 GHz, 6-Core (8 cores in total) | 16 GB |
| System under test | API Gateway API Manager   | Red Hat Enterprise Linux 7.2 | Dell R420 - 2 Intel® Xeon® processor E5-2400 @ 2.2 GHz, 6-Core (8 cores in total) | 16 GB |
| Back-end service  | NGINX 1.10.2 | Red Hat Enterprise Linux 7.2 | Dell R420 - 2 Intel® Xeon® processor E5-2400 @ 2.2 GHz, 6-Core (8 cores in total) | 16 GB |

## Multi-node HA configuration with local Cassandra

The multi-node HA configuration had six nodes that all had a local Cassandra instance running. The Admin Node Manager was located on the first node in the configuration.

To test scalability, the same tests were repeated, increasing the number of nodes one by one up to the total of six nodes. The configuration always had the minimum of three Cassandra instances running to provide a Cassandra quorum for replicating data, one on each of the first three nodes. For tests with 4–6 nodes, the number of Cassandra instances was increased in parallel with the number of nodes.

There were four systems involved:

* **Traffic generator**: JMeter client running multiple threads to simulate multiple clients
* **Load balancer**: NGINX server
* **System under test**: API Gateway or API Manager instance
* **Back-end service**: NGINX web server

The traffic from the traffic generator was passed to the load balancer that directed the traffic to available nodes in the system under test. All nodes then sent the traffic to the back-end service.

![Diagram illustrating the multinode HA configuration with local Cassandra](/Images/CapacityPlanningGuide/CPG_HAL.png)

### Multi-node HA with local Cassandra hardware profiles

The hardware profiles of the four machines were as follows:

| System            | Machine      | OS                           | CPU                                                                               | RAM   |
|-------------------|--------------|------------------------------|-----------------------------------------------------------------------------------|-------|
| Traffic generator | JMeter 3.0   | Red Hat Enterprise Linux 7.2 | Dell R420 - 2 Intel® Xeon® processor E5-2400 @ 2.2 GHz, 6-Core (8 cores in total) | 16 GB |
| Load balancer     | NGINX 1.10.2 | Red Hat Enterprise Linux 7.2 | Dell R420 - 2 Intel® Xeon® processor E5-2400 @ 2.2 GHz, 6-Core (8 cores in total) | 16 GB |
| System under test | API Gateway API Manager   | Red Hat Enterprise Linux 7.2 | Dell R420 - 2 Intel® Xeon® processor E5-2400 @ 2.2 GHz, 6-Core (8 cores in total) | 16 GB |
| Back-end service  | NGINX 1.10.2 | Red Hat Enterprise Linux 7.2 | Dell R420 - 2 Intel® Xeon® processor E5-2400 @ 2.2 GHz, 6-Core (8 cores in total) | 16 GB |

## Multi-node HA configuration with remote Cassandra

The multi-node HA configuration had six nodes. The Admin Node Manager was located on the first node in the configuration. Cassandra was running on three separate dedicated machines.

To test scalability, the same tests were repeated increasing the number of nodes up to the total of six nodes. The number of nodes did not affect the number of Cassandra instances, but the three-node Cassandra HA cluster was always used.

There were four systems involved:

* **Traffic generator**: JMeter client running multiple threads to simulate multiple clients
* **Load balancer**: NGINX server
* **System under test**: API Gateway or API Manager instance
* **Back-end service**: NGINX web server

The traffic from the traffic generator was passed to the load balancer that directed the traffic to available nodes in the system under test. All nodes then sent the traffic to the back-end service. In addition, each node also communicated with the remote Cassandra, and the three Cassandra machines synchronized between themselves.

![Diagram illustrating the multinode HA configuration with remote Cassandra](/Images/CapacityPlanningGuide/CPG_HAR.png)

### Multi-node HA with remote Cassandra hardware profiles

The hardware profiles of the four machines were as follows:

| System            | Machine      | OS                           | CPU                                                                               | RAM   |
|-------------------|--------------|------------------------------|-----------------------------------------------------------------------------------|-------|
| Traffic generator | JMeter 3.0   | Red Hat Enterprise Linux 7.2 | Dell R420 - 2 Intel® Xeon® processor E5-2400 @ 2.2 GHz, 6-Core (8 cores in total) | 16 GB |
| Load balancer     | NGINX 1.10.2 | Red Hat Enterprise Linux 7.2 | Dell R420 - 2 Intel® Xeon® processor E5-2400 @ 2.2 GHz, 6-Core (8 cores in total) | 16 GB |
| System under test | API Gateway API Manager   | Red Hat Enterprise Linux 7.2 | Dell R420 - 2 Intel® Xeon® processor E5-2400 @ 2.2 GHz, 6-Core (8 cores in total) | 16 GB |
| Back-end service  | NGINX 1.10.2 | Red Hat Enterprise Linux 7.2 | Dell R420 - 2 Intel® Xeon® processor E5-2400 @ 2.2 GHz, 6-Core (8 cores in total) | 16 GB |
