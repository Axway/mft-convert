{
"title": "API Gateway 7.7 April 2019 Release Notes",
"linkTitle": "API Gateway April 2019",
"weight": "20",
"date": "2020-03-26"
}

## Summary

API Gateway is available as a software installation or a virtualized deployment in Docker containers. The software installation is available on Linux. For more details on supported platforms for software installation, see [API Gateway Installation Guide](/docs/apim_installation/apigtw_install/).

Docker deployment is supported on Linux. For a summary of the system requirements for a Docker deployment, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

## New features and enhancements

The following new features and enhancements are available in this release.

### Traffic monitor enhancements

You can now enable and disable monitoring per path instead of per interface to allow more fine grained monitoring configuration for Traffic Monitor data.

### Configure nested paths in API Gateway Manager

You can edit and configure nested paths in the API Gateway Manager UI, and the changes are synchronized with Policy Studio.

### Filter JMS transactions by custom property in Traffic Monitor

You can search for Java Message Service (JMS) custom properties in Traffic Monitor using the filter options.

### Redaction support for API Gateway trace log

Trace log records can now be redacted.

### Entity Explorer added to API Gateway client tools installer

The API Gateway client tools installer now includes Entity Explorer.

### Policy Studio filter enhancements

The **ICAP** filter now supports custom headers.

### Subscription licensing for API Management

API Gateway and API Manager now support usage based subscription licensing when deployed in container mode. This feature is not available for API Gateway and API Manager when deployed in classic mode.

### Elastic topology container deployment enhancements

The following abilities have been added:

* Use an environment variable to set the TRACE level in an API Gateway container.
* Insert files into a container at startup.
* Use selectors for log file naming to facilitate persistent storage of files in volumes.

### OpenJDK support

API Gateway now supports OpenJDK JRE, and the default installation includes an OpenJDK JRE. The installer no longer ships with Oracle JRE.

You can configure Apache Cassandra to use the API Gateway OpenJDK JRE or you can install a separate JRE (OpenJDK or Oracle) for use with Cassandra. If using Cassandra TLS/SSL with JDK8u151 or later, it is no longer necessary to download Java Cryptographic Extension (JCE) policies for your JRE.

### Third-party library updates

OpenJDK replaces Oracle JRE as the Java runtime for API Gateway and API Manager.

## Limitations of this release

This release has the following limitations:

### Elastic topology container deployment

When using an elastic container deployment:

* Traffic monitor data for a specific API Gateway instance does not persist in the event of that instance container stopping. However, you can redirect the trace and traffic logs to `stdout` instead of to separate files, which allows the logs to be read directly from each container by an external logging service.
* Distributed Ehcache is not supported. However, you can use Apache Cassandra as a distributed data store.
* To upgrade from versions earlier than 7.6.2, you must first upgrade to a 7.7 classic deployment and then migrate to an elastic container deployment.

### Other deployment options

This release is not available as a virtual appliance or as a managed service on Axway Cloud.

## Deprecated features

As part of our software development life cycle we constantly review the core API Management products and related components. As part of this review, API Tester has been deprecated in this release. This standalone tool is no longer supported and will be removed from the next major release. It is recommended to use alternative tools, such as Postman or SoapUI.

API Tester is vulnerable to the following security vulnerabilities, and continued usage is at your own risk:

* CVE-2009-1006
* CVE-2011-3545
* CVE-2011-3551
* CVE-2011-3553
* CVE-2011-3556
* CVE-2011-3557
* CVE-2013-2380
* CVE-2013-2461
* CVE-2013-5780
* CVE-2013-5782
* CVE-2013-5797
* CVE-2013-5802
* CVE-2013-5803
* CVE-2013-5804
* CVE-2013-5823
* CVE-2013-5825
* CVE-2013-5830

## Removed features

In our efforts to continually upgrade our products in response to the needs of our customers’ IT environments, Axway occasionally discontinues support for some capabilities. API Gateway 7.6.2 is the last release that includes the following capabilities, which have been removed from the 7.7 release:

* Support for MySQL Server 2005 has been removed.

## Known issues

The following are known issues for this release of API Gateway.

### Documentation might contain references to removed features

Documentation might contain references to removed features (for example, hardware or virtual appliances, or Windows support). This does not mean that the removed features are still supported, and the references should be ignored.

### Team development – Conflicts when adding dependencies between template projects

In Policy Studio, if you create a new common project from the Common Project template and a new API project from the API Project template and you try to add the API project as a dependent project of the common project, a conflict occurs.

This is due to an issue with the Axway PassPort repositories containing conflicting (randomly generated) password fields in the common and API projects.

As a workaround, use the Entity Explorer tool to set the value of the field `FIELD_KEYSTORE_PASSWORD` to the same value in both the common and API projects.

Related issues: RDAPI-11159

### Upgrade – Changes in order of execution of database statements

If you are upgrading your API Gateway installation, and you are using a **Retrieve from or write to database** filter with multiple database statements in your old installation, you must review the ordering of the database statements after you upgrade.

