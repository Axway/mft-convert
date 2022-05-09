---
title: Performance test settings
linkTitle: Performance test settings
date: 2019-09-26
weight: "3"
description: Describes the settings used for performance testing. For example, this includes the number of API Manager, API Gateway, and Cassandra instances, and the various security protocols and monitoring settings used.
---


* For the single node API Management performance tests, a standalone API Gateway or API Manager and a Cassandra instance were used. For the multi-node HA tests, a minimum of one and maximum of six standalone API Gateway or API Manager instances and three Cassandra instances were used.
* All API Manager tests used Cassandra instances as the backing data store. In the single node configuration and the multi-node HA configuration with local Cassandra, Cassandra was co-located with the API Manager server. In the multi-node HA configuration with remote Cassandra, Cassandra was located on separate dedicated hosts.
* Each single node test ran for 10 minutes, with a 60 second ramp-up period for each test. Each multi-node test ran for 5 minutes, with a 60 second ramp-up period for each test. In the single node tests, the traffic generator ran 60 threads for each test. In the multi-node tests, the traffic generator ran 60 threads for 1–3 nodes and 120 threads for 4–6 nodes.
* The out-of-the-box settings for the API Gateway server were used unless otherwise specified. There was no tuning for API Gateway or API Manager.

The settings are summarized as follows:

| Setting               | Single node                                   | Multi-node                                                                                       |
|-----------------------|-----------------------------------------------|--------------------------------------------------------------------------------------------------|
| Number of instances   | 1 API Gateway or API Manager, and 1 Cassandra | 1–6 API Gateway or API Manager, and 3 Cassandra                                                  |
| API Manager datastore | Cassandra co-located with API Manager         | Cassandra co-located with API Manager for local HA, and located on dedicated hosts for remote HA |
| Test duration         | 10 minutes with 60 second ramp-up             | 5 minutes with 60 second ramp-up                                                                 |
| Number of threads     | 60 per test                                   | 60 for 1–3 nodes, 120 for 4–6 nodes                                                              |

{{< alert title="Note" color="primary" >}}In the scalability tests with multi-node HA configuration, the environment was fully cleaned before adding new nodes. Both API Gateway and Cassandra were removed completely and then reinstalled. This eliminated any risk of reusing Cassandra data from previous tests.{{< /alert >}}

## Connection security settings

For all scenarios, real-world production settings were used for connection security. These include the following:

* Traffic generator sent all requests to API Gateway, API Manager, or load balancer using TLS 1.2 and 2048-bit TLS certificates
* TLS 1.2 and 2048-bit TLS certificates used between API Gateway or API Manager and the back-end service
* FIPS-compliant TLS cipher suites used on all channels
* TLS sessions reused (TLS handshake not renegotiated for each request) on all connections
* HTTP 1.1 on all connections
* HTTP keep-alives enabled for all connections

## Monitoring settings

Default monitoring settings were used (unless explicitly stated):

* Trace level set to INFO
* Traffic monitoring enabled
* Real-time monitoring enabled
* Transaction event logging enabled
