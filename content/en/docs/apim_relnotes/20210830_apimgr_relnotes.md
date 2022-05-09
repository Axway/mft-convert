---
title: API Gateway and API Manager 7.7 August 2021 Release Notes
linkTitle: API Gateway and API Manager August 2021
weight: 95
date: 2021-06-21
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

### Elastic-Stack Logging is available for API Gateway

The Elastic-Stack (Elasticsearch, Logstash, and Kibana) solution, version 3.2.0, is now production-ready for you to integrate with your API Gateway. For more information see, [Configure Elastic-Stack Logging](/docs/apim_administration/apigtw_admin/logging/#configure-elastic-stack-logging).

### YAML configuration store

It is now possible to deploy [YAML configuration](/docs/apim_yamles/) stores in the API Gateway Manager web UI on port 8090. For more information, see [Manage domain topology in API Gateway Manager](/docs/apim_administration/apigtw_admin/managetopology/#deploy-a-yaml-deployment-package).

### ICAP message preview

The ICAP filter now supports message preview ([RFC 3507, Section 4.5](https://datatracker.ietf.org/doc/html/rfc3507#section-4.5)). For more information, see [Message preview in Configure ICAP servers](/docs/apim_policydev/apigw_external_connections/common_icap_conf/#enable-message-preview-in-request-headers).

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update, which may impact on your current installation.

### McAfee Anti-Virus filter has been retired

The McAfee Anti-Virus filter has been retired in this update. This means that this filter is no longer available to use when creating policies in Policy Studio. Existing configurations that use the filter will show a warning when upgraded in Policy Studio or with an upgrade tool, such as `projupgrade`.

It is important that you update your policies and replace the McAfee Anti-Virus filter with the [Internet Content Adaption Protocol (ICAP)](/docs/apim_policydev/apigw_external_connections/common_icap_conf/) virus scan filter, or remove it entirely.

It will still be possible to deploy upgraded configurations containing the retired McAfee filter, and updated gateways will continue to run existing deployed configuration, but the McAfee filter, now deprecated, will show a warning in the gateway trace files and will always return a fail response.

### Support for Apache Cassandra 3.11.11

API Gateway now supports Apache Cassandra 3.11.11. For details on how to upgrade existing Apache Cassandra environments, see [Upgrade Apache Cassandra](/docs/apim_installation/apigw_upgrade/upgrade_cassandra/).

{{< alert title="Note" >}}Apache Cassandra version 2.2 onwards will no longer be supported by Apache Cassandra after their end of service date of April 2022. Axway will continue to provide support for these versions on a best efforts basis post April 2022, however, no critical updates will be available from Apache Cassandra from that time. Therefore, we recommend you to update your Cassandra installation to version 3.11.11 before that time.
{{< /alert >}}

### Unauthenticated request rate limiter is available in API Manager

You can now configure an unauthenticated request rate limiter in your API Manager. For more information, see [Configure API Manager unauthenticated request rate limiter](/docs/apim_administration/apimgr_admin/api_mgmt_config/#configure-api-manager-unauthenticated-request-rate-limiter).

### Notice of schedule change for updates

The cadence of the updates for API Gateway, API Manager, and API Portal has changed. From the August update onwards, the update schedule follows a three months release cycle.

## Deprecated features

<!--As part of our software development life cycle we constantly review our API Management offering. In this update, the following capabilities have been deprecated-->

No features have been deprecated in this update.

## End of support notices

<!-- There are no end of support notices in this update.-->

The following items are end of support:

### Apache Cassandra 2.2.x

Apache Cassandra version 2.2 onwards will no longer be supported by Apache Cassandra after their end of service date of April 2022. Axway will continue to support these versions on a best efforts basis, however, no critical updates will be available from Apache Cassandra from that time.

## Removed features

<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, the following features have been removed:-->

The following features have been removed:

### Antivirus filters

The McAfee Anti-Virus filter has been retired in this update.

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [Release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID                                | Cve Identifier | Description                                                                                                                                                                                                                                                                                     |
| ----------- | -------------------------------------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|RDAPI-22331|01209122||**Issue**: An unauthenticated user may send unlimited requests to API Manager's `/forgotpassword` endpoint. **Resolution**: Introduce an unauthenticated request rate limiter to monitor and limit the number of requests that an unauthenticated user can send to API Manager's `/forgotpassword` endpoint.|
|RDAPI-24359|01248057||**Issue**: The OCSP Client filter does not support the use of different CAs with the same DN. **Resolution**: When multiple CAs with the same DN are part of the certificate store, the OCSP Client filter will now select the correct CA for the certificate in use. **Issue**: In the OCSP Client filter configuration, options to validate the response are not greyed out when the "Do not validate" response is selected. **Resolution**: Response validation options are now only configurable when "Validate response" is enabled. **Issue**: Concurrent OCSP Client requests using dynamic configuration options can result in incorrect results. **Resolution**: A threading issue in the OCSP Client filter has been corrected and concurrent requests are now correctly executed.|
|RDAPI-24768|01270717||**Issue**: When invoking the API Gateway file API, one of the directories (conf-dir) discloses a file (envSettings.props) containing application related configuration. **Resolution**: A system property, `com.vordel.dwe.file.Service.includeConfDirectory`, has been added to control whether or not `conf-dir` and `envSettings.props` appear in the output of the API Gateway File API.|
|RDAPI-25068|01280382||**Issue**: Security Vulnerability - Time Based Username Enumeration **Resolution**: A new login response time (in milliseconds) property can be configured in the API Manager settings to delay the login response on a forbidden authentication error. The configured value range is from 1 to 60000 (1 minute). The login response time should be configured to ensure a successful login and an invalid user or password login take the same processing time in the API Manager setup. Defaults to 100 milliseconds. For more information, see [Analyzing Response Times section](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account) in the OWASP documentation.|
|RDAPI-25086|01281781||**Issue**: In API Manager UI, the grant type dialogs for OAuth inbound security are misleading - they suggest it is possible to configure grant types via these dialogs which is not possible. **Resolution**: It is now clear in these dialogs that their purpose is to update the API Catalog with relevant grant type information.|

### Other fixed issues

| Internal ID | Case ID                             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------- | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|RDAPI-18332|01112944|**Issue**: API Manager Try-It dialog from a front-end API or API Catalog method is not correctly rendering the request body for recursive references. **Resolution**: Try-It dialog correctly renders the request body.|
|RDAPI-21009|01175322|**Issue**: API config update request without lockUserAccount object breaks UI updates to API Manager settings. **Resolution**: API Manager config update requests without a lockUserAccount object now use a set of default values.|
|RDAPI-21384|01175637  01176810|**Issue**: Virtualized SOAP 1.2 Web Service in API Manager responds with "500 Internal Server Error" instead of "405 Method Not Allowed"  when an invalid HTTP method is used **Resolution**: Virtualized SOAP 1.2 Web Service now responds "405 Method Not Allowed" when an invalid HTTP method is used.|
|RDAPI-21675|01193426|**Issue**: In API Gateway Manager UI the Messaging Queues view does not display a long queue name correctly. **Resolution**: Long queue names are now correctly displayed in the Queues view.|
|RDAPI-21770|01156958  01187661  01157558|**Issue**: In the 'Compare Attribute' filter, negated match types return the wrong boolean value. **Resolution**: Updated the 'Compare Attribute' and 'LDAP RBAC' filters to return the correct boolean value for negated match types as configured in the 'Evaluate to true for negated match types on null values' field.|
|RDAPI-22147|01240459  01204857  01251076|**Issue**: Sorting backend APIs by the "created on" column in API Manager UI does not display APIs in chronological order, rather it sorts them alphabetically by the displayed date. **Resolution**: Sorting by this column now displays APIs in chronological order.|
|RDAPI-22756|01264393  01218766  01241841  01214471|**Issue**: API Manager UI does not support filtering applications by the application's ID. **Resolution**: The applications list can now be filtered by ID.|
|RDAPI-22764|01253152  01269024  01277729  01251271  01270346  01246532  01253900  01205016  01274197  01276375|**Issue**: Policy Studio is slow at startup, creating projects and opening filters. **Resolution**: Policy Studio class loading is improved and now Policy Studio is faster at startup, creating projects, and opening filters.|
|RDAPI-23557|01237958|**Issue**: A trace redaction record containing binary or non-UTF-8 data could cause product stability issues. **Resolution**: Binary and non-UTF-8 data is now silently redacted by default. This can be overridden by setting the new 'utf' configuration flag in the regular expressions' configuration to 'true'.|
|RDAPI-23658|01221630  01230554|**Issue**: API Gateway instances get blocked on shutdown, or during deployment, by transactions waiting for JMS filters to exit. **Resolution**: When JMS Services are stopped, transactions executing JMS filters are now unblocked and an error condition is raised.|
|RDAPI-23963|01253196|**Issue**: API Manager might be slow, or fail to create API access requests with a large number of Applications and APIs. **Resolution**: API Manager API Client cache has been optimized to improve performance updating large numbers of Applications, and a race condition was addressed in API Catalog updating APIs.In addition, the new `com.axway.apimanager.runtime.poller.tracelevel` Java system property can be set to define trace level for the API Manager event poller background process only, independently from Default System Settings trace level. Default is INHERIT.|
|RDAPI-23981|01242085|**Issue**: It is not possible to configure the number of entity expansions limit or the number of content model nodes for API Gateway XML Schema validation. **Resolution**: You can use the Java system property jdk.xml.entityExpansionLimit to configure the number of entity expansions limit, and the jdk.xml.maxOccurLimit property to configure the number of content model nodes. In the jvm.xml file, add `<ConfigurationFragment><VMArg name="-Djdk.xml.entityExpansionLimit=5000" /><VMArg name="-Djdk.xml.maxOccurLimit=10000" /></ConfigurationFragment>`. For more information, see [System property change log](/docs/apim_reference/system_props/). |
|RDAPI-24011|01254384  01282465  01265503  01267027  01255145  01284961  01280232|**Issue**: Policy Studio  doesn't open projects with dependencies using recent projects links from Home view. **Resolution**: In Policy Studio the following broken functionality has been restored:- Open project from link in the Home view:-- If the project is not opened yet, the project is opened and shown in the configuration view.-- If the project is already opened, the configuration view is selected and displayed.- With 'Team Development' enabled, adding or removing a project dependency reuses the existing project configuration view.Also, if the environmentalized fields decorator exists then it is disabled and initialized on configuration view refresh.|
|RDAPI-24256|01223923|**Issue**: In API Gateway, an OPTIONS request always returns a "200 Success" status, even when handled by a custom Policy that could set another status code, like  for example, through a "Reflect message" filter. **Resolution**: API Gateway now respects what status code custom policies set when handling OPTIONS requests.|
|RDAPI-24285|01256211|**Issue**: In API Manager, when setting Preflight Max-age in the CORS profile section, the UI allows negative or non-integer values to be entered. **Resolution**: Validation has been added to both the UI and the backend to gracefully reject invalid values in the Preflight Max-age field.|
|RDAPI-24324|01260184  01259252|**Issue**: Process memory growing when an incoming connection is closed before zip or deflate content is fully processed.  **Resolution**: The memory leak in zlib caused by this behavior has been corrected.**Issue**: Process memory slowly growing when using WebSockets. **Resolution**: The memory leak caused by OpenSSL objects not being released by the WebSocket layer has been corrected.|
|RDAPI-24360|01256550|**Issue**: In API Gateway, a regression in the "XML Complexity" filter logic is preventing the usage of Java aliases for the charset when passed as part of the Content-Type header. **Resolution**: The "XML Complexity" filter now accepts both IANA charsets and any valid Java alias as part of the Content-Type header.|
|RDAPI-24371|01211819  01251135|**Issue**: API Gateway CRL Dynamic and CRL Responder filters throw decode error. **Resolution**: API Gateway CRL retriever service is now closing down correctly to avoid undefined scheduler behavior that may lead to the spontaneous hanging of API Gateway on configuration redeployment or process shutdown. Corrected CRL update scheduler to avoid unexpected CRL updates. Optimized memory usage and CRL list decoding by the CRL Dynamic and CRL Responder filters. Addressed security issues with the storage of cached CRL files. Improved performance of the certificate revocation process.|
|RDAPI-24487|01283007  01283336  01261414  01284598|**Issue**: In API Gateway, when a remote host with "Allow HTTP 1.1" and "Include Content Length in response" checked is configured, an OPTIONS request will result in an invalid HTTP response missing a Content-Length header. **Resolution**: API Gateway now returns a valid HTTP response with Content-Length header to an OPTIONS request when a remote host is configured with "Allow HTTP 1.1" and "Include Content Length in response" checked.|
|RDAPI-24490|01265464|**Issue**: Policy Studio upgrade triggered upon opening a project always adds Server Settings node. **Resolution**: The Server Settings node is no longer added if the opened project did not previously have Server Settings.|
|RDAPI-24496|01260519  01261991|**Issue**: Certificate cache was not cleared when closing a Configuration Studio project. This was causing spurious errors related to certificate duplication. **Resolution**: Certificate cache is now cleared when closing a Configuration Studio project.|
|RDAPI-24503|01264377|**Issue**: API Manager UI issue when the user requests API Access in Application, the Approve/Reject buttons and warning message are not fully visible. **Resolution**: The Application API Access table now adds a horizontal scrollbar if needed and fully displays any warning messages if present.|
|RDAPI-24516|01264369|**Issue**: Unpublished APIs can no longer be upgraded after the [November 2020](/docs/apim_relnotes/20201130_apimgr_relnotes/) (v7.7.20201130) update. **Resolution**: The upgrade restriction was reverted to the previous behavior, and unpublished APIs can again be upgraded with the v1.3 API. In the v1.4 API, unpublished APIs can be upgraded by an Organization Administrator only if the api.manager.orgadmin.selfservice.enabled Java system property is set to true (defaults to false).|
|RDAPI-24517|01263929|**Issue**: SameSite cookie attribute was set to 'Strict' in all circumstances. **Resolution**: SameSite cookie attribute can now be set in Policy Studio filters, API Manager configuration, and Client Registry configuration.|
|RDAPI-24536|01257877|**Issue**: Upgrading a project from 7.5.3 to 7.7 using the projupgrade script clears environmentalized references to entities that are in a sub-project. **Resolution**: A new `com.axway.env.settings.removeMissingReferredEntity` java system property is added to control the setting of environmentalized references to missing entities that are in a sub-project during upgrade. By default it is set to true and environmentalized references to missing entities are unset. The projupgrade script sets the property to false if not provided and environmentalized references to entities in sub-projects are not unset.|
|RDAPI-24564|01266364|**Issue**: If an exception occurs when loading a ScriptingFilter at startup/deploy time, there is no easy way to identify which filter is causing the problem. **Resolution**: The error trace now clearly identifies the script filter causing the exception.|
|RDAPI-24579|01214337|**Issue**: In Policy Studio, it is not possible to create a mapping link using a function when an unqualified XML schema is used in a  Data Map. **Resolution**: It is now possible to create a mapping link using a function if an unqualified XML schema is used in the Data Map.|
|RDAPI-24581|01268234|**Issue**: API Gateway Content-Type filter is not validating SOAP MTOM requests correctly, resulting in broken MTOM services. **Resolution**: API Gateway Content-Type filter now correctly validates SOAP MTOM message Content-Types. |
|RDAPI-24597|01261069|**Issue**: Swagger API lookup queries performed by organization administrators and users are significantly slower than identical requests performed by API administrators. **Resolution**: Unnecessary database transactions have been removed, resulting in these queries performing as quickly for organization administrators and users as for API administrators.|
|RDAPI-24626|01263068|**Issue**: API Manager does not invoke API method specific Fault Handler. **Resolution**: API Manager API Broker has been corrected to invoke the Fault Handler configured for the corresponding API method.|
|RDAPI-24630|01287085  01268930|**Issue**: When using an ICAP filter, if the ICAP server returns a 204 response code, API Gateway is not populating HTTP response message attributes (for example "http.response.status"). **Resolution**: API Gateway now populates correctly the HTTP response message attributes when an ICAP server returns a 204 response code.|
|RDAPI-24646|01269872|**Issue**: Test resource is located in the incorrect directory for salesforce-apiconnector jar. **Resolution**: The test resource has been moved to the correct test directory and is no longer included in the released jar.|
|RDAPI-24657|01268651  01270387|**Issue**: JWT Sign behavior change for "Selector expression" with a Certificate alias as asymmetric key stopped working. **Resolution**: JWT Sign with asymmetric key accepts again a Certificate alias for "Selector expression".|
|RDAPI-24767|01271378|**Issue**: API Manager backend API drop-down list unordered in the 'create new frontend API' dialog. **Resolution**: The 'create new frontend API' dialog now displays a case-sensitive sorted backend API list by name and version. This list can be searched based on the keyboard key pressed, highlighting the first API starting with the key pressed; hitting again the same key highlights the next matching API.|
|RDAPI-24785|01271488|**Issue**: No debug level trace to identify missing Analytics Database rows. **Resolution**: Added debug level trace to identify missing Analytics Database rows for Metrics aggregation.|
|RDAPI-24808|01271829|**Issue**: API Gateway Analytics returns a blank CSV report when the From field in a scheduled report is set to Yesterday. **Resolution**: CSV reports now display the correct information when the From field in a scheduled report is set to Yesterday.|
|RDAPI-24850|01274322|**Issue**: Form-data request with repeating form parameters causes a server error. **Resolution**: Form-data request with repeating form parameters is properly processed and no exception is thrown.|
|RDAPI-24851|01272418  01273790|**Issue**: API Gateway database connection pool leak when "Connection reset" occurs on a database connection. **Resolution**: API Gateway database connection pool error handling now processes abandoned connections and returns them to the pool.In addition, the new `com.axway.apigw.dbconnection.removeabandoned` Java system property allows removal of abandoned connections if they exceed the abandoned connection timeout set in the below system property. Defaults to 'true'. Also, the new `com.axway.apigw.dbconnection.removeabandoned.timeoutms` Java system property sets the abandoned connection timeout in milliseconds. Defaults to '300000'. Also, the new `com.axway.apigw.dbconnection.testonreturn` Java system property allows validation of connections before they are returned to the pool. Defaults to 'true'.|
|RDAPI-24879|01247698  01190619  01153597  01188911|**Issue**:  multipart/form-data parameter types were changed in order to avoid issues with files parts being treated as text. However, the change did not fully address this issue (in terms of file parts). **Resolution**: multipart/form-data parameter types are now fully handled as per spec, see: [RFC 7578 section 4.2](https://datatracker.ietf.org/doc/html/rfc7578#section-4.2)|
|RDAPI-24912|01264369|**Issue**: API Gateway can be blocked when redeploying API Manager configuration. **Resolution**: The API Manager API Client Cache service thread has been corrected to avoid spontaneous hanging while processing API Client KPS data.|
|RDAPI-24936|01262539  01276087  01230222  01241996|**Issue**: HTTP Strict Transport Security header is not added when connecting with some SSL clients. Also, Host header for TLS connection includes default port 443. **Resolution**: API Gateway was corrected to process TLS connections, and the corresponding HTTP headers are now set respectively, including the HTTP Strict Transport Security and Host headers.|
|RDAPI-24946|01269102  01276749|**Issue**: RAML API definitions break API Manager after upgrading from 7.7.0-2019-04-12 rev. 59de774 to 7.7.20210330 and it was difficult to know which API was created using RAML. **Resolution**: At API Manager startup there is an error trace if an invalid payload type is detected containing the resource ID and type. A debug trace is also added that identifies the loaded APIs and associated resource IDs. This can be used to find the API matching the resource ID in the error trace.|
|RDAPI-25249|01258866  01269927  01257081  01253734  01276115  01278358  01268921  01256706  01228890  01281231|**Issue**: API Manager runtime only accepts empty values for query string parameters and it always reports a missing parameter when a parameter is overridden with an outbound proxy parameter. Also, legacy APIs failed processing because of enum parameters validation. **Resolution**: API Manager now accepts empty values for any query parameter data type based on Swagger allowEmptyValue property, if this property is present.If the Swagger property is not present, the `com.vordel.coreapireg.runtime.broker.parameters.allowEmptyDefault` Java system property is used instead.Defaults to 'false'. And the new `com.axway.api.runtime.broker.parameters.skipRequiredValidation` Java system property allows to skip required parameters validation during processing of user requests when it is set to 'true' . Defaults to 'false'.In addition, you can set the new `com.axway.api.runtime.broker.parameters.skipEnumValidation` Java system property to 'true' to skip enum parameters validation only. Defaults to 'false'.|

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
