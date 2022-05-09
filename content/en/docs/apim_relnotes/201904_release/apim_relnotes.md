{
"title": "API Manager 7.7 April 2019 Release Notes",
"linkTitle": "API Manager April 2019",
"weight": "60",
"date": "2020-03-26"
}

## Summary

API Gateway is available as a software installation or a virtualized deployment in Docker containers. The software installation is available on Linux. For more details on supported platforms for software installation, see [API Gateway Installation Guide](/docs/apim_installation/apigtw_install/).

Docker deployment is supported on Linux. For a summary of the system requirements for a Docker deployment, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

## New features and enhancements

The following new features and enhancements are available in this release.

### API Manager custom properties

* API Manager provides a new REST API for viewing all existing custom properties and their values (including metadata) for APIs, users, organizations, and apps.
* Custom properties have default values.
* Custom properties are validated.

### Compliance enhancements

* Enforce password changes at first login to API Manager.
* API Manager displays the user's last login time.
* You can configure an advisory banner to be displayed on the login page.
* Users are redirected to the login page after session timeout.

### Auditing enhancements

Additional audit events are logged in the audit log.

### Additional search field support for front-end and back-end APIs

* Support to retrieve front-end APIs that are exposed on a particular path.
* Ability to query for back-end APIs with a particular `basePath`, `resourcePath`, `organizationId`, or `serviceType`.

### Try It improvements

When trying the method of an API, if authentication credentials are required you can now select an application and an API Key/OAuth credential.

API Manager now supports CORS/Javascript Origin validation of the credentials to minimize CORS issues when using Try It.

### Support for Swagger in YAML format

API Manager now supports Swagger 2.0 documents that are in YAML format, including support for external references.

### Swagger 2.0 enhancements

Swagger 2.0 download from the API Catalog has been enhanced and now includes missing fields. For more information regarding unsupported Swagger 2.0 fields, see [Known issues](#known-issues).

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

The following are known issues for this release of API Manager.

### Documentation might contain references to removed features

Documentation might contain references to removed features (for example, hardware or virtual appliances, or Windows support). This does not mean that the removed features are still supported, and the references should be ignored.

### Cassandra synchronization in multi-datacenter environments

In multi-datacenter environments with Cassandra read/write consistency set to local quorum, there is a small risk of configuration corruption if the event triggering API Manager to load a configuration change happens before the configuration replication to the other datacenter is complete. Changing the polling time as described in
[Configure API Management in multiple datacenters](/docs/apimgmt_multi_dc/multi_datacenter_config/#configure-api-management-in-multiple-datacenters) reduces this risk, but does not remove it completely.

This issue results in outdated configuration data being used for the affected API until API Gateway is restarted. For example, as a result of this, valid traffic may be rejected if a new API has been added and not updated, or wrong traffic may be accepted if an API has been deprecated and not updated. The workaround requires a restart of all API Gateway instances in the affected datacenter.

Axway is working on a product change that will avoid restarting API Gateway in such situations, and recommends to:

* Wait for the resolution before going live with multiple datacenters and local quorum consistency.
* If this is not possible, monitor your production environment closely for this error, and restart API Gateway if the error is encountered.

### RAML import does not support references to external files

Importing RAML version 0.8 or RAML version 1.0 files that include references to external files is currently not supported.

Related issues: RDAPI-10356

### Upgrade from API Manager 7.3.0 not supported

Upgrading API Manager version 7.3.0 to version 7.7 is not supported.

Related issues: RDAPI-5136, RDAPI-8237

### API Manager users cannot complete registration after upgrading from 7.3.1

New users that were registered in API Manager 7.3.1 before an upgrade, but who did not complete registration by activating their account with the link provided in email, cannot complete registration after the upgrade. The link in the email references the API Manager API v1.1 that is no longer available. For example:

```
https://<API Gateway IP address>/api/portal/v1.1/users/validateuser?email=s@s.com&validator=9a5addcb-e10c-499b-bf0a-0c70915f3862
```

The workaround is that the user copies the link address, pastes it to the address bar, and changes the API version `v1.1` to `v1.2` or `v1.3`. After this, the activation link works, and the user can complete registration.

This issue does not occur when upgrading from API Manager 7.4.0 or later.

Related issues: RDAPI-3417

### API Manager removes trailing slashes from the paths of APIs created from a Swagger definition file

When a back-end API is created from a Swagger definition file that contains trailing slashes in the path, API Manager removes the trailing slashes from the paths. Furthermore, when a request comes in with a trailing slash, API Gateway returns HTTP error `403 bad request` because it does not match the requested path.

To preserve the trailing forward slashes, edit the `jvm.xml` file and set the `com.vordel.apimanager.uri.path.trailingSlash.preserve` system property to `true`. After updating the file, restart the API Gateway instance to enable the changes to be applied.

For example:

```
<VMArg name="-Dcom.vordel.apimanager.uri.path.trailingSlash.preserve=true"/>
```

The default value of the property is `false`.

Related issues: RDAPI-9243

### Unsupported Swagger 2.0 elements

When registering a back-end API from a Swagger 2.0 definition, API Manager does not support the following elements and does not import them into the API Catalog:

* `title`
* `securityDefinitions`

For each path/API method:

* `security`
    * For each parameter: `allowEmptyValue`
    * For each response code: `headers`

{{< alert title="Note" color="primary" >}}Some of these elements are also used in the model definitions section in the Swagger 2.0 specification, and API Manager imports these elements when contained in that section. API Manager supports all elements in the Swagger model definitions section.{{< /alert >}}

### Error importing API collections

When importing API collections from earlier versions of API Manager, an error occurs if the APIs are based on back-end APIs that were generated from Swagger version 1.1 or 1.2.

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
