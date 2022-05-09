---
title: API Gateway and API Manager 7.7 January 2021 Release Notes
linkTitle: API Gateway and API Manager January 2021
weight: 100
date: 2021-01-31
description: API Gateway and API Manager updates are cumulative, comprising new features and changes delivered in previous updates, unless specifically indicated otherwise in the Release notes.
---

## Installation

* To **update** your API Gateway, see [Update from API Gateway One Version](/docs/apim_installation/apigw_upgrade/upgrade_steps_oneversion/).
* To **upgrade** from an older version, see [Upgrade from API Gateway 7.5.x or 7.6.x](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/).
* For more details on supported platforms for software installation, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).
* For a summary of the system requirements for a Docker deployment, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

### Update a container deployment

Any custom `fed` files deployed to a container must be upgraded using [upgradeconfig](/docs/apim_installation/apigw_upgrade/upgrade_analytics#upgradeconfig-options) or [projupgrade](/docs/apim_reference/devopstools_ref#projupgrade-command-options). They must be upgraded the same way, regardless of whether they are API Manager enabled or not.

The `.fed` files contain the updates for the API Manager configuration and can be used to build containers.

## New features and enhancements

The following new features and enhancements are available in this update.

### Option to disable SSL renegotiation is added for HTTPS listeners and Connection filters

We have added a new option, **Disable renegotiation in TLSv1.2 and earlier**, in Policy Studio to allow you to disable SSL renegotiation when configuring HTTPS interfaces or connecting to URL filters. This option is enabled by default, meaning that renegotiation is not allowed, and listeners do not send HelloRequest messages, and they ignore renegotiation requests via ClientHello.

For more information, see:

* [Configure Advanced SSL settings](/docs/apim_policydev/apigw_gw_instances/general_services/#configure-advanced-ssl-settings)
* [Connect to URL filter](/docs/apim_policydev/apigw_polref/routing_common/)

### Backup passphrase parameter to restore operation in kpsadmin

We have added a new `--passphrase` parameter to the restore operation in `kpsadmin`.
If the parameter is not specified the script will prompt for a passphrase. The passphrase parameter is provided to allow data that may be encrypted at rest, be decrypted when being restored into the target API Gateway environment. For more info, see [Manage KPS using kpsadmin](/docs/apim_policydev/apigw_kps/how_to_use_kpsadmin_command/).

### Content Security Policy header to improve security

The Content Security Policy (CSP) header sets a policy that instructs the browser to only fetch resources, such as scripts, images, or objects, from the specified locations. A compliant browser will deny loading any resources from locations not listed in the policy. The CSP header reduces an attacker's ability to inject malicious content and helps to protect a web page from attacks like Cross-Site Scripting (XSS), dynamic code execution, and clickjacking.

For more information, see [Define a restrictive content security policy](/docs/apim_installation/apiportal_install/secure_harden_portal/#define-a-restrictive-content-security-policy).

### YAML configuration store (Technical preview capability)

This update includes bug fixes and enhanced functionality for YAML configuration as follows:

* Support for [certificates and keys](/docs/apim_yamles/yamles_edit/#add-a-new-certificate-and-private-key-to-a-yaml-configuration) in standard PEM files.
* Enhanced support for managing more configuration content in [externalized files](/docs/apim_yamles/yamles_externalized_files).
* Restructured [entity type](/docs/apim_yamles/apim_yamles_references/yamles_types/) information into separate files to enable custom type support.
* Enhanced policy readability.
* Enhanced support for environmentalization with encryption.
* Fix issue of reordering fields in YAML files after configuration edits via tooling.
* Support for `${env.CERT}` environmentalization of certificates in the YAML configuration.

See the [September 2020](/docs/apim_relnotes/20200930_apimgr_relnotes/#yaml-configuration-store-technical-preview-capability) release notes for an overview of this technical preview, and the [YAML configuration](/docs/apim_yamles/) documentation for more detailed information.

## Important changes

<!-- It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.. -->

There are no major changes in this update.

## Deprecated features

<!-- As part of our software development life cycle we constantly review our API Management offering. In this update, the following capabilities have been deprecated. -->

In the [January 2020](/docs/apim_relnotes/20200130_apimgr_relnotes/) update, we have announced the deprecation of all the Antivirus filters in API Gateway. This is a reminder that in July 2021 we will remove the Antivirus filters from API Gateway. So, we recommend you to use the API Gateway's ICAP capability, which allows the gateway to integrate with ICAP capable external virus scanners.

## Removed features

No capabilities have been removed in this update.

<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, the following features have been removed: -->

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [Release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID            | Cve Identifier                               | Description                                                                                                                                                                                                                                                                                                                   |
| ----------- | ------------------ | -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-19851 | 01155164           | CVE-2018-10237 CVE-2018-1324 CVE-2018-11771  | **Issue:** API Gateway included `commons-compress-1.14.jar`, which contains vulnerabilities CVE-2018-1324 and CVE-2018-11771, and `guava-20.jar`, which contains vulnerability CVE-2018-10237. **Resolution:** API Gateway now includes `commons-compress-1.20.jar` and `guava-30.0-jre.jar`, and it is no longer vulnerable. |
| RDAPI-22021 | 01184311  01184328 | CVE-2019-13118 CVE-2019-13117 CVE-2019-11068 | **Issue**: API Gateway includes libxlst library that has known vulnerabilities. **Resolution**: API Gateway is update to use libxslt 1.1.34 that addresses known vulnerabilities.                                                                                                                                             |
| RDAPI-22685 | 01218205           | CVE-2020-1971                                | **Issue**: API Gateway was including OpenSSL 1.1.1g version which has known vulnerabilities. **Resolution**: API Gateway is updated to include OpenSSL 1.1.1i addressing CVE-2020-1971.For more details, see [OpenSSL Security Advisory 08 December 2020](https://www.openssl.org/news/secadv/20201208.txt)                   |
| RDAPI-22716 | 01216866  01216225 |                                              | **Issue**: There was a security vulnerability of Persistent Cross Site Scripting attack (XSS) in the API Gateway Manager UI. It interprets the data written in Key Property Store (KPS) as code and thus executes what is stored inside a KPS entry. **Resolution**: Security vulnerability has been fixed.                   |

### Other fixed issues

| Internal ID | Case ID                                                    | Description                                                                                                                                                                                                                                                                                                                                                                       |
| ----------- | ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-15981 | 01111041                                                   | **Issue**: Scope fields are visible for API Keys in API Manager when Application Scopes are disabled. **Resolution**: Scope fields are now only visible when Application Scopes are enabled.                                                                                                                                                                                      |
| RDAPI-18198 | 01094525  01098372                                         | **Issue**: In API Manager, OPTIONS requests for POST on a WSDL API would fail if trailing slash was absent from the path. **Resolution**: API Manager can now produce correct OPTIONS responses on WSDL APIs no matter if trailing slash is present or not.                                                                                                                       |
| RDAPI-19354 | 01142425                                                   | **Issue**: Importing a swagger with duplicate consumes or produces entries into API Manager succeeds and the duplicates are persisted. **Resolution**: Duplicates are now removed on import.                                                                                                                                                                                      |
| RDAPI-19580 | 01140662                                                   | **Issue**: In API Manager, trials were not started if, on login, a password change is required. **Resolution**: API Manager has been updated so that the trials will be started before the user is directed to change their password.                                                                                                                                             |
| RDAPI-19586 | 01135092                                                   | **Issue**: API Catalog was not displaying the correct state for APIs in the API Manager UI. **Resolution**: API Catalog now displays the correct state for APIs.                                                                                                                                                                                                                  |
| RDAPI-19702 | 01144547                                                   | **Issue**: A valid sequence of actions resulted in an error popup "entity could not be found" being displayed in the API Manager UI. **Resolution**: This popup no longer appears following this sequence of actions.                                                                                                                                                             |
| RDAPI-19788 | 01139823                                                   | **Issue**: The "page size" drop down field was sometimes inconsistent with the number of objects actually being displayed in API Manager UI. **Resolution**: The "page size" drop down field now correctly reflects the amount of objects on each page.                                                                                                                           |
| RDAPI-20480 | 01155158                                                   | **Issue**: Enum query parameters were not being validated by API Manager at runtime. **Resolution**: Enum query parameters are now validated at runtime.                                                                                                                                                                                                                          |
| RDAPI-20550 | 01168067  01164507                                         | **Issue**: In Policy Studio, the entity store was breaking whenever a KPS Collection was renamed due to broken links caused by entity changes being overriden. **Resolution**: Updated the entity store to ensure that, when an entity requires two updates, the second update will not override the first update.                                                                |
| RDAPI-20793 | 01169981                                                   | **Issue**: API Manager was duplicating the resource path for 1.2 swagger in some cases. **Resolution**: API Manager no longer duplicates the resource path in any case.                                                                                                                                                                                                           |
| RDAPI-21113 | 01156840  01164189                                         | **Issue**: API Manager responds with "500 Internal Server Error" when an API uses Outbound authentication and a method query-string parameter contains `%25` (valid encoding for a percent sign: `%`). **Resolution**: API Manager behaves as expected when an API uses Outbound authentication and a method query-string parameter contains a valid encoding for a percent sign. |
| RDAPI-21337 | 01183703                                                   | **Issue**: Axway Open Documentation Database support only shows SQL Server version which are past end of life. **Resolution**: Newer SQL Server versions added to the documentation.                                                                                                                                                                                              |
| RDAPI-21444 | 01184175                                                   | **Issue**: Policy Studio cannot add, delete or rename scripts from **Resources > Scripts** navigation tree node. **Resolution**: Scripts can be added, renamed, or deleted from **Resources > Scripts** navigation tree. node.                                                                                                                                                    |
| RDAPI-21589 | 01189546  01215714                                         | **Issue**: The ICAP Filter will set the attribute `http.request.verb` to REQMOD after receiving a "No Content" response. **Resolution**: The ICAP Filter now keep the attribute `http.request.verb` from the original request after receiving a "No Content" response.                                                                                                            |
| RDAPI-21681 | 01183925  01189487                                         | **Issue**: Swagger API blob not updated after updated API import that uses a modified Swagger document located at the same URL as original import. **Resolution**: Swagger API blob is updated to the latest version of the Swagger document.                                                                                                                                     |
| RDAPI-21916 | 01193100  01198428                                         | **Issue**: WAF cannot access JSON payload using OWASP_CRS 3.2.0. **Resolution**: JSON payloads are properly parsed when using WAF.                                                                                                                                                                                                                                                |
| RDAPI-22258 | 01206856                                                   | **Issue**: Incorrect status messages were being logged due to the misidentification of the response status returned by a request. **Resolution**:  Response processing has been updated so that the response status is correctly identified.                                                                                                                                      |
| RDAPI-22362 | 01208320                                                   | **Issue**: Library platform/lib/engines/chil.so is missing. **Resolution**: The Chil library no longer exists for OpenSSL 1.1. Security engines requiring it are incompatible with this new version. References to `nfast` and `chil` engines have been removed from the `vpkcs11` template.                                                                                      |
| RDAPI-22424 | 01202329                                                   | **Issue**: SameSite attribute is not present in  API Gateway cookies. **Resolution**: SameSite attribute is now set in API Gateway server cookies.                                                                                                                                                                                                                                |
| RDAPI-22652 | 01211524                                                   | **Issue**: When selecting traffic monitoring records with a threat protection status of blocked, returned list is always empty. **Resolution**: Threat protection filter 'Exception' has been removed, and filter 'Block' has been updated to correctly match blocked requests.                                                                                                   |
| RDAPI-22676 | 01217234  01217207                                         | **Issue**: API Manager fails to validate API requests if an optional query parameter is not sent. **Resolution**: API Manager validation of optional query parameters with `allowEmptyValue` set to True has been corrected.                                                                                                                                                      |
| RDAPI-22709 | 01217571  01216689                                         | **Issue**: Breaking change made connection to the embedded SFTP server impossible by remote part using weak ciphers. **Resolution**: Use of weak ciphers for the embedded SFTP server can now be re-enabled by setting system property 'com.axway.apigw.sftp.knowninsecure.allow=true'.                                                                                           |
| RDAPI-22874 | 01216051                                                   | **Issue**: Configuring Raw Redactor or Trace Redactor to redact groups that are not part of the regular expression output causes a product crash. **Resolution**: Redacted groups that do not match when a regular expression is executed, are now ignored.                                                                                                                       |
| RDAPI-22903 | 01220687                                                   | **Issue**: *ProjPack* utility generates potentially different configuration values for encrypted fields with passphrase none. **Resolution**: *ProjPack* preserves configuration values for none project and target passphrases.                                                                                                                                                  |
| RDAPI-22955 | 01223877  01225742  01226996  01226982  01226992  01227000 | **Issue**: API Manager Runtime is incorrectly validating header parameters case-sensitively. **Resolution**: Header parameters are now validated case-insensitively according to RFC7230.                                                                                                                                                                                         |

## Known issues

The following are known issues for this update.

| Internal ID | Description                                                                                                                                                                                                                                                                                                                                 |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-22573 | Service pack update fails with the following message when passphrases are in use: `"Problem connecting to store: Invalid passphrase for configuration"`.                                                                                                                                                                                    |
| RDAPI-16486 | Changes in the mapper always require a reload in the Execute Data Maps filter and once reloaded then providing values for the required parameters must be repeated                                                                                                                                                                          |
| RDAPI-17282 | Connector for Salesforce APIs in API Manager doesn't work or is impossible to configure                                                                                                                                                                                                                                                     |
| RDAPI-18332 | "Try-it" for API-Method is not working                                                                                                                                                                                                                                                                                                      |
| RDAPI-18523 | Inconsistent application search behavior relating to application sharing                                                                                                                                                                                                                                                                    |
| RDAPI-18601 | EMT environmentalization issue                                                                                                                                                                                                                                                                                                              |
| RDAPI-18986 | projpack is unable to merge projects after projupgrade; likely due to Default APIManager policies                                                                                                                                                                                                                                           |
| RDAPI-18990 | "Failed to delete undefined" window pops up unexpectedly when attempting to delete application                                                                                                                                                                                                                                              |
| RDAPI-19292 | When an APIM admin user's login name is changed, the user is directed to a blank page                                                                                                                                                                                                                                                       |
| RDAPI-19293 | API Catalog Try It shows only the first security device of a security profile                                                                                                                                                                                                                                                               |
| RDAPI-19442 | Saving Mode Stuck for the Application Creation in different session                                                                                                                                                                                                                                                                         |
| RDAPI-19601 | Sharing section of Application issue when Organization of application changed                                                                                                                                                                                                                                                               |
| RDAPI-20527 | xml2json filter, unable to use xml with valid namespace syntax                                                                                                                                                                                                                                                                              |
| RDAPI-20593 | Issue with Authentication profiles related to permethod override                                                                                                                                                                                                                                                                            |
| RDAPI-20726 | How to get attribute value for apimgmt.application.id                                                                                                                                                                                                                                                                                       |
| RDAPI-20742 | Inconsistent deletion of Environmentalized Settings                                                                                                                                                                                                                                                                                         |
| RDAPI-20952 | NullPointerException when opening a project with dependencies                                                                                                                                                                                                                                                                               |
| RDAPI-21009 | Issue after updating API Manager settings through rest api if lockUserAccount is missing                                                                                                                                                                                                                                                    |
| RDAPI-21061 | updated error message, OAS3 file import without servers section url                                                                                                                                                                                                                                                                         |
| RDAPI-21171 | Minor UI issue affecting pagination in API keys and Oauth client credentials                                                                                                                                                                                                                                                                |
| RDAPI-21275 | Application default quota in days produces "Cannot instantiate API constraint" error                                                                                                                                                                                                                                                        |
| RDAPI-21295 | XSD files are not downloaded when Use client registry is enabled                                                                                                                                                                                                                                                                            |
| RDAPI-21384 | Webservice (WSDL-based) responds with 500 instead of 405 when an invalid HTTP method is used                                                                                                                                                                                                                                                |
| RDAPI-21411 | Inconsistent treatment of multiple scopes in Oauth request                                                                                                                                                                                                                                                                                  |
| RDAPI-21423 | Quota values getting re-initialized just after clicking quota text box                                                                                                                                                                                                                                                                      |
| RDAPI-21434 | "Expect to Retrieve X rows" setting on database query filter                                                                                                                                                                                                                                                                                |
| RDAPI-21438 | Search box for Applications will not accept certain Unicode characters                                                                                                                                                                                                                                                                      |
| RDAPI-21456 | Application export doesn't include external credential                                                                                                                                                                                                                                                                                      |
| RDAPI-21653 | Custom Property Maximum Size                                                                                                                                                                                                                                                                                                                |
| RDAPI-21675 | API Gateway Manager > Messaging > {Queue}: display issue long name queue                                                                                                                                                                                                                                                                    |
| RDAPI-21680 | Method PATCH not visible api gateway - traffic monitor http filter                                                                                                                                                                                                                                                                          |
| RDAPI-21697 | problems with recent change to multipart form-data RDAPI-20461                                                                                                                                                                                                                                                                              |
| RDAPI-21770 | Incorrect semantics of negated match types like IS_NOT in Compare Attribute filter                                                                                                                                                                                                                                                          |
| RDAPI-21875 | User creation failing under small load                                                                                                                                                                                                                                                                                                      |
| RDAPI-22087 | API Manager - Duplicate header if header is overwritten in inbound security policy                                                                                                                                                                                                                                                          |
| RDAPI-22147 | API Manager GUI - API sorting issue                                                                                                                                                                                                                                                                                                         |
| RDAPI-22197 | Report display issue                                                                                                                                                                                                                                                                                                                        |
| RDAPI-22204 | Wrong documentation on API Manager Swagger/OAS for corsOrigins                                                                                                                                                                                                                                                                              |
| RDAPI-22221 | libxml Error while importing WSDL                                                                                                                                                                                                                                                                                                           |
| RDAPI-22247 | CWE-548 - Static Content Providers default to "Allow directory listings"                                                                                                                                                                                                                                                                    |
| RDAPI-22331 | automated submission form protection for forgottenpassword                                                                                                                                                                                                                                                                                  |
| RDAPI-22333 | OAS 3 import implicitly requires a "servers" object, which should not be mandatory                                                                                                                                                                                                                                                          |
| RDAPI-22430 | Issues removing custom policies from API Manager                                                                                                                                                                                                                                                                                            |
| RDAPI-22442 | Error when saving ENV file in Configuration Studio-Linux                                                                                                                                                                                                                                                                                    |
| RDAPI-22452 | On APIMgr monitoring, application id is displayed instead of application name                                                                                                                                                                                                                                                               |
| RDAPI-22455 | Self crosssite scripting vulnerability in API Manager                                                                                                                                                                                                                                                                                       |
| RDAPI-22459 | Accept header in per-method override is not replacing header                                                                                                                                                                                                                                                                                |
| RDAPI-22599 | APIM does accept colon (:) in query parameter name                                                                                                                                                                                                                                                                                          |
| RDAPI-22624 | API GW audit.log cannot handle UTF-8, unlike trace log.                                                                                                                                                                                                                                                                                     |
| RDAPI-22671 | XPath wizard - Unexpected behavior of 'Evaluate' button                                                                                                                                                                                                                                                                                     |
| RDAPI-22679 | KPS REST API - Stack trace disclosed in error message                                                                                                                                                                                                                                                                                       |
| RDAPI-22756 | No longer able to search applications by ID on API Manager 7.7                                                                                                                                                                                                                                                                              |
| RDAPI-22760 | setting for EMT offload of audit.log files                                                                                                                                                                                                                                                                                                  |
| RDAPI-22763 | Need Support for McAfee Scan Engine 6200                                                                                                                                                                                                                                                                                                    |
| RDAPI-22954 | swagger imports fine, but comes out with error from catalog 2.0 link                                                                                                                                                                                                                                                                        |
| RDAPI-22573 | Service pack update fails with the following message when passphrases are in use: `"Problem connecting to store: Invalid passphrase for configuration"`. As a workaround, you can wait for the purge task, which runs every 10 minutes on the NodeManager process, to remove old gateway configurations before starting the update process. |

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