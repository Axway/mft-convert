---
title: API Gateway and API Manager 7.7 November 2020 Release Notes
linkTitle: API Gateway and API Manager November 2020
weight: 110
date: 2020-11-05
---
## Summary

API Gateway is available as a software installation or a virtualized deployment in Docker containers. API Manager is a licensed product running on top of API Gateway, and has the same deployment options as API Gateway.

The software installation is available on Linux. For more details on supported platforms for software installation, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

Docker deployment is supported on Linux. For a summary of the system requirements for a Docker deployment, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

## New features and enhancements

The following new features and enhancements are available in this update.

### Run a script to back up and restore Cassandra

An automated script to back up and restore Cassandra clusters is now included in API Gateway. For more information, see [Apache Cassandra backup and restore](/docs/cass_admin/cassandra_bur).

### Enhanced support for OAS3 parameters

Support for the OpenAPI specification 3 (OAS3) parameter attributes is now enriched. The `allowEmptyValue` and `explode` fields, as well as the free-form parameter definition, are all supported in this version of API Manager.

### Enhanced API life cycle management for Organization administrators

We have [previously](/docs/apim_relnotes/20200930_apimgr_relnotes/#organization-administrators-can-publish-apis) released a system property, `api.manager.orgadmin.selfservice.enabled`, to include additional permissions for an Organization administrator to publish and unpublish APIs that were created in their organization without approval from an API Administrator.

In this update, additional API life cycle events (deprecate, undeprecate, upgrade, and grant access to APIs) will also be enabled for an Organization administrator when `api.manager.orgadmin.selfservice.enabled` is set to True.

For more information, see [System property changes](/docs/apim_reference/system_props/).

### YAML configuration store (Technical preview capability)

This update includes bug fixes and enhanced functionality for YAML configuration as follows:

* Support for conversion of [XML configuration fragments to YAML configuration fragments](/docs/apim_yamles/apim_yamles_cli/yamles_cli_convert#convert-your-xml-configuration-fragment-to-a-yaml-configuration-fragment).
* Support for [import and export](/docs/apim_yamles/apim_yamles_cli/yamles_cli_importexport) of YAML configuration fragments.
* Allow user-controlled file names for YAML entities.
* Improved layout in YAML entity files.
* Enhanced support for managing more configuration content in [externalized files](/docs/apim_yamles/yamles_externalized_files).

See the [September 2020](/docs/apim_relnotes/20200930_apimgr_relnotes/#yaml-configuration-store-technical-preview-capability) release notes for an overview of this technical preview, and the [YAML configuration](/docs/apim_yamles/) documentation for more detailed information.

## Important changes

There are no major changes in this update.

<!-- It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.. -->

## Deprecated features

As part of our software development life cycle we constantly review our API Management offering. In this update, the following capabilities have been deprecated:

* API Gateway and API Manager versions 7.5.x and 7.6.x reached end of support (EOS) in November 2020.

## Removed features

No capabilities have been removed in this update.

<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, the following features have been removed: -->

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [Release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID                                          | Cve Identifier | Description                                                                                                                                                                                                                                                                                                                              |
| ----------- | ------------------------------------------------ | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-18674 | 01123229                                         |                | **Issue**: A malicious user can modify data in an unencrypted export file and cause invalid applications to be stored in the database. Invalid applications cannot be deleted or visualized in the API Manager UI. **Resolution**: The Application ID field is now validated. Invalid applications are no longer stored in the database. |
| RDAPI-21336 | 01183015                                         |                | **Issue**: API Manager UI sends a CSRF token as a URL parameter. It should be submitted as a HTTP Header to prevent it from being accessible to collect via upstream proxy/browser logs. **Resolution**: API Manager UI always sends CSRF tokens as HTTP headers.                                                                        |
| RDAPI-22172 | 01205254, 01205882, 01205986, 01208891, 01196812 |                | **Issue**: In API Gateway, when using a Threat Protection profile, requests that are timing out within the gateway lead to a crash of the application due to incorrect memory handling. **Resolution**: Memory is now handled correctly when there is a request timeout with a Threat Protection profile active.                         |

### Other fixed issues

| Internal ID | Case ID                                          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------- | ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-17431 | 01090329  01092098                               | **Issue**: API Gateway Policy Studio and projdeploy script attempt to re-deploy configuration on error response from Admin Node Manager. **Resolution**: Policy Studio and projdeploy script attempt to deploy configuration only once as expected.                                                                                                                                                                                                                                                                                                             |
| RDAPI-19132 | 01134834                                         | **Issue**: In API Manager, updating the Retirement date field is not being enforced for API Deprecation when "Retire API at specific date" is selected. **Resolution**: If "Retire API at specific date" is selected, the Retirement date field is a required value.                                                                                                                                                                                                                                                                                            |
| RDAPI-19240 | 01138931                                         | **Issue**: API Manager Applications can be shared with Users in 'Pending Approval' state. **Resolution**: API Manager Applications can now only be shared with Users in 'Approved' state.                                                                                                                                                                                                                                                                                                                                                                       |
| RDAPI-19278 | 01138179                                         | **Issue**: Migrating an API with its client apps from one org to another, the API access of the migrated app to the new API is removed when the API access is removed from the old org. **Resolution**: API Access is not deleted from the migrated applications.                                                                                                                                                                                                                                                                                               |
| RDAPI-19453 | 01153936  01142389                               | **Issue**: The new Application form in API Manager UI enters an error state when an invalid image file type is uploaded. **Resolution**: The new Application form now recovers from this state after a valid image type is uploaded.                                                                                                                                                                                                                                                                                                                            |
| RDAPI-19958 | 01190536  01206332  01184602  01192821  01155634 | **Issue**: SMTP Servers configured with TLS security in API Manager fail to send emails. **Resolution**: SMTP Servers configured with TLS security now successfully send emails.                                                                                                                                                                                                                                                                                                                                                                                |
| RDAPI-19971 | 01146075  01149226                               | **Issue**: Commands "nodemanager" and "startinstance" used with option "-s" to check process status are not showing consistent result when executed by different system user. **Resolution**: Those commands are no longer trying to open the files containing the PID with write access. A correct status is now returned when used by a different user.                                                                                                                                                                                                       |
| RDAPI-20740 | 01168262  01168258                               | **Issue**: In the Policy Studio Settings, the Transaction Access Log 'Format' field could not be environmentalized. **Resolution**: The 'Format' field has been updated so that it can be successfully environmentalized.                                                                                                                                                                                                                                                                                                                                       |
| RDAPI-20817 | 01170172                                         | **Issue**: In API Manager, generated Swagger schema files are not valid Swagger definitions. **Resolution**: Errors have been removed and schema files now respects Swagger specification.                                                                                                                                                                                                                                                                                                                                                                      |
| RDAPI-20829 | 01168561                                         | **Issue**: In API Manager, all API Access could not be revoked from organizations through the api-promote script. **Resolution**: A new property (organization.apis.remove.all) has been added to the script to allow users to remove all API Access from organizations.                                                                                                                                                                                                                                                                                        |
| RDAPI-20919 | 01171136                                         | **Issue**: It is hard to identify from traces the timeout setting involved in connection closure. **Resolution**: Timeout exceptions raised by connections now include the timeout type and the corresponding value that triggered the timeout.                                                                                                                                                                                                                                                                                                                 |
| RDAPI-21030 | 01164214                                         | **Issue**: API Manager ignores the "dont.expect.100.continue" Java system property if the Outbound Security is HTTP Basic. **Resolution**: The ConnectToURL filter supports the "dont.expect.100.continue" Java system property when also used with Kerberos or HTTP Basic Authentication profiles. Note that the ConnectToURL filter with Kerberos Authentication profile has been also fixed to respect HTTP protocol of server when sending the Expect 100-Continue header in request.                                                                       |
| RDAPI-21189 | 01153025                                         | **Issue**:  AWS SQS message poller stops working when a disconnection occurs or a message processing results in an error. **Resolution**:  The AWS SQS poller service has been improved to handle connection and processing errors, resulting in polling retries performed as configured.                                                                                                                                                                                                                                                                       |
| RDAPI-21263 | 01180738                                         | **Issue**: API Manager "Import from topology" selects non-SSL ports of a Policy Studio created API. **Resolution**: "Import from topology" now allows the user to select the desired port from a drop-down menu of backend URLs in API Manager UI. It defaults to the SSL port.                                                                                                                                                                                                                                                                                 |
| RDAPI-21436 | 01184617  01185739  01206009  01185976  01184227 | **Issue**: Long startup and deployment time are observed when many APIs are virtualized. **Resolution**: You must add the new "-Dcom.axway.apimanager.configure.apis.nonblocking.enabled=true" property to your JVM configuration file (jvm.xml). The property will start loading APIs after completion of the boot and deployment sequence.                                                                                                                                                                                                                    |
| RDAPI-21469 | 01187236                                         | **Issue**: kpsadmin script was returnin inconsistent status codes, for example, 0 (success) instead of 1 (error). **Resolution**: The kpsadmin script exception handling has been updated to address exceptions at the scripts end, allowing for a correct exit code to be determined.                                                                                                                                                                                                                                                                          |
| RDAPI-21479 | 01186081                                         | **Issue**: In API Gateway, when an XML body contains special characters, the filter "Check XML Complexity" fails with an error when the XML Prolog is not present, even if an encoding is set in the "Content-Type" header. **Resolution**: The filter "Check XML Complexity" now accepts bodies without XML Prolog by using the encoding passed in the "Content-Type" header, if present (otherwise defaulting to UTF-8)                                                                                                                                       |
| RDAPI-21506 | 01188439                                         | **Issue**: API Manager cannot import Swagger with external docs defined in method. A NullPointerException is thrown. **Resolution**: Error that caused the exception thrown has been fixed. API Manager can now import Swagger files with external docs defined.                                                                                                                                                                                                                                                                                                |
| RDAPI-21557 | 01174059  01173295                               | **Issue**: API Gateway might relay a "100 continue" response from backend when HTTP response has not been fully received (for example, timeout, disconnection). **Resolution**: Connection filter has been corrected to no longer forward the intermediary 100 response received from backend.                                                                                                                                                                                                                                                                  |
| RDAPI-21679 | 01187897                                         | **Issue**: Amazon SQS Queue Poller dialog does not show the unit type used by the visibility timeout parameter. **Resolution**: The unit type of the visibility timeout (seconds) is now displayed in the configuration dialog.                                                                                                                                                                                                                                                                                                                                 |
| RDAPI-21784 | 01185642  01192682                               | **Issue**: HTTP redaction of a multipart form-data field is not performed when the corresponding Content-Disposition header contains more than one parameter. **Resolution**: HTTP multipart/form-data redaction has been corrected to handle extra header parameters and trigger field redaction.                                                                                                                                                                                                                                                              |
| RDAPI-21801 | 01179687  01181723                               | **Issue**: Query parameters are being encoded as they pass from frontend request to backend request. **Resolution**: A java system property com.coreapireg.apimethod.querystring.passthrough has been added that allows API Manager to bypass query string encoding to send the original query parameters to the backend API, while removing any undefined query parameters. To set this property, add the following line to the JVMSettings section of the system/conf/jvm.xml file: `<VMArg name="-Dcom.coreapireg.apimethod.querystring.passthrough=true"/>` |
| RDAPI-21813 | 01196493  01195666                               | **Issue**:  The presence of duplicate certificates (certificates with the same thumbprints) in Policy Studio causes improper handling of certificate entities in memory and may lead to errors in API Gateway. The logged error messages don't specify the root of the problem. **Resolution**: Warning messages are logged in the trace when duplicate certificates are present in Policy Studio. Messages are logged on deploy from Policy Studio and in Certificates screen in Policy Studio when certificates are loaded and manipulated.                   |
| RDAPI-21815 | 01195371                                         | **Issue**: The API Manager API Proxy Registration API method upgrading an existing API to a newer API may timeout with large numbers of API Manager applications and organizations under stress load. **Resolution**: The API Proxy Registration API method upgrading an existing API to a newer API supports large numbers of API Manager applications and organizations.                                                                                                                                                                                      |
| RDAPI-21970 | 01200328                                         | **Issue**: In API Manager an exception is thrown when checking a request for optional body parameter when there is no request body. **Resolution**: The check is now only performed if the body parameter is required.                                                                                                                                                                                                                                                                                                                                          |
| RDAPI-21984 | 01200641                                         | **Issue**:  Groovy 2.5 engine throws "No signature of method: java.util.Date.format() is applicable for argument types: (String) values: \[dd-MMM-yyyy]" when formatting dates. **Resolution**: groovy-dateutil jar has been added to API Gateway distribution.                                                                                                                                                                                                                                                                                                 |
| RDAPI-22019 | 01204861  01201541                               | **Issue**: URI Template Encoding ignores full stop "." in a path segment when path parameters have been extracted. **Resolution**: URI Template Encoding now considers full stop in the path segment.                                                                                                                                                                                                                                                                                                                                                           |
| RDAPI-22061 | 01201966  01198537                               | **Issue**: If a backend API defines a header parameter named 'Content-Type', such parameter is duplicated in the request as there is already an entity header with the same name. **Resolution**: 'Content-Type' header is no longer duplicated in the request. In case of conflict value of entity header takes precedence.                                                                                                                                                                                                                                    |
| RDAPI-22088 | 01201232  01193442  01214078                     | **Issue**: SQL for API Manager Metrics uses unsupported double quotation marks (") in Oracle DB query. **Resolution**: SQL no longer uses unsupported characters in Oracle DB query.                                                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-22159 | 01199853  01200341                               | **Issue**: The update_apigw.sh script is failing with "NotImplementedError: passwd.pw_passwd unimplemented" message. **Resolution**: The update_apigw.sh script is updated to prevent this error.                                                                                                                                                                                                                                                                                                                                                               |
| RDAPI-22410 | 01211646  01211082  01211925                     | **Issue**: API Manager import was removing parts of the Method Path which matched the API Resource Path. **Resolution**: API Manager no longer removes parts of Method Path incorrectly importing.                                                                                                                                                                                                                                                                                                                                                              |

## Known issues

The following are known issues for this update.

| Internal ID | Description                                                                                                                                                        |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| RDAPI-15981 | Scopes fields for API Key remain visible even if Application Scopes are disabled                                                                                   |
| RDAPI-16486 | Changes in the mapper always require a reload in the Execute Data Maps filter and once reloaded then providing values for the required parameters must be repeated |
| RDAPI-17282 | Connector for Salesforce APIs in API Manager doesn't work or is impossible to configure                                                                            |
| RDAPI-18198 | CORS preflight fails for WSDL based API Manager APIs, thus Try-It fails                                                                                            |
| RDAPI-18332 | "Try-it" for API-Method is not working                                                                                                                             |
| RDAPI-18523 | Inconsistent application search behaviour relating to application sharing                                                                                          |
| RDAPI-18986 | projpack is unable to merge projects after projupgrade; likely due to Default APIManager policies                                                                  |
| RDAPI-18990 | "Failed to delete undefined" window pops up unexpectedly when attempting to delete application                                                                     |
| RDAPI-19006 | Delete API "not found" after changing Application Org                                                                                                              |
| RDAPI-19292 | When an APIM admin user's login name is changed, the user is directed to a blank page                                                                              |
| RDAPI-19293 | API Catalog Try It shows only the first security device of a security profile                                                                                      |
| RDAPI-19333 | Grant API access removed from API Access list in application when FE unpublished                                                                                   |
| RDAPI-19354 | Issue with backend API import regarding Duplicate parameters value                                                                                                 |
| RDAPI-19442 | Saving Mode Stuck for the Application Creation in different session                                                                                                |
| RDAPI-19580 | Trial option in the Organization does not work                                                                                                                     |
| RDAPI-19586 | Retired API appears as "published" in Catalog                                                                                                                      |
| RDAPI-19601 | Sharing section of Application issue when Organization of application changed                                                                                      |
| RDAPI-19788 | Issue with pagination across multiple sections                                                                                                                     |
| RDAPI-20255 | Application Quota not promoted by apimanger promote                                                                                                                |
| RDAPI-20480 | swagger enum query parameters not validated                                                                                                                        |
| RDAPI-20550 | renaming existing KPS collection in PS breaks ES store                                                                                                             |
| RDAPI-20726 | How to get attribute value for apimgmt.application.id                                                                                                              |
| RDAPI-20742 | Inconsistent deletion of Environmentalized Settings                                                                                                                |
| RDAPI-20793 | Resource Path is duplicated in API Manager for Swagger 1.2                                                                                                         |
| RDAPI-20928 | Parameter types differ in Backend API and In TryIt                                                                                                                 |
| RDAPI-20952 | NullPointerException when opening a project with dependencies                                                                                                      |
| RDAPI-21009 | Issue after updating API Manager settings through rest api if lockUserAccount is missing                                                                           |
| RDAPI-21061 | Updated error message, OAS3 file import without servers section url                                                                                                |
| RDAPI-21113 | "URLDecoder: Incomplete trailing escape (%) pattern" only when virtualized API uses Outbound authentication                                                        |
| RDAPI-21171 | Minor UI issue affecting pagination in API keys and Oauth client credentials                                                                                       |
| RDAPI-21295 | XSD files are not downloaded when Use client registry is enabled                                                                                                   |
| RDAPI-21296 | APIM error contacting the server after logon                                                                                                                       |
| RDAPI-21384 | Webservice (WSDL-based) responds with 500 instead of 405 when an invalid HTTP method is used                                                                       |
| RDAPI-21411 | Inconsistent treatment of multiple scopes in Oauth request                                                                                                         |
| RDAPI-21423 | Quota values getting re-initialized just after clicking quota text box                                                                                             |
| RDAPI-21434 | "Expect to Retrieve X rows" setting on database query filter                                                                                                       |
| RDAPI-21444 | Policy Studio can't delete or edit scripts                                                                                                                         |
| RDAPI-21456 | Application export doesn't include external credential                                                                                                             |
| RDAPI-21653 | Custom Property Maximum Size                                                                                                                                       |
| RDAPI-21689 | \[ELI] HTTP 500 when accessing user profile                                                                                                                        |
| RDAPI-21697 | problems with recent change to multipart form-data RDAPI-20461                                                                                                     |
| RDAPI-21770 | Incorrect semantics of negated match types like IS_NOT in Compare Attribute filter                                                                                 |
| RDAPI-21875 | User creation failing under small load                                                                                                                             |
| RDAPI-21916 | WAF can't access JSON payload using OWASP_CRS 3.2.0                                                                                                                |
| RDAPI-22087 | \[API Manager] Duplicate header if header is overwritten in inbound security policy                                                                                |
| RDAPI-22197 | Report display issue                                                                                                                                               |
| RDAPI-22204 | Wrong documentation on API Manager Swagger/OAS for corsOrigins                                                                                                     |
| RDAPI-22218 | Xerces SAXParserImpl logs warnings to console when instance is started                                                                                             |
| RDAPI-22221 | libxml Error while importing WSDL                                                                                                                                  |
| RDAPI-22247 | CWE-548 - Static Content Providers default to "Allow directory listings"                                                                                           |
| RDAPI-22258 | Wrong result value retrieve in postCircuitProcessing() for FilterInterceptor                                                                                       |
| RDAPI-22424 | Issue with SameSite cookies                                                                                                                                        |
| RDAPI-22430 | Issues removing custom policies from API Manager                                                                                                                   |
| RDAPI-22442 | Error when saving ENV file in Configuration Studio-Linux                                                                                                           |
| RDAPI-22452 | On APIMgr monitoring, application id is displayed instead of application name                                                                                      |
| RDAPI-22459 | Accept header in per-method override is not replacing header                                                                                                       |
| RDAPI-22513 | Package properties not visible, PS and CS tools on Linux                                                                                                           |

## Update a classic (non-container) deployment

To **update** your API Gateway, see [Update from API Gateway One Version](/docs/apim_installation/apigw_upgrade/upgrade_steps_oneversion/).

To **upgrade** from an older version, see [Upgrade from API Gateway 7.5.x or 7.6.x](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/).

## Update a container deployment

Any custom `fed` files deployed to a container must be upgraded using [upgradeconfig](/docs/apim_installation/apigw_upgrade/upgrade_analytics#upgradeconfig-options) or [projupgrade](/docs/apim_reference/devopstools_ref#projupgrade-command-options). They must be upgraded the same way, regardless of whether they are API Manager enabled or not.

The `fed` now contains the updates for the API Manager configuration and can be used to build containers.

## Documentation

This section describes documentation enhancements and related documentation.

### Policy Studio help documentation

This update includes changes to the Policy Studio documentation, but the version of the documentation (Eclipse Help) shipped with Policy Studio is out of sync with the online documentation and does not include these changes.

We are working to rectify this issue in a future update. In the meantime, you can find the up to date documentation online at [Develop in Policy Studio](/docs/apim_policydev/).

### API Management open source documentation

The latest version of API Gateway, API Manager, and API Portal documentation has been migrated to Markdown format and is available in a [public GitHub repository](https://github.com/Axway/axway-open-docs) to prepare for future collaboration using an open source model. As part of this migration, the documentation has been restructured to help users navigate the content and find the information they are looking for more easily.

Documentation change history is now stored in GitHub. To see details of changes on any page, click the link in the **Last modified** section at the bottom of the page.

### Related documentation

To find all available documentation for this product version:

1. Go to [Manuals on the Axway Documentation portal](https://docs.axway.com/bundle).
2. In the left pane Filters list, select your product or product version.

Customers with active support contracts need to log in to access restricted content.

The following reference documents are also available:

* [Supported Platforms](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en) - Lists the different operating systems, databases, browsers, and thick client platforms supported by each Axway product.
* [Interoperability Matrix](https://docs.axway.com/bundle/Axway_Products_InteroperabilityMatrix_allOS_en) - Provides product version and interoperability information for Axway products.

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email [support@axway.com](mailto:support@axway.com) or visit [Axway Support](https://support.axway.com/).

See [Get help with API Gateway](/docs/apim_administration/apigtw_admin/trblshoot_get_help/) for the information that you should be prepared to provide when you contact Axway Support.
