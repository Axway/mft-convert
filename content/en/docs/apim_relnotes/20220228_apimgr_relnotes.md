---
title: API Gateway and API Manager 7.7 February 2022 Release Notes
linkTitle: API Gateway and API Manager February 2022
weight: 95
date: 2022-01-21
description: API Gateway and API Manager updates are cumulative, comprising new
  features and changes delivered in previous updates unless specifically
  indicated otherwise in the Release notes.
---
## Installation

* To **update** your API Gateway, see [Update from API Gateway One Version](/docs/apim_installation/apigw_upgrade/upgrade_steps_oneversion/).
* To **upgrade** from an older version, see [Upgrade from API Gateway 7.5.x or 7.6.x](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/).
* For more details on supported platforms for software installation, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).
* For a summary of the system requirements for a Docker deployment, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

### Update a container deployment

Any custom `.fed` files deployed to a container must be upgraded using [upgradeconfig](/docs/apim_installation/apigw_upgrade/upgrade_analytics#upgradeconfig-options) or [projupgrade](/docs/apim_reference/devopstools_ref#projupgrade-command-options). They must be upgraded the same way, regardless of whether they are API Manager enabled or not. The `.fed` files contain the updates for the API Manager configuration and can be used to build containers.

## New features and enhancements

The following new features and enhancements are available in this update.

### Configure an API Gateway Docker container to use a persisted volume for API Gateway configuration

This feature enhancement allows an API Gateway in EMT mode to be reconfigured without the need to rebake its Docker image. Configuration updates such as policy changes, JVM system properties, environment properties and so on, can now be mounted on a Docker Volume and made available to an API Gateway container. For more information, see [Create an API Gateway with Docker volumes](/docs/apim_howto_guides/configuring_apigw_container/).

### Initial YAML Environmentalization Support in Policy Studio

Initial support for editing and managing YAML-based projects with environmentalization of values has been added to Policy Studio. This implementation allows the following:

* Extracting field values out to `values.yaml` in the root of your project.
* Inlining values back into the original entity fields.
* Visualizing the `values.yaml` tree in the **Navigator** view.
* Basic editing of externalized values via text fields for a given tree node.
* Manually editing entity fields to reference environmentalized values via placeholder syntax (for example, `{{Policies.My_Policy.Name.field}}`).
* Content assist for referencing known values.

For more information on YAML-based environmentalization and `values.yaml` in general, see [YAML Environmentalization](/docs/apim_yamles/yamles_environmentalization/).

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update, which may impact on your current installation.

### Distributed cache socket connect timeout

A new socket connect timeout is added for distributed cache replicated updates. The default timeout is 500 millisecond and can be changed if required using JVM system property `com.axway.rmi.socket.connect.timeout`. For more information, see [System property changes](/docs/apim_reference/system_props/#77-february-2022).

### WSDL Schema Validation

The Schema Validation filter now caches WSDL Schema objects to improve performance of WSDL schema validation. Previously only Schemas that were configured using the Schema Validation filter were cached. The size of the Schema Validation cache can be increased if required with system property schemaCacheSize. For more information, see [System property changes](/docs/apim_reference/system_props/#762).

### Axway Terms and Conditions must be accepted to start an API Gateway Docker container

Updated General Terms and Conditions (T&C) have been added to API Gateway. During installation in interactive mode a pop-up window is shown for you to accept the T&C. For API Gateway Docker container, a new parameter (`ACCEPT_GENERAL_CONDITIONS`) has been introduced, which must be set to `yes` in order to start a container.

For more information, see [Acceptance of General Conditions for license and subscription services](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/#acceptance-of-general-conditions-for-license-and-subscription-services).

### API Gateway password policy and passphrase policy APIs stricter validation

API Gateway now returns a server error when an invalid JSON payload is passed in the body of a `/adminusers/passwordpolicy` or `/topology/passphrasepolicy` PUT request.

### API Manager Proxy deprecation with a retirement date

In the API Manager Swagger definition, the description for the `retirementDate` parameter defined in the `POST /proxies/{id}/deprecate` path previously contained the sentence "Set to the past to retire immediately". This statement was inaccurate and has now been removed.

Previously, the `retired` flag on the proxy was immediately set to `true` when the `retirementDate` contained a date in the past; however, the proxy itself was not immediately retired. Now, API Manager sets the `retired` flag to `true` when the proxy retirement process is complete. If you expect the `retired` flag in the immediate response from the deprecation request to be `true` then you may be affected by this change.

This issue has been addressed and more details can be found in the related description of the RDAPI-25708 ticket, in the [Other fixed issues](/docs/apim_relnotes/20220228_apimgr_relnotes/#other-fixed-issues) table.

### Increased validation on API Manager Quota operations

Additional input validation has been added to the `PUT /quotas/{id}` and `POST /quotas` paths. This validation is already performed in the `PUT /applications/{id}/quota` and `POST /applications/{id}/quota` paths. As part of this validation API Manager confirms that the API and method defined in the provided quotas exist in the API Catalog. If you are creating or updating quotas on an API immediately after creating it then you may be affected by this change.

## Deprecated features

<!--As part of our software development life cycle we constantly review our API Management offering. As part of this update, the following capabilities have been deprecated-->

No features have been deprecated in this update.

## End of support notices

The following items are end of support:

### Apache Cassandra 2.2.x

Apache Foundation no longer supports Apache Cassandra database version 2.2.x. Although Axway will continue to support this version on a best efforts basis, no critical updates will be available from Apache anymore.

API Gateway supports Cassandra 3.11.11 since [August 2021](/docs/apim_relnotes/20210830_apimgr_relnotes/#support-for-apache-cassandra-31111) update. Therefore, we recommend that all customers update their Cassandra installation to this version as soon as possible.

The Cassandra version 3.11.x end of support date is April 2023. Axway is planning to include support for Cassandra version 4 later this year.

For details on upgrading existing Apache Cassandra environments, see [Upgrade Apache Cassandra](/docs/apim_installation/apigw_upgrade/upgrade_cassandra/).

## Removed features

To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this update, the following features have been removed:

<!--No features have been removed in this update.-->

### Titanium SDK generation

Titanium app was removed as an option for Software Development Kits (SDKs) generation. For more information, see [Mobile Backend Services use cases](/docs/apim_administration/apimgr_admin/api_mgmt_connect_mobile_backend_services/#mobile-backend-services-use-cases).

### Developer tools on Windows 8.1

Support is removed for the installation and update of [Developer tools](/docs/apim_installation/apigtw_install/install_dev_tools/) on Windows 8.1.

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [Release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID                                | Cve Identifier | Description       |
| ----------- | -------------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------ |
|RDAPI-26476|01326549|CVE-2021-42550|**Issue**: Vulnerability found in logback-core-1.2.0.jar deployed with API Gateway.  **Resolution**: The API Gateway logback-core library has been updated to version 1.2.10. This version addresses known vulnerabilities.|

### Other fixed issues

| Internal ID | Case ID                             | Description                    |
| ----------- | ----------------------------------- | -------------------------------------------------------------- |
|RDAPI-19492|01200982  01225341  01144469  01249313|**Issue**: The performance of the Schema Validation filter when validating SOAP operations is too slow. **Resolution**: Caching of the generated XML schema grammar is used to improve the performance of the Schema Validation filter when validating SOAP operations. The size of the cache can be increased if required with the existing 'schemaCacheSize' system property.|
|RDAPI-20726|01159966|**Issue**: The Read Application filter's default attribute name is not available in the message white board. **Resolution**: Read Application filter now uses authentication.application.id, replacing apimgmt.application.id as the default attribute name. This attribute is available in the message white board.|
|RDAPI-22954|01225723  01225700|**Issue**: Swagger definitions with differently defined return codes import correctly in API manager, but when the same API is exported through the API Catalog, the resulting Swagger definition cannot be imported back into API Manager. **Resolution**: Swagger definitions generated by the API catalog can now be re-imported.|
|RDAPI-23841|01212352|**Issue**: Deleting an organization causes a stack trace to occur due to a missing index in Cassandra. **Resolution**: Ensure the missing index is always created if it is missing when starting API Manager.|
|RDAPI-24520|01265588  01265458|**Issue**: Upgrade of projects via Policy Studio was adding the project's ID to the name of the upgraded files and was not removing old files. **Resolution**: Upgrade of projects via Policy Studio now replaces the old store files with the upgraded ones without adding the ID to their names.|
|RDAPI-24563|01265493  01290372|**Issue**: Customers want to be able to configure the jython cache path. **Resolution**: Jython cache path is now configurable through jvm.xml via a new VM argument: `<VMArg name="-Dpython.cachedir=$VDISTDIR/system/lib/jython/.jython_cache" />`|
|RDAPI-24845|01263392|**Issue**: Random namespace addition in XSLT Transformation filter. **Resolution**: The XSLT Transformation filter is now using the configured secure TransformerFactory from `jvm.xml` file for XSLT 1.0 version. As before, a specific Provider class name can be set in the Advanced tab for the filter.|
|RDAPI-25070|01329609  01281131|**Issue**: The API Gateway to Cassandra read request timeout is not user accessible and is set to 12 seconds by default. **Resolution**: The read request timeout can now be set by way of the system property "CASSANDRA_READ_TIMEOUT_MS". The value represents the timeout in milliseconds, and it must be a non-negative integer. To disable the read timeout, set the value to 0. Example for 1 min: `<SystemProperty name="CASSANDRA_READ_TIMEOUT_MS" value="60000" />`|
|RDAPI-25161|01265904|**Issue**: When two proxy APIs of the same weight are detected, an error message occurs. **Resolution**: Tweak this error message to note the message can be ignored if it is an acceptable use case during API upgrade.|
|RDAPI-25286|01289304|**Issue**: 7.7 update doesn't work when group passphrases are injected with the passphraseExec mechanism. **Resolution**: 7.7 update now works with passphrases injected with the passphraseExec mechanism.|
|RDAPI-25356|01292308|**Issue**: Customers required the ability to direct users to a custom path or URL on logout from API Gateway Manager UI **Resolution**: A new 'logoutRedirectUrl' property is now available in the app.config file for API Gateway Manager Web UI. This property can be set to direct the website to an alternate path or URL following a successful logout.|
|RDAPI-25572|01299173  01289130|**Issue**: Trace filter with 'Included attributes' checked aborts when there are two or more Host headers **Resolution**: Trace Filter has been modified to no longer throw any aborts and to log as many pieces of information as possible. Also, all errors encountered while tracing transactions are now logged. |
|RDAPI-25675|01305410|**Issue**: API Manager 'Upgrade access to newer API' drop down list is unordered. **Resolution**: The 'Upgrade to' drop down list in the 'Upgrade API' dialog now displays a case-sensitive sorted API list by name (and version). This list can be searched based on the keyboard key pressed, highlighting the first API starting with the key pressed; hitting again the same key highlights the next matching API.|
|RDAPI-25681|01299756|**Issue**: API Gateway Manager > Traffic Monitor > HTTP page shows empty data on the right span when 'Record received data for transactions' is disabled in Server Settings > Monitoring > Traffic Monitor configuration. **Resolution**: When the 'Record received data for transactions' is disabled, the recorded sent data is now displayed on the right span of API Gateway Manager > Traffic Monitor > HTTP page. The left span for the recorded received data shows NO DATA.|
|RDAPI-25684|01319310  01303724|**Issue**: Averages for some metrics in Analytics are not aggregated correctly. **Resolution**: Metrics are corrected in Analytics reports for the following averages: Instance CPU, System CPU, Instance Memory, Response Time.|
|RDAPI-25708|01285798|**Issue**: API retirement status is not updated based on the retirement date. **Resolution**: API retirement status is now updated correctly by API Manager and the corresponding API retirement alert is invoked. **Important**: The API is deprecated immediately upon Proxy API deprecation request, and if a retirement date is provided then API Manager processes the API retirement separately. The API retired flag is set during this process along with the invocation of the corresponding alert for each API Manager instance. The Proxy API deprecation request does not retire the API immediately when the provided retirement date is in the past.In API Manager UI, you must refresh the list of APIs on the Front-End and API Catalog pages to verify whether a deprecated API has been retired.|
|RDAPI-25717|01306083|**Issue**: Connection error in the updated Analytics `configureserver` script caused by the wrong MySQL driver being used. **Resolution**: Jython scripts are now configuring the JDBC drivers from the entity store.|
|RDAPI-25848|01309379|**Issue**: Structural error (`should have required property 'schema', missingProperty: schema`) in Swagger generated from **API Manager** > **API Catalog** download as Swagger 2.0 link for APIs with `body` parameters with no schema. **Resolution**: The schema property is now generated from `type` and `format` properties if `body` parameters have no schema set. If `type` matches a key in the API models then the schema is set as `#/definitions/<type>` reference. If `type` matches `array` and `format` matches a key in the API models then the schema is set to `array` type and items as `#/definitions/<format>` reference.|
|RDAPI-25904|01309368|**Issue**: API Gateways using distributed Ehcache configured to use manual peer discovery and synchronous replication for caching can become unresponsive if one of the API Gateways is not responding and there are many cache updates ongoing. **Resolution**: A new timeout is added to control how long Ehcache waits for a response from a synchronous cache update and allow the other API Gateways to remain responsive until the issue with the unresponsive API Gateway is resolved. A timeout value of 0 means no timeout. The default timeout is 500 milliseconds and can be changed if required using JVM system property com.axway.rmi.socket.connect.timeout.|
|RDAPI-25915|01314483|**Issue**: Missing validation for Default Quota settings in API Manager server. **Resolution**: Default Quota settings are now validated in API Manager server.|
|RDAPI-25939|01314580|**Issue**: API Gateway Monitoring Metrics/Totals REST-API is not working, metrics missing. **Resolution**: The API Gateway Monitoring Metrics/Totals REST-API now returns all metrics.|
|RDAPI-25960|01314224|**Issue**: Error while creating an application with no APIs field set using REST API. **Resolution**: Now creating an application with no API fields set in the REST API request returns with a 201 status code.|
|RDAPI-26020|01314777|**Issue**: The 'Is the attributed contained in the cache'  filter fails to find a key in a cache when the cache is being updated using the same key at the same time by the 'Cache Attribute' filter due to Java thread interference. **Resolution**: The Java thread interference between the filters is removed, and simultaneous cache access using the same key by the 'Is the attributed contained in the cache' filter and the 'Cache Attribute' filter will no longer cause a failure.|
|RDAPI-26252|01317828|**Issue**: Proxy server only successfully authenticates with valid credentials when it is first used, all subsequent authentication attempts fail. **Resolution**: The proxy server now always successfully authenticates with valid credentials.|
|RDAPI-26363|01329862  01329120  01331657|**Issue**: Forward slash cannot be used in the scopes field for OAuth and OAuth (External). **Resolution**: Forward slash is now allowed in the scopes field for OAuth and OAuth (External).|
|RDAPI-26375|01327536|**Issue**: Trailing path parameters are not considered when selecting a best path match for an API request. **Resolution**: API method definitions that end with a path parameter are now considered correctly when determining a best path match for two similar paths.|
|RDAPI-26446|01329679|**Issue**: The Traffic Monitor REST API documentation has incorrect URLs and no example values for each endpoint. **Resolution**: The apidocs have been updated to show the correct URLs and example values.|
|RDAPI-26511|01333896|**Issue**: Due to the case-insensitive behavior of the Windows file system, manifest files and manifest directories in projects upgraded with projupgrade are not renamed with uppercase letters and as a result end up being removed. **Resolution**: The Projupgrade script now correctly handles case-insensitive manifest directories and manifest files when upgrading on Windows.|
|RDAPI-26572|01329616|**Issue**: Revoke access API for organization triggers an Internal Server Error. **Resolution**: API Manager correctly processes organizations that might be pending API access requests when revoking API access for an organization.|
|RDAPI-26578|01336428|**Issue**: Connect to URL Filter is failing with a ClassCastException error. **Resolution**: Headers handling was using the wrong method and that issue has been corrected.|
|RDAPI-26597|01337211|**Issue**: Update of API Gateway from v7.7.20210330 to v7.7.20210830 is failing to remove the `filterCircuit` field in `PortalConfiguration` migrate step 42. **Resolution**: The `PortalConfiguration` migrate step 42 now removes the `filterCircuit` field.|

## Known issues

The following are known issues for this update.

### Scripting filter whiteboard attributes not preloaded for Jython scripts

The Scripting filter now uses a Jython 2.7 scripting environment (previously, Jython 2.5) to execute Jython scripts. As a result of this version change, the whiteboard attributes, such as `http.request.uri` and `http.request.verb`, are no longer preloaded for use by Jython scripts. However, you can run a Jython script to load these attributes before they are accessed as follows:

```
from com.vordel.trace import Trace

def invoke(msg):
    msg.forceGenerateAttributes()
    Trace.info("This trace statement was generated in script filter!  [" + str(msg.get("http.request.verb")) + "] [" + str(msg.get("http.request.uri")) + "]")
    return True
```

Related Issue: RDAPI-21363

### When an API Gateway instance is started, Xerces SAXParserImpl writes warnings to the error console

At API Gateway instance startup, the following warnings are logged to the error console, as opposed to the trace log:

```
Warning: org.apache.xerces.jaxp.SAXParserImpl$JAXPSAXParser: Property 'http://javax.xml.XMLConstants/property/accessExternalDTD' is not recognized.
Warning: org.apache.xerces.jaxp.SAXParserImpl$JAXPSAXParser: Property 'http://www.oracle.com/xml/jaxp/properties/entityExpansionLimit' is not recognized.
```

These new properties were added in JAXP 1.5 specification, which is supported by the embedded implementation in the JRE but not supported yet in Xerces-J Apache implementation. These are harmless warning messages, which are written to the error console instead of throwing an exception if a property is not supported by the Apache Xerces-J implementation.

Related Issue: RDAPI-22218

### API Gateway web service WSDL schema validation failure

If a web service is defined using multiple WSDLs, an error of 'Cannot find the declaration of element' might occur during the schema validation of a SOAP message. This might happen because of a duplication of the WSDLs types schema `targetNamespace`. To avoid this failure, you must change the types schema `targetNamespace` to be unique across the WSDLs.

Related Issue: RDAPI-26621

### Policy Studio installation update issue

An intermittent issue exists whereby after an installation update of Policy Studio, an error dialog is shown - "An error has occurred, see the log file", and the product will fail to start. To resolve this issue, copy the `org.apache.jasper.glassfish_2.2.2.v201205150955.jar` from the Policy Studio installation backup plugins directory to the main Policy Studio plugins directory.

Related Issue: RDAPI-26743

### YAML performance issue in Policy Studio

A Policy Studio performance issue has been observed when utilizing the YAML entity store and running on the Windows operating system. This can result in increased waiting times for certain UI user actions.

Related Issue: RDAPI-26745

## Documentation

To find all available documentation for this product version:

1. Go to [Manuals on the Axway Documentation portal](https://docs.axway.com/bundle).
2. In the left pane **Filters** list, select your product or product version.

Customers with active support contracts must log in to access restricted content.

For information on the different operating systems, databases, browsers, and thick client platforms supported by each Axway product, see [Supported Platforms](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en).

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email [support@axway.com](mailto:support@axway.com) or visit [Axway Support](https://support.axway.com/).

See [Get help with API Gateway](/docs/apim_administration/apigtw_admin/trblshoot_get_help/) for the information that you should be prepared to provide when you contact Axway Support.
