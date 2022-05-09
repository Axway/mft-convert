---
title: API Gateway and API Manager 7.7 March 2021 Release Notes
linkTitle: API Gateway and API Manager March 2021
weight: 95
date: 2021-03-31
description: API Gateway and API Manager updates are cumulative, comprising new features and changes delivered in previous updates unless specifically indicated otherwise in the Release notes.
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

### Updated cipher scheme

The cipher scheme for all encrypted data in the system (such as Database, LDAP passwords, Private keys, and so on) has been enhanced to use PBKDF2 (Password based key derivation function 2) with more secure parameters. This reduces vulnerability to brute force attacks.

This feature has introduced changes related to how sensitive data is managed, how data can be encrypted in custom libraries or Policy Studio script filters, and how data can be migrated between environments, for example, from this release onwards, encrypted KPS data can no longer be transferred directly between environments.

For more information, see [Update cipher scheme](/docs/apim_installation/apigw_upgrade/upgrade_steps_oneversion/#update-cipher-scheme).

### Passphrase policy enforcement

A new passphrase policy and two new endpoints to manage the policy have been introduced to enforce the policy when the node manager or their group's passphrases are changed via:

* managedomin script
* PUT /deployment/passphrase/nodemanager/{serviceID}
* PUT /deployment/passphrase/group/{groupID}

With a suitably strict passphrase policy enabled, the user will no longer be able to select extremely weak passphrases, such as `password` or `1234`. For more information see, [Configure a passphrase policy](/docs/apim_administration/apigtw_admin/manage_user_access/#configure-a-passphrase-policy-for-node-managers-and-api-gateway-groups).

### Security enhancements to JWT Sign and Verify filters

Significant changes have been made to the JWT Sign and Verify filters to comply with PSD2. These filters are now much more closely aligned with the [RFC 7515](https://tools.ietf.org/html/rfc7515) and [RFC 7519](https://tools.ietf.org/html/rfc7519) specifications.

Note that API Gateway still does not support:

* JWS JSON Serialization
* Nested JWTs
* Unsecured JWTs

For more information, see:

* [JWT Sign filter](/docs/apim_policydev/apigw_polref/integrity_additional/#jwtsign-filter)
* [JWT Verify filter](/docs/apim_policydev/apigw_polref/integrity_additional/#jwtverify-filter)

### Automatic upgrade of projects in Policy Studio

After applying [API Gateway One Version](/docs/apim_installation/apigw_upgrade/upgrade_steps_oneversion/) update to Policy Studio, opening a pre-existing project will now offer to automatically upgrade the project.

For more information, see [Upgrade of project after 7.7 One Version update](/docs/apim_policydev/apigw_poldev/gs_project/#upgrade-of-project-after-77-one-version-update).

### API Manager request rate limiter

Rate limit monitors the number of requests that a user can send to API Manager during an active session. If the number of requests in an individual session exceeds the configured boundaries, the session is terminated and the user must log in again to continue using API Manager.

For more information, see [Configure the API Manager request rate limiter](/docs/apim_administration/apimgr_admin/api_mgmt_config/#configure-api-manager-request-rate-limiter).

### HTTP strict transport security profile

In order to be compliant with security best practices, HTTP Strict Transport Security (HSTS) support has been added to API Gateway. Profiles can be configured within Policy Studio.

For more information, see [Configure HTTP strict transport security](/docs/apim_policydev/apigw_gw_instances/configure_http_strict_transport_security).

### Certification with MySQL 8

API Gateway is now certified as compatible with MySQL 8. For more information see, [Third Party JDBC Drivers](/docs/apim_installation/apigtw_install/metrics_db_install/#add-third-party-jdbc-driver-files).

### YAML configuration store (Technical preview capability)

This update includes bug fixes and enhanced functionality for YAML configuration as follows:

* YAML configuration templates.
* Add support for YAML configuration in Topology API.
* Support for YAML configuration in multi-node topology.
* Entities with same key fields at the same hierarchy level is now fully supported.
* CLI import sub-command now comes with new options to relax some constraint and facilitate import.

See the [September 2020](/docs/apim_relnotes/20200930_apimgr_relnotes/#yaml-configuration-store-technical-preview-capability) release notes for an overview of this technical preview, and the [YAML configuration](/docs/apim_yamles/) documentation for more detailed information.

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.

### Apache Cassandra security advisory

The version of the `libthrift` library within Cassandra database is vulnerable to *Improper Access Control*. To mitigate this, we recommend you to upgrade the `libthrift` library to version `0.9.3-1` in your [Apache Cassandra installation](/docs/apim_installation/apigtw_install/cassandra_install/).

### Changes in JWT filters

There is now a distinction between JSON Web Signature (JWS), where the payload can be of any content type, and a JSON Web Token (JWT), which must be a valid JSON. The system defaults to JWS after you update the product.

Note also that for [JWT verification](/docs/apim_policydev/apigw_polref/integrity_additional/#jwtverify-filter), the previous **None** option for shared-key HMAC signing has now been replaced with a checkbox. If **None** was selected in the old filter, then the checkbox will be disabled after migration, resulting in the same behavior.

### New Cipher schemes for configuration and KPS

New cipher schemes, which are used to encrypt relevant data in configuration and in KPS, have been added to API Gateway. These changes impact how sensitive data is encrypted and managed, how data can be migrated between environments, and how data can be encrypted in custom libraries or policy studio script filters.

For more information on the cipher scheme and the passphrase policy enforcement feature, see [Update cipher scheme](/docs/apim_installation/apigw_upgrade/upgrade_steps_oneversion/#update-cipher-scheme).

### Driver for metrics database

An update was made to the MySQL JDBC driver version to support MySQL8, and the driver is no longer compatible with Maria DB. Therefore, it is not possible to support both Maria DB and MySQL8 databases together. We plan to rectify this restriction in a later update.

For more information, see [Install and configure a metrics database](/docs/apim_installation/apigtw_install/metrics_db_install/#add-third-party-jdbc-driver-files).

## Deprecated features

As part of our software development life cycle we constantly review our API Management offering. In this update, the following capabilities have been deprecated:

### Antivirus filters

In the [January 2020](/docs/apim_relnotes/20200130_apimgr_relnotes/) update, we announced the deprecation of all the Antivirus filters in API Gateway. This is a reminder that in July 2021 we will remove the Antivirus filters from API Gateway. So, we recommend you to use the API Gateway's ICAP capability, which allows the gateway to integrate with ICAP capable external virus scanners.

### End of Support notices

The following items are end of support (EOS):

* API Gateway and API Manager [7.7 January 2020 update](/docs/apim_relnotes/20200130_apimgr_relnotes/).
* Internet Explorer 11 and earlier versions are no longer supported. Microsoft Edge is recommended.
* MySQL 5.6.

### Removed features

<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, the following features have been removed: -->

No capabilities have been removed in this update.

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [Release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID            | Cve Identifier                               | Description      |
| ----------- | ------------------ | -------------------------------------------- | ---              |
|RDAPI-23337 | | CVE-2018-1320 CVE-2019-0205 | **Issue**: Apache Cassandra includes `libthrift` library which has known vulnerabilities. **Resolution**: New installations, which opt to install Cassandra from the API Gateway installation packages, include the upgraded `libthrift` 0.9.3-1 library that addresses these two known vulnerabilities. Existing customers are advised to upgrade the `libthrift` library to 0.9.3-1 in their Cassandra environments.|
|RDAPI-22247|01207324||**Issue**: Newly created Static Content Providers enable "Allow directory listings" by default. **Resolution**: Newly created Static Content Providers "Allow directory listings" is disabled by default.|
|RDAPI-22559|01215211  01215001||**Issue**: Missing protection mechanisms on binaries. **Resolution**: API Gateway native code and customized libraries are now built with protection mechanisms in place. Because of the compiler used, protection for stack canaries can only be achieved using `-fstack-protector` to avoid performance penalties. As a consequence, some libraries are reported as "No RELRO" or "No canary found". The following libraries under the `<INSTALL_DIR>/apigateway/Linux.x86_64/lib` folder are delivered as-is (not generated as part of the API Gateway solution): libdb-4.3.so, liblnxfv.so.4, liblua-5.1.so, libobaccess.so, libpcap.so.0.9.8, libsigar-amd64-linux.so, libsqlite3.so, libstdc++.so.5.0.7, libz.so.1.2.5. For the same reason, the following binaries under the `<INSTALL_DIR>/apigateway/Linux.x86_64/bin` folder are delivered as-is: wkhtmltopdf/libcrypto.so.1.0.0, wkhtmltopdf/libssl.so.1.0.0, wkhtmltopdf/wkhtmltopdfRPATH for vshell is intended and required.|
|RDAPI-22708|01217185||**Issue**: API Manager does not provide a Session Timeout that will stop users to keep a session open indefinitely. Only Idle Session Timeout is provided. **Resolution**: API Manager now provides a Session Timeout alongside the Idle Session Timeout.|
|RDAPI-23200|01229206|CVE-2020-13956|**Issue**: API Gateway included Apache HttpClient version 4.5.9, which has vulnerability CVE-2020-13956.  **Resolution**: API Gateway now includes Apache HttpClient version 4.5.13, and is no longer vulnerable.|
|RDAPI-23553|01237742||**Issue**: JWT Decrypt filter successfully decrypts even when an invalid key is provided. **Resolution**: JWT Decrypt filter now fails as it should when an invalid key is provided.|

### Other fixed issues

| Internal ID | Case ID                                          | Description               |
| ----------- | ------------------------------------------------ | -------------------------- |
|RDAPI-18777|01124550|**Issue**: In API Manager, when you remove all restrictions from an application quota, 'Override default application quota' is left checked and the default application quota does not apply to that application. **Resolution**: If we do not provide any restriction to the default application quota, 'Override default application quota' is unchecked and the default application quota applies if present.|
|RDAPI-20474|01165533|**Issue**: API Manager takes long time to process APIs that contain hundreds or thousands of API methods. API Manager transactions processing is blocked when an API is virtualized or changes are made to a virtualized API. Also, it was possible to virtualize a back-end API more than once. **Resolution**: The virtualization of an API or changes to a virtualized API do not block API Manager transactions processing. Improved API Manager performance when importing, virtualizing, updating, exporting, and deleting APIs with large number of API methods. A back-end API can now only be virtualized once. In addition, the following Java system property improves performance of the API Gateway configuration load at startup and deployment from Policy Studio or managedomain: `com.axway.apimanager.configure.apis.nonblocking.enabled` enables non-blocking multi-threaded load of virtualized APIs brokers. Defaults to False.|
|RDAPI-21697|01247698  01190619  01153597  01188911|**Issue**: API Manager APIs fail validation of integer fields in the multipart/form-data. **Resolution**: Integer fields in multipart/form-data are now properly validated.|
|RDAPI-22087|01201660  01216870|**Issue**: If a front-end API is using an inbound security set to invoke a policy, the invoked policy used is overwriting a header using an 'Add HTTP Header' filter with the overwrite existing header option. Since the API method has this same header defined a parameter, the call to the back-end API is done with a duplicate header, each of the header having a different value, one with value passed as part of the client API call, and the other value is the one set by the policy. **Resolution**: The API call now only contains one header with the value that the policy overwrites, the one set by the client API call.|
|RDAPI-22417|01222760  01211150  01227691|**Issue**: API Gateway Manager change password form failed to update account passwords. **Resolution**: The change password form now successfully updates account passwords.|
|RDAPI-22431|01161798  01242061  01172963|**Issue**: Impossible to use HSM keys for Cassandra SSL. **Resolution**: The "Vordel" Java Security Provider is now selected first and implementation of "RSASSA-PSS" signature algorithm has been added to it. The Java KeyStore interface (PKCS8) has been corrected to support HSM and OpenSSL engine keys.  Debugging traces for SSL/TLS layers and PKCS11 engine have been improved.|
|RDAPI-22534|01214405|**Issue**: On API Gateway upgrade, instances running on other installations are incorrectly included when checking for running instances.  **Resolution**: The instance check has been updated to only consider instances running on the current installation.|
|RDAPI-22679|01217498|**Issue**: A CWE-209 was detected in the KPS API wherein it leaks Jersey error messages to callers on certain request. **Resolution**: Error messages have been fixed. They no longer contain sensitive information.|
|RDAPI-22710|01212407|**Issue**: EMT script fails setup when using a password protected env file. **Resolution**: Script updated to accept password parameter for env file.|
|RDAPI-22904|01219454|**Issue**: Adding a Trusted Certificate to an API Manager front-end API fails domain validation if a URL with a top level domain that is not a valid public domain, such as local, is used. **Resolution**: Adding a Trusted Certificate to an API Manager front-end API now supports user defined top level domains, which can be configured using the Java system property `com.axway.apimanager.user.tlds` as follows: `<ConfigurationFragment><VMArg name="-Dcom.axway.apimanager.user.tlds=local,locala" /></ConfigurationFragment>`|
|RDAPI-22979|01225687|**Issue**: The `upgradeconfig` script was unclear about the consequences of upgrading an `*.env` archive without a `*.pol` archive. **Resolution**: A warning has been added to the script. |
|RDAPI-22988|01218858|**Issue**: In Policy Studio, when exporting an API, referring policies can be partially exported, which causes broken dependencies. **Resolution**: Referring policies will now be automatically exported unless removed by the user.|
|RDAPI-23021|01223740|**Issue**: Custom profiles are not correctly set for API Manager APIs when virtualized. **Resolution**: Custom profiles are now set at virtualization, otherwise the default profile is used.|
|RDAPI-23023|01187719  01185390|**Issue**: Swagger 2 body parameter with `allOf` causes API Manager display issue. **Resolution**: API Manager now displays `allOf:Type1,Type2,...` as body data type in API Catalog.|
|RDAPI-23173|01231941  01232072|**Issue**: API Manager unable to import Swagger definitions containing nested `allOf` schemata. **Resolution**: Swagger definitions with nested `allOf` schemata are imported.|
|RDAPI-23191|01231602|**Issue**: Query parameter validation allows an empty parameter value as a default, this goes against the OpenAPI specification.  **Resolution**: Validation now adheres to the spec but the default behaviour can be determined by using the following JVM Setting. To permit empty query parameters by default, add the following line to the apigateway/conf/jvm.xml: `<VMArg name="-Dcom.vordel.coreapireg.runtime.broker.parameters.allowEmptyDefault=true" />`|
|RDAPI-23195|01232219  01236906|**Issue**: External OAuth security in a front-end API produces an invalid OAS3 document. **Resolution**: Downloaded OAS3 document is now valid.|
|RDAPI-23451|01239111|**Issue**: Verb field in API Method definition form is editable and vulnerable to an XSS attack. **Resolution**: Verb field is no longer editable and vulnerable to XSS attack. It can be only set by selecting a value from a given list.|
|RDAPI-23472|01236639|**Issue**: API key incorrectly passed to backend even when "remove credentials on success" is chosen. **Resolution**: API Key is removed after a successful authentication when "remove credentials on success" is chosen.|
|RDAPI-23513|01241098|**Issue**: Back-end APIs with duplicated parameters were imported silently, there's not alert about the duplicated parameters. **Resolution**: When Swagger 1.x or Swagger 2 APIs with duplicate parameters are imported, each duplication is reported as an error for the customer to review as the API may exhibit problems at runtime.|
|RDAPI-23616|01239777|**Issue**: The migration REST API for application doesn't work in API Gateway Januar 21 update. **Resolution**: If the export `options.filename` option isn't set, the /migrate/applications/export/download API returns an internal server error indicating the error condition. The API also accepts override of the export `options.filename` option if a filename is provided as part of the request, otherwise the `options.filename` is used.|
|RDAPI-23683|01243563  01245489|**Issue**: External OAuth device authentication failures are not written in the apigateway/events log. Only successful authentications are logged. **Resolution**: External OAuth device authentication failures are properly written in the apigateway/events log.|
|RDAPI-23685|01243959|**Issue**: API Manager runtime was rejecting requests pertaining to APIs that contained parameters with special characters; the logic used to perform the parameter definition lookup was incorrect when dealing with such parameters. **Resolution**: The API Manager runtime logic has been updated to better cater for such use-cases.|
|RDAPI-23727|01246357|**Issue**: API Management January 21 release loads API Manager applications page twice. **Resolution**: Applications page loaded once in API Manager.|
|RDAPI-23772|01213157  01213178|**Issue**: Configuring multiple certificate realms on the same device slot result in private keys not being loaded for user being already logged in. **Resolution**: PKCS11 errors for user already logged in and library already initialized will no longer avoid private keys from being loaded. **Issue**: Using a SoftHSM that requires same OpenSSL version as product results in a dead lock or crash on startup. **Resolution**: PKCS11 providers are now loaded out of OpenSSL's initialization phase to avoid locking conflicts.|

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
