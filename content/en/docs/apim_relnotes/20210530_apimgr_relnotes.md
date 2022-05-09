---
title: API Gateway and API Manager 7.7 May 2021 Release Notes
linkTitle: API Gateway and API Manager May 2021
weight: 95
date: 2021-04-26
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

### Dependency view and revoke access to front-end APIs

API Manager allows Organization Administrators and API Administrators to grant access to their APIs within any organization. Now, you can also view the usage of APIs, where access has been granted to organizations and their applications, as well as revoking their access. For more information, see [Manage API access](/docs/apim_administration/apimgr_admin/api_mgmt_virtualize_web/#manage-api-access).

### Import APIs over HTTPS through a HTTPS proxy server

It is now possible to send an API download request to a HTTPS proxy server over HTTPS. This ensures the confidentiality of the download request as the request is sent over HTTPS.

If using a proxy server to import APIs is a requirement, then it is recommended to do so by using a HTTPS Proxy server. For more information, see [Configure a proxy server](/docs/apim_administration/apimgr_admin/api_mgmt_config/#configure-a-proxy-server).

### YAML configuration store (GA)

The YAML configuration store feature reached General Availability (GA) in this update of API Gateway, and it is now production-ready.

The YAML configuration store provides a more CI/CD/DevOps and developer-friendly means for creating and managing API Gateway configuration. Tooling is provided, which allows for the conversion of the federated configuration into YAML fragments that can be managed using standard DevOps tools, moving away from a proprietary team development approach. This enables the use of standard source control and DevOps tools that facilitate and encourage a better and more collaborative experience.

This initiative focuses on:​

* Fine-grained configuration for an improved development collaboration experience​.
* Managing configuration as code for a developer-friendly​ experience.
* Design for improved DevOps capability (CLI tooling, extended environmentalization).

{{< alert title="Note" color="primary" >}} The XML configuration store is the default format and is still supported. {{< /alert >}}

We strongly encourage our customers to explore the possibilities of the new configuration format and provide feedback to us on this experience.

For more information, see:

* [Axway API Management User Group - Learn how to DevOps your configuration](https://community.axway.com/s/article/Axway-APIM-User-Group-Learn-how-to-DevOps-your-Configuration).
* [YAML configuration reference documentation](/docs/apim_yamles/).
* [Axway University YAML Entity Store](https://university.axway.com/learn/course/internal/view/elearning/1362/yaml-entity-store) free course.

To follow-up on what's coming based on this capability, see [API Management Roadmap](https://community.axway.com/s/api-management).

#### Update API Gateway with technical preview YAML configurations deployed

This procedure is valid **for API Gateway May 21 update only**. Later versions will upgrade the YAML configuration automatically when running a service pack update.

Perform these steps to update an API Gateway installation, which has YAML configuration from the [March 21 update](/docs/apim_relnotes/20210330_apimgr_relnotes/#yaml-configuration-store-technical-preview-capability) deployed:

1. For each API Gateway installation, back up your deployed YAML configurations.
2. Deploy a simple **federated store**, as a placeholder, to replace all deployed YAML configurations. For example, a configuration created with Policy Studio using a template.
3. Stop any running gateways.
4. Run `yamles upgrade --targz` on your backed up configuration to create a `.tar.gz` file. For more information, see [Upgrade YAML configuration](/docs/apim_yamles/apim_yamles_cli/yamles_cli_upgrade).
5. Update [API Gateway server](/docs/apim_installation/apigw_upgrade/upgrade_steps_oneversion/).
6. Deploy your upgraded YAML configurations, using `managedomain` or `projdeploy` scripts, to replace the placeholder configuration. For more information, see [Package and deploy a YAML configuration](/docs/apim_yamles/yamles_packaging_deployment).

#### Support for MariaDB 10.5

The product now supports MariaDB 10.5.

For more information, see [Install and configure a metrics database](docs/apim_installation/apigtw_install/metrics_db_install.md).

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update, which may impact on your current installation.

### Notice of schedule change for updates

The cadence of the updates for API Gateway, API Manager, and API Portal is changing. From the May update onwards, the update schedule will change from every two months to every three months.

{{< alert title="Note" color="primary" >}}The next update is now scheduled for August 2021.{{< /alert >}}

<!-- RDAPI-21747 -->

### Embedded ActiveMQ host name verification

Host name verification was introduced to alleviate a potential CWE-933 security risk, and it is enabled by default on the Embedded ActiveMQ cluster of new API Gateway installations, which choose to use Embedded Active MQ with SSL enabled. Host name verification is now disabled for customers who use Embedded Active MQ with SSL enabled or who are updating or upgrading the product, as this will result in JMS queues unable to service requests. Enabling host name verification when using SSL is a more secure option and should be considered as part of the upgrade.

Note that enabling host name verification requires a certificate update. For more information, see [Embedded ActiveMQ settings in Policy Studio](/docs/apim_reference/general_activemq_settings/).

### INSTALL_DIR/apigateway folder permissions changed

The permissions on `INSTALL_DIR/apigateway` folder have been restricted down on Linux from `755` to `750` (read/write/execute for owner, read/execute for group, no permissions for world) to provide additional protection for sensitive content, including files and sub-folders. With this change, access is denied to users who are not an owner or part of the group read, write, and execute permissions.

{{< alert title="Note" color="primary" >}}It is recommended to verify any business process or custom scripts that access files in this directory as this is potentially a breaking change.{{< /alert >}}

### Changes to JWT Verify filter

There are new output options that can be configured for the JWT Verify filter in Policy Studio. For more information, see [JWT Verify - Output](/docs/apim_policydev/apigw_polref/integrity_additional/#verify-output).

<!-- RDAPI-22777 -->

### Analytics CSP Header

For users of the Analytics application, the default Content Security Policy header for Analytics has been enhanced to be more restrictive and secure in the content which it accepts. If loading non-standard content into the Analytics UI, you might need to adjust the CSP headers setting in `envProps.settings`, under the `env.ANALYTICS.CONTENTSECURITYPOLICY` field.

<!-- RDAPI-23947 -->

### Authorization records threshold

Some customers with large number of OAuth tokens have experienced a significant slowdown in response times within API Manager after logging in. To address this situation two changes have been made. Firstly, OAuth authorizations are now only loaded when a user visits the OAuth Authorizations screen itself. Secondly, because of the limited ability of Cassandra to paginate data, a Java system property has been introduced whereby the screen is no longer populated if the number of tokens is above a configured threshold. The property is called `com.vordel.oauthAuthorizationRecordsThreshold`, and it defaults to `-1`, meaning no change in behavior compared to before. Setting this value to, for example, `10000`, means that if there are more than ten thousand tokens in the Cassandra table, then the screen is not populated and a warning message is display to inform the user.

<!-- RDAPI-23648 -->

### Replacement of MD5 hashed API Gateway configuration files with SHA256

API Gateway configuration files were previously provided with an accompanying MD5 hashed version, to provide a means of verifying data integrity. The MD5 version of the files has now been removed and replaced with a stronger hashed version using SHA256. Customers that have carried out integrity checks on the MD5 version should now switch to the SHA256 files instead.

### New SHA-256 hash algorithm option on SFTP server fingerprint check

To improve security in API Gateway, a new `SHA-256 hash` algorithm option was added to [File Upload](/docs/apim_policydev/apigw_polref/routing_additional/#file-upload-filter) and [File Download](/docs/apim_policydev/apigw_polref/routing_additional/#file-download-filter) routing filters, and [FTP Poller](/docs/apim_policydev/apigw_gw_instances/general_ftp_scanner/) on SFTP server fingerprint check.

The `SHA-256` algorithm option is designed to replace the existing `MD5` algorithm, and it is advisable to use it now as it is more secure.

## Deprecated features

As part of our software development life cycle we constantly review our API Management offering. In this update, the following capabilities have been deprecated:

### Antivirus filters

In the [January 2020](/docs/apim_relnotes/20200130_apimgr_relnotes/) update, we announced the deprecation of all the Antivirus filters in API Gateway. This is a reminder that in August 2021 we will remove the Antivirus filters from API Gateway. So, we recommend you to use the API Gateway's ICAP capability, which allows the gateway to integrate with ICAP capable external virus scanners.

### Packet Sniffing

The packet sniffing capability is deprecated from this update. The removal date of this feature will be communicated in upcoming releases. The packet sniffing capability addresses very specific edge cases, and as there are more mature opensource tools available, it is not seen as a strategic capability for API Gateway.

### SFTP server fingerprint check (MD5 hash algorithm)

SFTP server fingerprint check using `MD5` hash algorithm is deprecated on [File Upload](/docs/apim_policydev/apigw_polref/routing_additional/#file-upload-filter), [File Download](/docs/apim_policydev/apigw_polref/routing_additional/#file-download-filter), and [FTP Poller](/docs/apim_policydev/apigw_gw_instances/general_ftp_scanner/) for security reasons, and will be removed in the future. You must use `SHA-256` instead.

### End of Support notices

The following items are end of support (EOS):

* MariaDB 5.5

### Removed features

<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, the following features have been removed: -->

No capabilities have been removed in this update.

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [Release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID                                | Cve Identifier | Description                                                                                                                                                                                                                                                                                     |
| ----------- | -------------------------------------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-21747 |                                        | CWE-933        | **Issue**: TLS host name verification is not configurable and cannot be enabled for Embedded ActiveMQ. **Resolution**: TLS host name verification is now configurable, and it is enabled by default.                                                                                            |
| RDAPI-23017 | 01227628  01255903                     |                | **Issue**: Transaction data spilled to disk as `bigmsg` file is not purged when an error occurs while writing the file. **Resolution**: Incomplete transaction data is now purged by garbage collector when no longer referenced.                                                               |
| RDAPI-23470 | 01201884  01230910  01232914  01259400 |                | **Issue**: SMTP Servers configured with TLS/SSL security in API Gateway failed to do a proper handshake and send emails. **Resolution**: Ensured that SMTP servers configured with TLS/SSL security in API Gateway do a proper handshake based on connection type and send emails successfully. |
| RDAPI-23668 |                                        | CWE-350        | **Issue**: DNS re-branding in NodeJS < 10.24. **Resolution**: The version of NodeJS shipped with API Gateway to facilitate client SDK generator was upgraded to 10.24.0.                                                                                                                        |
| RDAPI-23400 |                                        | CWE-693        | **Issue**: Default security headers missing in API Gateway Manager 302 responses. **Resolution**: Added missing default security headers.                                                                                                                                                       |
| RDAPI-23367 |                                        | CWE-693        | **Issue**: X-XSS-Header should be set to '1; mode=block' by default. **Resolution**: Updated instances of X-XSS-Header to be '1; mode=block'.                                                                                                                                                   |
| RDAPI-21214 |                                        | CWE-319        | **Issue**: Insecure transport when importing API over HTTP through HTTP Proxy. **Resolution**: Added option to import API securely over HTTPS through HTTPS Proxy.                                                                                                                              |
| RDAPI-14825 |                                        | CWE-89         | **Issue**: Query string injection: Amazon Web Services. **Resolution**: Removed unused filter and processor from product.                                                                                                                                                                       |

### Other fixed issues

| Internal ID | Case ID                             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------- | ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-21211 | 01179705                            | **Issue**: API Manager does not support SOAP MTOM definitions. **Resolution**: SOAP definitions that use MTOM are now supported in API Manager, existing SOAP MTOM definitions will need to be re-imported. There is a new jvm flag to maintain the legacy SOAP 1.1 error response codes which can be set in the `apigateway/conf/jvm.xml` file: `<ConfigurationFragment><VMArg name="-Dcom.axway.apimanager.fault.legacy.soap=true"/></ConfigurationFragment>`                                                                                                                                                                                                                                                                                                 |
| RDAPI-21423 | 01180760                            | **Issue**: Quota values getting re-initialized just after clicking Quota text box in Application Quota UI, as they are automatically saved when clicking on any of the fields. **Resolution**: Quota values will only get re-initialized when a change has happened in the quota. Automatically saving the same quota without any changes won't affect the active quota.                                                                                                                                                                                                                                                                                                                                                                                        |
| RDAPI-21680 | 01189795                            | **Issue**: PATCH method cannot be chosen by the API Gateway Manager traffic monitor HTTP method filter. **Resolution**: PATCH has been added to the list of available methods in the filter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| RDAPI-21742 | 01192896                            | **Issue**: In API Manager, Applications are displayed incorrectly after the deletion of one application. **Resolution**: Applications are now displayed correctly.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| RDAPI-22599 | 01216580                            | **Issue**: API Manager UI does not accept legal characters such as `:` or `!` in parameter names. **Resolution**: API Manager will accept all legal characters in parameter names as per specification.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| RDAPI-22857 | 01216014                            | **Issue**: In API Manager, user could not revert the admin username back to `apiadmin` after it had been updated to a different name.This was due to the old username not being deleted. **Resolution**: When a new username is created, the old username is deleted so that it can be created again if needed.                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| RDAPI-23189 | 01251128  01231628                  | **Issue**: In API Gateway, the XML Signature Generation filter throws an error when "Add Inclusive Namespaces for exclusive canonicalization" and "Sign timestamp" are used at the same time. **Resolution**: "Add Inclusive Namespaces for exclusive canonicalization" and "Sign timestamp" can now be used at the same time.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| RDAPI-23220 | 01212326                            | **Issue**: Use of some regular expression patterns in Trace redaction and Raw redaction can generate excessive stack recursion and lead to a process crash. **Resolution**: A `recursionLimit` parameter has been added to both TraceRedactor and RawRedactor configuration elements with a default value of `1000`. That limit is used to configure the regular expression execution engine (PCRE). The "recursionLimit" value is automatically lowered whenever the available thread stack size is estimated to be too small. To prevent any possible leak of data and any interruption of processing flow,  the Trace Redaction mechanism will no longer raise any error and will replace the record being redacted by a description of the redaction error. |
| RDAPI-23304 | 01234002  01231601  01260483        | **Issue**: In API Gateway Manager, setting the trace level to "From System Settings" always set the trace level to DATA instead of the current entity store trace level setting.  **Resolution**: When the "From System Settings" trace level is selected, the entity store trace level will now be used.                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| RDAPI-23363 | 01237213                            | **Issue**: HSM calls start failing systematically when connection to HSM is lost and sessions become invalid. **Resolution**: HSM keys handle are now refreshed when sessions with HSM device are lost and re-established. A blocking transparent retry mechanism has been implemented so that temporary failures are recovered without any policy or connection failure. Retry options can be configured in the `instance/conf/vpkcs11.xml` file. **Issue**: Command line tool `keystoreadmin` fails to load PKCS11 library with error "UnsatisfiedLinkError: Error looking up function 'C_Initialize'". **Resolution**: The `keystoreadmin` tool is now conforming to PKCS11 standard and is only using the "C_GetFunctionList" interface call.               |
| RDAPI-23665 | 01219454  01242097                  | **Issue**: Error reading certificates exception when importing certificates into API Manager and the certificate is imported from a URL ending in `.local` which is not a valid public domain. **Resolution**: The error reading certificates exception no longer occurs when importing certificates into API Manager when the certificate is imported from a URL ending in `.local`, which is not a valid public domain.                                                                                                                                                                                                                                                                                                                                       |
| RDAPI-23696 | 01244600                            | **Issue**: Users can create items with IDs containing URL reserved characters making REST request using command line. However, the UI might become unresponsive when dealing with items containing such IDs. **Resolution**: API Manager UI now deals with ID that contains URL reserved characters.                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-23729 | 01217956  01218858                  | **Issue**: In Policy Studio, when importing an API, API methods which were not selected were still imported. **Resolution**: The import has been updated to correctly remove API methods that are deselected.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| RDAPI-23867 | 01259494                            | **Issue**: On [March 2021](/docs/apim_relnotes/20210330_apimgr_relnotes/) release of Policy Studio, upgrading a .fed file containing Node Manager or Analytics fails with an error message, "Unable to find template for product". This happens because the location of the product templates for Node Manager and Analytics was incorrect. **Resolution**: The location of the products templates for NodeManager and Analytics is fixed now.                                                                                                                                                                                                                                                                                                                  |
| RDAPI-23914 | 01252778                            | **Issue**: In API Manager, legacy APIs failed processing because of enum parameters validation. **Resolution**: You can set the new 'com.axway.api.runtime.broker.parameters.skipEnumValidation' Java system property to `true` to skip enum parameters validation during processing of user requests. Defaults to `false`.                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| RDAPI-23968 | 01252558                            | **Issue**: Vulnerability found in snakeyaml-1.25.jar deployed with API Gateway. **Resolution**: The API Gateway snakeyaml library has been updated to version 1.28. This version addresses known vulnerabilities.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| RDAPI-24025 | 01255678                            | **Issue**: API Manager was incorrectly merging inbound transaction headers and API method outbound header parameters of the same names. **Resolution**: API Manager applies API method outbound header parameters instead of the inbound transaction headers of the same names.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| RDAPI-24033 | 01254195  01254335  01255287        | **Issue**: API Manager runtime only accepts empty values for query string parameters. **Resolution**: API Manager accepts empty values for any query parameter data type based on Swagger allowEmptyValue property, if this property is present. If the Swagger property is not present, the 'com.vordel.coreapireg.runtime.broker.parameters.allowEmptyDefault' Java system property is used instead. Defaults to `false`.                                                                                                                                                                                                                                                                                                                                     |
| RDAPI-24041 | 01250204                            | **Issue**: The Cache Attribute filter sometimes reports that a value is not cached in a distributed cache if the same key is used repeatedly. **Resolution**: The Cache Attribute filter reports caching result correctly in a distributed cache if the same key is used repeatedly.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-24049 | 01255678                            | **Issue**: API Manager outbound request does not forward all original header parameters of the same name. **Resolution**:  Problem was caused by en error in API Gateway handling of multi-value headers. Only first value was returned when queried. Issue has been resolved and now API Gateway properly returns a collection of values when a multi-value header is queried.                                                                                                                                                                                                                                                                                                                                                                                 |
| RDAPI-24051 | 01252436  01252568                  | **Issue**: A product crash occurs when connecting to HTTPS server through an HTTPS proxy. **Resolution**: Fixed crash attempting a TLS handshake on the outgoing connection via HTTPS proxy.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| RDAPI-24134 | 01256975  01254465  01262164        | **Issue**: Circular dependency warning dialog is displayed incorrectly for policy circuits that do not contain circular dependency. The error log message for circular dependencies does not contain information for the full chain of circuits in which circular dependency occurs. **Resolution**: The error log message for circular dependencies now contains information for the full chain of circuits in which circular dependency occurs, and dialog now displays correctly. The System property `suppressCircularDependencyValidationWarnings` has been added to allow circular dependency warning dialog to be suppressed when the property is set to `true`.                                                                                         |
| RDAPI-24152 | 01257571                            | **Issue**: When editing an API in Policy Studio, the associated filter does not have a `clearValueWhenDisabled` property set to `false` by default.  **Resolution**: The `clearValueWhenDisabled` property has been set to `false` by default to prevent the `uriprefix` field from being cleared on deselection of the "Enable this path resolver" checkbox.                                                                                                                                                                                                                                                                                                                                                                                                   |
| RDAPI-24222 | 01258765 01262558 01264526 01259035 | **Issue**: When creating a Client Access Token Store in Policy Studio it is not possible to save the Store unless the Persistence Type is set to Database. **Resolution**: When creating a Client Access Token Store in Policy Studio it is now possible to save the Store for any Persistence Type.                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-24223 | 01257725  01257310                  | **Issue**: API Manager filter OAuth Authorizations by owner not working. **Resolution**: OAuth Authorizations can be filtered by application name, scope, owner, or creation date (same back-end and front-end locale).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| RDAPI-24266 | 01258234                            | **Issue**: Service Handler filter cannot be edited to change routing settings from 'Direct Connection to Service Endpoint' to 'Delegate to Routing Policy'. **Resolution**: Service Handler filter can be edited.                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|RDAPI-24271 | 01257804                            | **Issue**: In Policy Studio, a configuration error message is shown when you press the OK button in the OAuth Edit Access Token Store dialog. **Resolution**: The issue is fixed and the OK button works as expected.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| RDAPI-24318 | 01179461                            | **Issue**: Unexpected correlation ID on failure using OAuth (External) device. **Resolution**: The correlation ID is preserved on `get token Information Policy` failure from OAuth (External) device.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| RDAPI-24337 | 01261191                            | **Issue**: The "Access Token using Client Credentials" filter could not be saved when the "Do not generate a refresh token" option is selected. **Resolution**: The "Access Token using Client Credentials" filter can now be saved when the "Do not generate a refresh token" option is selected.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| RDAPI-21693 |                                     | **Issue**: The Oracle OJDBC6 JDBC driver used by API Gateway and Policy Studio is out of date. **Resolution**: The Oracle JDBC driver used by API Gateway and Policy Studio is upgraded to OJDBC8.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

## Known issues

The following are known issues for this update.

| Internal ID | Description                                                                                                                                                        |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| RDAPI-16486 | Changes in the mapper always require a reload in the Execute Data Maps filter and once reloaded then providing values for the required parameters must be repeated |
| RDAPI-17282 | Connector for Salesforce APIs in API Manager doesn't work or is impossible to configure                                                                            |
| RDAPI-17395 | APIGW Analytics - no data in DB during DB unavailability                                                                                                           |
| RDAPI-18332 | "Try-it" for API-Method is not working                                                                                                                             |
| RDAPI-18523 | Inconsistent application search behaviour relating to application sharing                                                                                          |
| RDAPI-18601 | EMT environmentalisation issue                                                                                                                                     |
| RDAPI-18986 | projpack is unable to merge projects after projupgrade; likely due to Default APIManager policies                                                                  |
| RDAPI-18990 | "Failed to delete undefined" window pops up unexpectedly when attempting to delete application                                                                     |
| RDAPI-19217 | Inconsistency between Application Developers and Account Settings pages in Manager                                                                                 |
| RDAPI-19292 | When an APIM admin user's login name is changed, the user is directed to a blank page                                                                              |
| RDAPI-19293 | API Catalog Try It shows only the first security device of a security profile                                                                                      |
| RDAPI-19334 | Access to retired APIs is not removed from other organizations as expected                                                                                         |
| RDAPI-19436 | API approve/enable functionality for an organization does not show on application view                                                                             |
| RDAPI-19442 | Saving Mode Stuck for the Application Creation in different session                                                                                                |
| RDAPI-19601 | Sharing section of Application issue when Organization of application changed                                                                                      |
| RDAPI-19742 | API metrics ignore an API's organization                                                                                                                           |
| RDAPI-19743 | API Broker not shown in circuit path on all failures                                                                                                               |
| RDAPI-20527 | xml2json filter, unable to use xml with valid namespace syntax                                                                                                     |
| RDAPI-20593 | Issue with Authentication profiles related to permethod override                                                                                                   |
| RDAPI-20594 | When a token is revoked with an incorrect Authorization header, the response is 400 instead of 401                                                                 |
| RDAPI-20726 | How to get attribute value for apimgmt.application.id                                                                                                              |
| RDAPI-20742 | Inconsistent deletion of Environmentalized Settings                                                                                                                |
| RDAPI-20952 | NullPointerException when opening a project with dependencies                                                                                                      |
| RDAPI-21009 | Issue after updating API Manager settings through rest api if lockUserAccount is missing                                                                           |
| RDAPI-21061 | updated error message, OAS3 file import without servers section url                                                                                                |
| RDAPI-21171 | Minor UI issue affecting pagination in API keys and Oauth client credentials                                                                                       |
| RDAPI-21275 | Application default quota in days produces "Cannot instantiate API constraint" error                                                                               |
| RDAPI-21295 | XSD files are not downloaded when Use client registry is enabled                                                                                                   |
| RDAPI-21325 | Improve load time performance of the Applications screen                                                                                                           |
| RDAPI-21332 | Cassandra 2.2.12 Vulnerabilities from latest scan                                                                                                                  |
| RDAPI-21384 | Webservice (WSDL-based) responds with 500 instead of 405 when an invalid HTTP method is used                                                                       |
| RDAPI-21411 | Inconsistent treatment of multiple scopes in Oauth request                                                                                                         |
| RDAPI-21438 | Search box for Applications will not accept certain Unicode characters                                                                                             |
| RDAPI-21456 | Application export doesn't include external credential                                                                                                             |
| RDAPI-21514 | Policy shortcut chain filter corrupts priority order when altering the sequence                                                                                    |
| RDAPI-21653 | Custom Property Maximum Size                                                                                                                                       |
| RDAPI-21675 | API Gateway Manager > Messaging > {Queue}: display issue long name queue                                                                                           |
| RDAPI-21770 | Incorrect semantics of negated match types like IS_NOT in Compare Attribute filter                                                                                 |
| RDAPI-21875 | User creation failing under small load                                                                                                                             |
| RDAPI-22073 | \[CWE-502] Deserialization of Untrusted Data via Guava - Cassandra and Configurationstudio                                                                         |
| RDAPI-22147 | API Manager GUI - API sorting issue                                                                                                                                |
| RDAPI-22164 | Policy Studio UX issue, shows original, non environmentalized URL for DB                                                                                           |
| RDAPI-22197 | Report display issue                                                                                                                                               |
| RDAPI-22204 | Wrong documentation on API Manager Swagger/OAS for corsOrigins                                                                                                     |
| RDAPI-22221 | libxml Error while importing WSDL                                                                                                                                  |
| RDAPI-22331 | automated submission form protection for forgottenpassword                                                                                                         |
| RDAPI-22333 | OAS 3 import implicitly requires a "servers" object, which should not be mandatory                                                                                 |
| RDAPI-22430 | Issues removing custom policies from API Manager                                                                                                                   |
| RDAPI-22452 | On APIMgr monitoring, application id is displayed instead of application name                                                                                      |
| RDAPI-22455 | Self crosssite scripting vulnerability in API Manager                                                                                                              |
| RDAPI-22513 | Package properties not visible, PS and CS tools on Linux                                                                                                           |
| RDAPI-22671 | XPath wizard - Unexpected behavior of 'Evaluate' button                                                                                                            |
| RDAPI-22756 | No longer able to search applications by ID on API Manager 7.7                                                                                                     |
| RDAPI-22760 | setting for EMT offload of audit.log files                                                                                                                         |
| RDAPI-22764 | Slowness with APIGateway Policy Studio, Windows ver 1803 and 1809                                                                                                  |
| RDAPI-22848 | Attempting to change the API default quota produces "cannot modify default quota properties" error                                                                 |
| RDAPI-22954 | swagger imports fine, but comes out with error from catalog 2.0 link                                                                                               |
| RDAPI-22987 | APIM login endpoint stops responding when pod has been running for 30 days                                                                                         |
| RDAPI-23222 | Unable to create user as error pop up is displayed                                                                                                                 |
| RDAPI-23326 | Cassandra CVE-2020-13946                                                                                                                                           |
| RDAPI-23379 | API Catalog shows http and https base urls, customer wants only https                                                                                              |
| RDAPI-23471 | if custom API Proxy broker, customized backend service url is not kept in serviceprofiles in .dat export                                                           |
| RDAPI-23499 | Using OAuth External Attributes to send serialized objects                                                                                                         |
| RDAPI-23500 | Trial option does not work (not fixed by RDAPI-19580)                                                                                                              |
| RDAPI-23549 | No VAPI matched request after upgrade from 7.5.3 SP12 using inbound security policy                                                                                |
| RDAPI-23557 | TraceRedactor error:java.lang.RuntimeException: regex error with code: -10                                                                                         |
| RDAPI-23571 | OAuth access tokens can be refreshed even after expiration when Cassandra TTL is NULL                                                                              |
| RDAPI-23601 | add header in inbound security for frontend doesn't appear anymore in params.header                                                                                |
| RDAPI-23654 | Trace record logs passwords in plain text at DATA level                                                                                                            |
| RDAPI-23655 | When an APIM administrator changes a user's password, the user's session should be ended                                                                           |
| RDAPI-23658 | HTTP transaction blocked by Send To JMS filter when deploying configuration                                                                                        |
| RDAPI-23723 | different crash after ModSec patch                                                                                                                                 |
| RDAPI-23779 | The http.headers attribute is vulnerable to CRLF injection                                                                                                         |
| RDAPI-23786 | nov20 PS loads a *.fed (1Mb) in +5min in win10                                                                                                                     |
| RDAPI-23820 | Memory leak in customer environment                                                                                                                                |
| RDAPI-23829 | API gateway and Portal duplicates the base url in the API catalog.                                                                                                 |
| RDAPI-23841 | Deleting an Org in API Manager always throws error/exception in trace                                                                                              |
| RDAPI-23853 | Slow API Manager GUI due to large authorization table                                                                                                              |
| RDAPI-23866 | Try-It example for SOAP request missing namespaces                                                                                                                 |
| RDAPI-23913 | Analytics PDF reports missing line item results that show on UI                                                                                                    |
| RDAPI-23946 | Cassandra 2.2.x EOL 30th April 2021                                                                                                                                |
| RDAPI-23963 | POST /applications/<app_id>/apis is much slower in Mar-21 than 7.5.3                                                                                               |
| RDAPI-23965 | release connections from pool for OracleDB-API_GW connectivity                                                                                                     |
| RDAPI-23981 | SAXParseException when attempting to process an inbound WebService request                                                                                         |
| RDAPI-23984 | "Set time" control in API Monitoring does not work fully in a Chinese locale                                                                                       |
| RDAPI-24011 | PS  doesn't open Dependencies Projects using recent projects links                                                                                                 |
| RDAPI-24024 | CVE-2021-2161 / CVE-2021-2163                                                                                                                                      |
| RDAPI-24145 | CPU spikes to 100% with unresponsive host                                                                                                                          |
| RDAPI-24148 | com.vordel.coreapireg.runtime.broker.InvokableMethodParamException: Required parameter 'null' missing                                                              |
| RDAPI-24222 | \[PS] Client Access Token Store creation: "You must enter a value for 'Purge up to'"                                                                               |
| RDAPI-24226 | pop messages are retrieved by both instances in the same group                                                                                                     |
| RDAPI-24251 | Issue in API update (API livecycle)                                                                                                                                |
| RDAPI-24256 | Policy responding to OPTIONS call returns 200 instead of 403 after upgrade from 7.5.3                                                                              |
| RDAPI-24324 | Memory leak in production                                                                                                                                          |
| RDAPI-24329 | core dump with websocket                                                                                                                                           |
| RDAPI-24345 | API Manager is matching VAPI to deleted APIs to incoming requests instead of the Published API                                                                     |
| RDAPI-24359 | OCSP filter issue with intermediate certs having same cert DN                                                                                                      |
| RDAPI-24360 | XML Complexity filter with charset of cp1252 fails                                                                                                                 |
| RDAPI-24371 | policy that check the revoked certificates throws an exception after an upgrade to 77                                                                              |
| RDAPI-24383 | Try-it not working with http port in API Manager                                                                                                                   |
| RDAPI-24400 | APIM sends Hosts header with default port even when unnecessary                                                                                                    |

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
