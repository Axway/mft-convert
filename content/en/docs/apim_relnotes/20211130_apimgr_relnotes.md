---
title: API Gateway and API Manager 7.7 November 2021 Release Notes
linkTitle: API Gateway and API Manager November 2021
weight: 95
date: 2021-09-21
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

### Set Client Credentials as a Grant type

You can now enable OAuth authorization for access tokens as Client credential type to share resources from another server using a Client ID and a Client Secret. For more information, see [Setting Grant type - Client Credentials](/docs/apim_administration/apimgr_admin/api_mgmt_virtualize_web/#grant-type---client-credentials).

### New Amplify menu header

A new customisable header has been integrated to API Manager and API Gateway Manager. The new header allows you to enable search and help in multiple Axway portals, and to access the [Amplify platform](https://platform.axway.com/#/) portal. For more information see, [Customize Amplify menu header](/docs/apim_administration/apimgr_admin/api_mgmt_custom/#customize-amplify-menu-header).

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update, which may impact on your current installation.

### YAML support in Policy Studio

Policy Studio has initial support for YAML-based policies with known limitations to avoid data corruption. The following are not yet supported:

* Environmentalization of values.
* Environmentalization of fields from within Policy Studio.
* Creating a new project from an API Gateway instance using environmentalized values.
* Opening a YAML project with manually environmentalized values.
* Importing fragments with environmentalized values.
* Importing custom filters.
* Copy and paste options on the Policy tree view.
* Configure API Manager and Configure OAuth options in the **File** menu.

[Team Development](/docs/apigtw_devops/team_dev_practices/#when-to-use-team-development) has also been disabled for YAML-based projects because it is not a supported feature.

### Docker scripts now use Python 3

The Externally Managed Topology (EMT) scripts were upgraded from Python 2.7 to Python 3. For more information, see [Set up your Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/#set-up-your-docker-environment).

## Deprecated features

<!--As part of our software development life cycle we constantly review our API Management offering. As part of this update, the following capabilities have been deprecated-->

No features have been deprecated in this update.

## End of support notices

The following items are end of support:

### Developer tools on Windows 8.1

This is a reminder that in February 2022 we will remove support for the installation and update of [Developer tools](/docs/apim_installation/apigtw_install/install_dev_tools/) on Windows 8.1.

## Removed features

<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this update, the following features have been removed: -->

No features have been removed in this update.

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [Release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID                                | Cve Identifier | Description                                                                                                                                                                                                                                                                                     |
| ----------- | -------------------------------------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|RDAPI-22455 |01206099||**Issue**: Some error responses from the API Manager Backend can contain embedded javascript code which could execute in the browser during an API Manager UI session. **Resolution**: These responses no longer execute in the browser.|

### Other fixed issues

| Internal ID | Case ID                             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------- | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|RDAPI-20527|01161918, 01157471|**Issue**: In API Gateway, when using an XML to JSON filter, if the XML to convert contains an empty namespace, the conversion fails and an exception is thrown. **Resolution**: The XML to JSON filter now handles empty namespaces and converts them to empty string in the JSON result.
|RDAPI-21653|01189455|**Issue**: Custom properties for proxies were truncated to 256 characters when saved. **Resolution**: These properties are now truncated to 4096 characters.
|RDAPI-21875|01188062, 01197172, 01264148|**Issue**: UserCache gets out of sync with KPS when a particular user is created and deleted in succession as part of a loop. **Resolution**: UserCache now supports quick creation and deletion of an user.
|RDAPI-22430|01206355|**Issues**: Incorrect display of references to Server Settings in Policy Studio. **Resolution**: Correct display of references in "Show references" dialog, "Edit" dialog, policy context drop down and "Delete" dialog in Policy Studio.
|RDAPI-22452|01205592, 01252170, 01207342, 01289980, 01233924, 01275423|**Issue**: To improve performance, only a subset of Application information is loaded, but this causes API Manager Monitoring to show the application ID instead of its name in API Manager reports. **Resolution**: All required Application information is now loaded once the user navigates to API Manager Monitoring. Enabling the `com.axway.apimanager.api.data.cache` system property is advised for improved performance.
|RDAPI-22513|01208778, 01213952, 01230249, 01226840, 01272340, 01232746|**Issue**: Inconsistency in storing manifest files under uppercase or lowercase meta-inf directory was causing issues with exporting deployment, policy, or environment packages. Loading up of policy and environment properties in Policy studio under certain cases was impossible. **Resolution**: The meta-inf directory and manifest files are unified to be in uppercase letters across all code that creates or modifies content by that path.
|RDAPI-22671|01211047|**Issue**: Button "Evaluate" of XPath wizard always raise an error on click. **Resolution**: "Evaluate" button no longer raises an error when selectors are not used.
|RDAPI-23601|01240288|**Issue**: REST API Request Header Parameters validation fails when the header parameter is set by Add HTTP Header filter as part of an inbound security policy invoked by API Manager. **Resolution**: Header Parameters set by a policy are properly validated.
|RDAPI-23866|01244079, 01249010|**Issue**: API Manager Try-it does not have a valid SOAP payload. It is missing namespace on elements. **Resolution**: Namespace is now added to elements in SOAP payload.
|RDAPI-24043|01157280|**Issue**: Timeout for import certificates when virtualizing an API. **Resolution**: A connection timeout field can be configured in Policy Studio > Server Settings > General settings page to configure the default time to connect to a remote host. If a connection to a remote host is not established within the time set in this field, the connection times out and the connection fails. Defaults to 30000 milliseconds (30 seconds). You can configure Connection Timeout on a per-host basis using the Remote Hosts setting. For more information, see [Configure remote host settings](/docs/apim_policydev/apigw_gw_instances/general_remote_hosts).
|RDAPI-24153|01230272|**Issue**: In API Manager Swagger file, the definition for "API Proxy Registration" POST method doesn't mention that only one Virtualized API can be created per back-end API. **Resolution**: API Proxy Registration POST definition now states that only one Virtualized API can be created per back-end API.
|RDAPI-24471|01255253, 01306173|**Issue**: Performance issues when accessing API Manager UI through a VPN or on a slow network connection. **Resolution**: API Manager Management APIs now support 'gzip' and 'deflate' Content-Encoding for API requests that provide them in the Accept-Encoding header.
|RDAPI-24874|01273743|**Issue**: In API Gateway, when the String Replace Filter is configured to do straight match, strings containing regex syntax characters (for example, *) are not escaped and make the filter do a regex match. **Resolution**: String Replace Filter now escapes strings when configured to do straight match.
|RDAPI-24922|01277012|**Issue**: Error message "Exceeds maximum size (1MB)" is displayed when uploading an image via the Applications screen in API Manager, if the file name contains non-ASCII characters. **Resolution**: When uploading an image with non-ASCII characters, its name is now encoded so it can be safely interpreted by the backend.
|RDAPI-25072|01260463|**Issue**: Deploy Configuration to Group in API Gateway Manager shows only last status in Chrome/Edge. **Resolution**: Deploy Configuration window now shows the status of every deployment step. Also, the 'deploymentDelay' property at 'INSTALL_DIR/apigateway/webapps/emc/vordel/manager/app/app.config' file controls the time between the deployment steps to guarantee that the status is correctly displayed. Defaults to '500' milliseconds.
|RDAPI-25091|01283439, 01287447|**Issue**: In API Gateway, when using a XACML PEP filter, if the XACML response does not contain the required decision, the filter fails with an exception and the message property `xacml.result.xml` does not contain the correct value. **Resolution**: If the XACML response does not contain the required decision, XACML PEP filter correctly sets the message property `xacml.result.xml` and fails gracefully.
|RDAPI-25103|01281011|**Issue**: When GZIP is enabled in "Output encoding", API Gateway includes "Content-Encoding" header in OPTIONS responses which produces an invalid HTTP response because of the empty body. **Resolution**: API Gateway generates valid HTTP responses when GZIP is enabled in "Output encoding".
|RDAPI-25122|01297515, 01277755|**Issue**: Payload containing UTF-8 characters are not displayed correctly in Traffic Monitoring when viewed by users not belonging to administrator group. **Resolution**: UTF-8 payload contents are no longer altered by Node Manager when user is not an administrator.
|RDAPI-25167|01285737|**Issue**: Reset of API Administrator password fails with 'API Administrator does not exist' error (over 1000 users). **Resolution**: The API Gateway server is filtering out the users based on the expected API Administrator user DN and the API Administrator password is reset correctly.
|RDAPI-25174|01289758, 01284538, 01312333|**Issue**: SFTP public key authentication is always rejected with a signature error. **Resolution**: RSA private key signature operation not supported by OpenSSL is now delegated to a Java provider.
|RDAPI-25184|01281926|**Issue**: The Policy Studio Store Message filter doesn't show the generated attribute when Show All Attributes is selected. **Resolution**: The Policy Studio Store Message filter now shows the generated attribute when Show All Attributes is selected.
|RDAPI-25261|01253196|**Issue**: API Manager might be slow, or fail to create API access requests with large number of Applications and APIs. **Resolution**: API Manager API Client cache has been optimized to improve performance updating large number of Applications, and a race condition was addressed in API Catalog updating APIs. In addition, the new `com.axway.apimanager.runtime.poller.tracelevel` Java system property can be set to define trace level for the API Manager event poller background process only, independently from Default System Settings trace level. Default is INHERIT.
|RDAPI-25269|01287939|**Issue**: URI containing non encoded space character are truncated. **Resolution**: URI containing non encoded space characters are no rejected with a "400 Bad Request" response as this format does not respect standards. The old behavior can be switched back on by exporting environment variable 'AXWAY_REQUEST_INSECURE_PARSER=true' before starting instance.
|RDAPI-25282|01289584|**Issue**:  In API Manager, some APIs are shown as `API not available: <UUID>` in the API Access list after upgrading to 7.7.20210330. **Resolution**: Affected APIs are now shown properly in the API Access list after the upgrade.
|RDAPI-25321|01292842, 01284398, 01284221, 01291893|**Issue**: The default maximum value of requests per minute in API Manager during an active session is too low for some update scripts to upgrade API Manager to August 21 update, and the CI/CD system used by the update process does not allow you to change this maximum value using Policy Studio on the target system. This issue is blocking the update. **Resolution**: A new system property `com.axway.api.runtime.management.allowRateLimit` has been defined to allow customers to enable/disable the API Manager request rate limiter. The default is "true". To disable it, add `<ConfigurationFragment><VMArg name="-Dcom.axway.api.runtime.management.allowRateLimit=false"/></ConfigurationFragment>` to jvm.xml file.  In addition to this change, default rate limit has been increased to 5000.
|RDAPI-25388|01293335|**Issue**: The cookie "RESOURCEOWNERCOOKIE" cannot be extracted or set, an Invalid Cookie Name exception is thrown. **Resolution**: Cookie names are now correctly validated according to RFC 2616 accepting alphabet chars.
|RDAPI-25450|01294189, 01296003, 01294015|**Issue**: Regular expression for validation of email address does not conform with the syntax as defined in RFC5322. **Resolution**: Regular expression for validation of email address amended to conform with the syntax as defined in RFC5322 as follows: Flocalhost allowed as a domain. The length of a label of domain is between 1 and 63 characters. The length of the local part of email address is between 1 to 64 characters. The length limit of the domain part of email address is a maximum of 255 characters.The length limit of email address is a maximum of 320 characters.
|RDAPI-25471|01295743|**Issue**: Deploying policy and environment packages from API Gateway Manager UI is failing on Aug21 update. **Resolution**: Deploying policy and environment packages from API Gateway Manager UI now succeeds.
|RDAPI-25496|01289258|**Issue**: If a backend is not reachable, API Manager returns to the client a 500 Internal Server Error response including the headers from the request even if com.`axway.apigw.request.headers.reflect` is set to false. **Resolution**: Headers returned by API Manager in case of policy execution error are now filtered.
|RDAPI-25509|01296538, 01294832|**Issue**: Policy Studio no longer supported importing custom filters following refactoring to improve performance. **Resolution**: Custom Filters are supported in Policy Studio.
|RDAPI-25535|01297122|**Issue**: An API key can be defined as OAuth id or External Client id and still be valid and usable. **Resolution**: API keys must be defined in API key store,  OAuth ids must be defined in OAuth id stores, and External client ids must be defined in External Client store to be rendered as valid.
|RDAPI-25564|01281886, 01258472|**Issue**: Front-end API OAuth inbound security incorrectly returns 401 in a 503 cache loading situation. **Resolution**: Front-end API OAuth inbound security now returns 503 Service Unavailable error if the API client cache is not loaded yet.
|RDAPI-25701|01297508|**Issue**: Static File Provider handler is not using MIME types resulting in HTTP header Content-Type returned for UI pages not being correct. **Resolution**: Static File Handler is now returning the default MIME type when Content-Type parameter is not explicitly defined in the configuration. New MIME types have been added: `image/x-icon` for `.ico` files, and `application/javascript` for `.config` files.
|RDAPI-25726|01306173|**Issue**: Output encodings configured in a listener interface are overridden by an incoming remote host. **Resolution**: Now the output encodings configured in a listener interface are used if the incoming remote host output encodings are set to 'Default'. If the output encodings for the incoming remote host are explicitly set, e.g. 'gzip', then the output encodings from the incoming remote host are used. If the AXWAY_LEGACY_INCOMING_OUTPUT_ENCODINGS environment variable is set with any value then the output encodings in the listener interface will be overridden by an incoming remote host as before.
|RDAPI-25730|01306369, 01315367, 01312901, 01312956|**Issue**: API Manager remote hosts are lost when instance is restarted. **Resolution**: API Manager remote hosts are loaded and configured during instance startup.
|RDAPI-25897|01296372, 01153597|**Issue**: Multipart form-data requests with literals in the form of `${foo.bar.xxx` with no matching bracket were responded with 500 response because a runtime exception was thrown inside API Manager. **Resolution**: These kind of literals are now properly supported.
|RDAPI-25942|01315239|**Issue**: The apigw-backup-tool script fails to restore Cassandra 3.11.x due to how schema tables are different from Cassandra 2.2.x. **Resolution**: Fixed the apigw-backup-tool to handle both Cassandra 2.2.x and 3.11.x schema tables.

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

## Documentation

To find all available documentation for this product version:

1. Go to [Manuals on the Axway Documentation portal](https://docs.axway.com/bundle).
2. In the left pane **Filters** list, select your product or product version.

Customers with active support contracts need to log in to access restricted content.

For information on the different operating systems, databases, browsers, and thick client platforms supported by each Axway product, see [Supported Platforms](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en).

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email [support@axway.com](mailto:support@axway.com) or visit [Axway Support](https://support.axway.com/).

See [Get help with API Gateway](/docs/apim_administration/apigtw_admin/trblshoot_get_help/) for the information that you should be prepared to provide when you contact Axway Support.
