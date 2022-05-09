---
title: Performance test results
linkTitle: Performance test results
weight: "6"
date: 2019-09-26
description: Presents the single node and multi-node performance results achieved for all API Management configurations.
---

## Test results terminology

This section defines common terms used in the performance test results:

| Metric                          | Definition                                                                             |
|---------------------------------|----------------------------------------------------------------------------------------|
| Average transactions per second (TPS)                            | The average number of transactions completed per second over the duration of the test. |
| Average latency @ 95%           | The average time for each transaction sampled at the 95th percentile.                  |
| Average CPU                  (%)                              | The average CPU usage of the API Gateway host over the duration of the test.           |
| Average memory                (%)                              | The average memory footprint of the API Gateway process over the duration of the test. |
| Network Read/Write (Mb/s)       | The average transfer speed for received (read) and sent (write) data.                  |
| Disk IO (%)                     | The average disk utilization over the duration of the test.                            |
| Threads                         | The number of threads the traffic generator ran for each test.                         |

## Test results for single node configuration

Download the [test results of all single node scenarios both with and without monitoring from Axway Support](https://support.axway.com/en/documents/document-details/id/1444235).

The test cases in the test results appear in the format `Product_TestScenario_MessageSize_SSL_TrafficMonitoring_RealTimeMonitoring`.

For example, the test case `API_Gateway_Benchmark_HTTPBasic_100KB_SSL_noTrafMon_noRTM` has the following characteristics:

* Product: API Gateway
* Test scenario: HTTP Basic authentication
* Message size: 100KB
* SSL: Enabled
* Traffic monitoring: Disabled
* Real-time monitoring: Disabled

Similarly, the test case `API_Manager_PassThrough_Post_100KB_SSL_TrafMon_RTM` has the following characteristics:

* Product: API Manager
* Test scenario: Pass through POST
* Message size: 100KB
* SSL: Enabled
* Traffic monitoring: Enabled
* Real-time monitoring: Enabled

## Test results for multi-node HA configurations

Download the [test results of all multi-node HA scenarios both with local and remote Cassandra from Axway Support](https://support.axway.com/en/documents/document-details/id/1444236).

The test cases in the test results appear in the format `Product_TestScenario_HighAvailability_NumberOfNodes_SSL_TrafficMonitoring_RealTimeMonitoring`. For multi-node tests, the message size is always 1KB, and it is not specified in the test case.

For example, the test case `API_Manager_APIKEY_HAL_MultiNode_6nodes_SSL_TrafMon_RTM` has the following characteristics:

* Product: API Manager
* Test scenario: API key
* HA: Multi-node HA with local Cassandra
* Number of nodes: 6
* SSL: Enabled
* Traffic monitoring: Enabled
* Real-time monitoring: Enabled

Similarly, the test case `API_Manager_APIKEY_MultiNode_RemoteCas_6nodes_SSL_TrafMon_RTM` has the following characteristics:

* Product: API Manager
* Test scenario: API key
* HA: Multi-node HA with remote Cassandra
* Number of nodes: 6
* SSL: Enabled
* Traffic monitoring: Enabled
* Real-time monitoring: Enabled

For CPU, memory, network read/write, and disk IO, the results are an average of all nodes for the given number of nodes tested at that time.