In earlier versions of API Gateway multiple database statements were not executed in the order they were listed in the filter. Now, database statements are executed in the order they are listed in the filter. After upgrading you must ensure that multiple database statements are listed in the order they should be executed.

Related issues: RDAPI-11455

### Save to File filter

The **Save to File** filter may cause up to 2% of the transactions to fail with the following error:

`java.lang.RuntimeException: No such file or directory. cannot remove file '/path/to/filename'`

This happens in the following cases:

* The **Save to File** filter is pointing to a directory where the number of files has already reached the set **Maximum number of files** limit.
* The **Save to File** filter is part of a policy that is under heavy concurrent load.

If this happens, it is recommended to use a periodic job scheduled at an appropriate frequency for the "housekeeping" of the directory, and not to rely on the **Save To File** filter to do this.

Related issues: RDAPI-7619

### WebSocket protocol

* If you use %h in the Access Log initial string and your DNS configuration is not correct (for example, a name server configured on `/etc/resolv.conf` is not reachable), the HTTP Long Polling connections have a time delay at the API Gateway. WebSocket connections are not affected.
* Adding the same URL for a WebSocket path and a HTTP path is not supported. You get an error message if you try this in Policy Studio.

### JWT filters

When you operate in FIPS mode, the implementation from the default, non-FIPS provider is invoked, if any of the following algorithms is selected in the **JWT Signing** filter:

* RSASSA-PSS using SHA-256 and MGF1 with SHA-256
* RSASSA-PSS using SHA-384 and MGF1 with SHA-384
* RSASSA-PSS using SHA-512 and MGF1 with SHA-512

To avoid this, disable the Bouncy Castle Crypto Provider in the /system/conf/jvm.xml file. When the JWT Signing filter with one of the above algorithms selected is called, the filter fails with the following error:

`ERROR 18/Apr/2016:16:24:39.275 [4a48:17e014570200451f205ec316] java exception:`

`com.vordel.circuit.jwt.JWTException: com.nimbusds.jose.JOSEException: Unsupported RSASSA algorithm: SHA512withRSAandMGF1 Signature not available`

Related issues: RDAPI-3041

### Add JSON Node filter displays redacted data in trace

When the **Add JSON Node** filter is used in an API Gateway policy, and redaction of JSON message content has been configured, sensitive redacted data in the JSON body is still displayed in the API Gateway trace log file. Regardless of the trace level, the redacted data should be hidden in the trace log when the message body has been processed by API Gateway.

Related issues: RDAPI-8227

## Tips and tricks

### Upgrade

* If you are upgrading your API Gateway installation, and you are using a **Scripting Language** filter in your old installation with the **Language** field set to `JavaScript (Rhino engine JRE7 and earlier)`, you must change the **Language** of the filter to `JavaScript` and ensure that the JavaScript syntax in the script conforms with Nashorn engine syntax. If you do not make these changes, the script continues to work in your new installation, but with a severe drop in performance. It is recommended to use Nashorn for all new development.
* After upgrading your API Gateway installation, it is recommended that you clear your browser cache before using any of the web UIs. See the documentation for your browser for information on how to clear the cache.

### Multiple datacenters

* You must add external load balancer hosts to the Node Manager whitelist to ensure that they are accepted in each datacenter.
* You may need to increase the Node Manager timeout for longer API Gateway startup times in a multi-datacenter environment.
* You may need to increase the maximum received bytes per transaction to optimize performance in a multi-datacenter environment.

### Performance

For best performance, we recommend:

* Always install the latest release and service packs to benefit from new improvements and features.
* Use HTTP 1.1 instead of 1.0 whenever possible to enable persistent connections.
* Use persistent connections throughout the entire stack, and overwrite the connection type with `keep-alive` whenever possible to avoid creating and dropping connections for each individual request.
* In a classic deployment, use Ehcache instead of KPS whenever possible, because data held in process memory is quicker to access. Note that Ehcache is not supported in a container deployment.
* Keep thread count reasonable. A good starting point to use as a rule of thumb is `initial latency(ms)* expected throughput (count) / 1000 ms = the number of threads (count)`. In HA deployment, you may want to account failure in one node. Note that the ratio of thread count and CPU cores impacts the latency. You may also want to consider horizontal scaling instead of vertical scaling.

## Documentation

You can find the latest information and up-to-date user guides at the [Axway Documentation portal](https://docs.axway.com).

### Related documentation

The Amplify API Management solution enables you to create, publish, promote, and manage Application Programming Interfaces (APIs) in a secure and scalable environment.

The following reference documents are also available:

* [Supported Platforms](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en) - Lists the different operating systems, databases, browsers, and thick client platforms supported by each Axway product.

* [Interoperability Matrix](https://docs.axway.com/bundle/Axway_Products_InteroperabilityMatrix_allOS_en) - Provides product version and interoperability information for Axway products.

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email <support@axway.com> or visit [Axway Support](https://support.axway.com).

See for the information that you should be prepared to provide when you contact Axway Support.
