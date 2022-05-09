---
title: API Gateway and API Manager 7.7 <month> 2021 Release Notes
linkTitle: API Gateway and API Manager <month> 2021
draft: true
weight: 100
date: 2021-01-06
description: API Gateway and API Manager updates are cumulative, comprising new features and changes delivered in previous updates unless specifically indicated otherwise in the Release notes.
---

<!--

This is a template for API Gateway and API Manager 7.7

To use it, copy and paste this file, then rename the file
-->

## Installation

* To **update** your API Gateway, see [Update from API Gateway One Version](/docs/apim_installation/apigw_upgrade/upgrade_steps_oneversion/).
* To **upgrade** from an older version, see [Upgrade from API Gateway 7.5.x or 7.6.x](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/).
* For more details on supported platforms for software installation, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).
* For a summary of the system requirements for a Docker deployment, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

### Update a container deployment

Any custom `.fed` files deployed to a container must be upgraded using [upgradeconfig](/docs/apim_installation/apigw_upgrade/upgrade_analytics#upgradeconfig-options) or [projupgrade](/docs/apim_reference/devopstools_ref#projupgrade-command-options). They must be upgraded the same way, regardless of whether they are API Manager enabled or not. The `.fed` files contain the updates for the API Manager configuration and can be used to build containers.

## New features and enhancements

The following new features and enhancements are available in this update.

### placeholder 1

placeholder text

For more information, see:

* [some reference 1](/docs/placeholder)
* [some reference 2](/docs/placeholder)

## Important changes

<!-- There are no major changes in this update. -->

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.

### placeholder 2

placeholder

## Deprecated features

As part of our software development life cycle we constantly review our API Management offering. In this update, the following capabilities have been deprecated.

### Antivirus filters

<!-- No features were deprecated in this release.-->

In the [January 2020](/docs/apim_relnotes/20200130_apimgr_relnotes/) update, we announced the deprecation of all the Antivirus filters in API Gateway. This is a reminder that in July 2021 we will remove the Antivirus filters from API Gateway. So, we recommend you to use the API Gateway's ICAP capability, which allows the gateway to integrate with ICAP capable external virus scanners.

## Removed features

<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, the following features have been removed: -->

No capabilities have been removed in this update.

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [Release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

table of security issues

### Other fixed issues

table of other issues

## Known issues

The following are known issues for this update.

| Internal ID | Description                                                                                                                                                                                                                                                                                                                                 |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|RDAPI-16486|Changes in the mapper always require a reload in the Execute Data Maps filter and once reloaded then providing values for the required parameters must be repeated|
|RDAPI-17282|Connector for Salesforce APIs in API Manager doesn't work or is impossible to configure|
|RDAPI-17395|APIGW Analytics - no data in DB during DB unavailability|
|RDAPI-18332|"Try-it" for API-Method is not working|
|RDAPI-18523|Inconsistent application search behaviour relating to application sharing|
|RDAPI-18601|EMT environmentalisation issue|
|RDAPI-18986|projpack is unable to merge projects after projupgrade; likely due to Default APIManager policies|
|RDAPI-18990|"Failed to delete undefined" window pops up unexpectedly when attempting to delete application|
|RDAPI-19217|Inconsistency between Application Developers and Account Settings pages in Manager|
|RDAPI-19292|When an APIM admin user's login name is changed, the user is directed to a blank page|
|RDAPI-19293|API Catalog Try It shows only the first security device of a security profile|
|RDAPI-19334|Access to retired APIs is not removed from other organizations as expected|
|RDAPI-19436|API approve/enable functionality for an organization does not show on application view|
|RDAPI-19442|Saving Mode Stuck for the Application Creation in different session|
|RDAPI-19601|Sharing section of Application issue when Organization of application changed|
|RDAPI-19742|API metrics ignore an API's organization|
|RDAPI-19743|API Broker not shown in circuit path on all failures|
|RDAPI-20527|xml2json filter, unable to use xml with valid namespace syntax|
|RDAPI-20593|Issue with Authentication profiles related to permethod override|
|RDAPI-20594|When a token is revoked with an incorrect Authorization header, the response is 400 instead of 401|
|RDAPI-20726|How to get attribute value for apimgmt.application.id|
|RDAPI-20742|Inconsistent deletion of Environmentalized Settings|
|RDAPI-20952|NullPointerException when opening a project with dependencies|
|RDAPI-21009|Issue after updating API Manager settings through rest api if lockUserAccount is missing|
|RDAPI-21061|updated error message, OAS3 file import without servers section url|
|RDAPI-21171|Minor UI issue affecting pagination in API keys and Oauth client credentials|
|RDAPI-21188|AWS SQS Poller does not seem to pass SQS Attributes|
|RDAPI-21211|APIM doesn't support SOAP 1.1 Binding for MTOM 1.0|
|RDAPI-21275|Application default quota in days produces "Cannot instantiate API constraint" error|
|RDAPI-21295|XSD files are not downloaded when Use client registry is enabled|
|RDAPI-21325|Improve load time performance of the Applications screen|
|RDAPI-21332|Cassandra 2.2.12 Vulnerabilities from latest scan|
|RDAPI-21384|Webservice (WSDL-based) responds with 500 instead of 405 when an invalid HTTP method is used|
|RDAPI-21411|Inconsistent treatment of multiple scopes in Oauth request|
|RDAPI-21423|Quota values getting re-initialized just after clicking quota text box|
|RDAPI-21438|Search box for Applications will not accept certain Unicode characters|
|RDAPI-21456|Application export doesn't include external credential|
|RDAPI-21514|Policy shortcut chain filter corrupts priority order when altering the sequence|
|RDAPI-21653|Custom Property Maximum Size|
|RDAPI-21675|API Gateway Manager > Messaging > {Queue}: display issue long name queue|
|RDAPI-21680|Method PATCH not visible api gateway - traffic monitor http filter|
|RDAPI-21770|Incorrect semantics of negated match types like IS_NOT in Compare Attribute filter|
|RDAPI-21875|User creation failing under small load|
|RDAPI-22147|API Manager GUI - API sorting issue|
|RDAPI-22164|Policy Studio UX issue, shows original, non environmentalized URL for DB|
|RDAPI-22197|Report display issue|
|RDAPI-22204|Wrong documentation on API Manager Swagger/OAS for corsOrigins|
|RDAPI-22221|libxml Error while importing WSDL|
|RDAPI-22331|automated submission form protection for forgottenpassword|
|RDAPI-22333|OAS 3 import implicitly requires a "servers" object, which should not be mandatory|
|RDAPI-22425|Missing HTTP Security Headers|
|RDAPI-22430|Issues removing custom policies from API Manager|
|RDAPI-22452|On APIMgr monitoring, application id is displayed instead of application name|
|RDAPI-22455|Self crosssite scripting vulnerability in API Manager|
|RDAPI-22459|Accept header in per-method override is not replacing header|
|RDAPI-22513|Package properties not visible, PS and CS tools on Linux|
|RDAPI-22599|APIM does accept colon (:) in query parameter name|
|RDAPI-22624|API GW audit.log cannot handle UTF-8, unlike trace log.|
|RDAPI-22671|XPath wizard - Unexpected behavior of 'Evaluate' button|
|RDAPI-22756|No longer able to search applications by ID on API Manager 7.7|
|RDAPI-22760|setting for EMT offload of audit.log files|
|RDAPI-22763|Need Support for McAfee Scan Engine 6200|
|RDAPI-22764|Slowness with APIGateway Policy Studio, Windows ver 1803 and 1809|
|RDAPI-22857| Renaming the apiadmin user via the server settings fails|
|RDAPI-22954|swagger imports fine, but comes out with error from catalog 2.0 link|
|RDAPI-22987|APIM login endpoint stops responding when pod has been running for 30 days|
|RDAPI-23017|Bigmsg leaks when client resets abruptly|
|RDAPI-23220|APIG crash caused by regular expression redaction|
|RDAPI-23222|Unable to create user as error pop up is displayed|
|RDAPI-23304|Trace file level is not persistent after Dynamic Setting change|
|RDAPI-23326|Cassandra CVE-2020-13946|
|RDAPI-23363|API Gateway does not support session recovery with SoftHSM dynamic instances|
|RDAPI-23379|API Catalog shows http and https base urls, customer wants only https|
|RDAPI-23470|STARTTLS broken in most SMTP modalities|
|RDAPI-23471|if custom API Proxy broker, customized backend service url is not kept in serviceprofiles in .dat export|
|RDAPI-23473|Packet sniffing|
|RDAPI-23553|JWT Decrypt filter not failing with invalid key|
|RDAPI-23557|TraceRedactor error:java.lang.RuntimeException: regex error with code: -10|
|RDAPI-23571|OAuth access tokens can be refreshed even after expiration when Cassandra TTL is NULL|
|RDAPI-23601|add header in inbound security for frontend doesn't appear anymore in params.header|
|RDAPI-23654|ANM logs passwords in plain text at DATA level|
|RDAPI-23655|When an APIM administrator changes a user's password, the user's session should be ended|
|RDAPI-23658|Deploy fails to an instance in API Manager group|
|RDAPI-23661|API Gateway replaces form-data characters in request|
|RDAPI-23665|Cert chain not imported when certificate is imported from a URL ending in ".local"|
|RDAPI-23696|No UUID Validation for API Key|
|RDAPI-23727|jan21 apimng loads application page twice|
|RDAPI-23729|Import config fragment produces inconsistent results when an API is partially imported in REST API reporsitory|
|RDAPI-23715|SSO users in API Manager are not logged out when logging out of API Portal|

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

### No VAPI matched request after upgrade from 7.5.3 SP12 using inbound security policy

If the following errors are present in the API Gateway traces when the instance is started, then there is a duplicate remote host configuration in API Gateway and API Manager configurations.

Duplicate remote host error:

```
ERROR 11/Mar/2021:11:10:27.207 [6dc4:000000000000000000000000] Failed to configure module:
...
com.vordel.api.common.ForbiddenException: PolicyStudio-registered remote host with name 'backend' and port '8080' already exists
 at com.vordel.apiportal.api.portal.controller.RemoteHostController.checkRemoteHost(RemoteHostController.java:616)
 at com.vordel.apiportal.api.portal.controller.RemoteHostController.updateRemoteHost(RemoteHostController.java:188)
 at com.vordel.apiportal.config.PortalConfiguration.addRemoteHosts(PortalConfiguration.java:354)
 at com.vordel.apiportal.config.PortalConfiguration.configure(PortalConfiguration.java:295)
```

Failure to configure circuit for VAPI error:

```
ERROR 11/Mar/2021:11:21:11.096 [6fb9:000000000000000000000000] Error configuring circuit for Front End (Proxy) API called [AA Petstore], Version [1.0.5], Organization [c33c32c5-f32e-4e38-8c52-1f149b7ebe9d]
ERROR 11/Mar/2021:11:21:11.096 [6fb9:000000000000000000000000] Error processing VAPI change event EventTimestamp [entityType=VIRTUALIZED_API, entityId=da281a2f-4e4d-4ce0-8747-a17614978a7f, eventType=CREATEUPDATE]:
java.lang.NullPointerException
 at com.vordel.apiportal.runtime.AuthenticationPolicySecurityDevice.exists(AuthenticationPolicySecurityDevice.java:182)
 at com.vordel.apiportal.runtime.AuthenticationPolicySecurityDevice.configure(AuthenticationPolicySecurityDevice.java:73)
```

To circumvent this problem, edit the API Gateway server configuration using Policy Studio and remove the duplicate remote host definition from **Environment Configuration > Listeners**, then deploy the updated configuration back to the server or servers in a group.

Related Issue: RDAPI-23549

### Policy Studio cannot upgrade Nodemanager or Analytics projects on import

Policy Studio cannot directly open or import Nodemanager or Anlytics projects or `.fed` files in this update. You must use the tools `projupgrade` and `upgradeconfig`, located in the `posix/bin/` folder, to upgrade the resources before they are imported in Policy Studio.

Related Issue: RDAPI-23867

## Documentation

<!-- This section describes documentation enhancements and related documentation.

### Documentation enhancements -->

There are no major changes in this update.

### Related documentation

To find all available documentation for this product version:

1. Go to [Manuals on the Axway Documentation portal](https://docs.axway.com/bundle).
2. In the left pane Filters list, select your product or product version.

Customers with active support contracts need to log in to access restricted content.

For information on the different operating systems, databases, browsers, and thick client platforms supported by each Axway product, see [Supported Platforms](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en).

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email [support@axway.com](mailto:support@axway.com) or visit [Axway Support](https://support.axway.com/).

See [Get help with API Gateway](/docs/apim_administration/apigtw_admin/trblshoot_get_help/) for the information that you should be prepared to provide when you contact Axway Support.
