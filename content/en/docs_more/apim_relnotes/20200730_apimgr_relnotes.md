---
title: API Gateway and API Manager 7.7 July 2020 Release Notes
linkTitle: API Gateway and API Manager July 2020
weight: 130
date: 2020-06-09T00:00:00.000Z
---
## Summary

API Gateway is available as a software installation or a virtualized deployment in Docker containers. API Manager is a licensed product running on top of API Gateway, and has the same deployment options as API Gateway.

The software installation is available on Linux. For more details on supported platforms for software installation, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

Docker deployment is supported on Linux. For a summary of the system requirements for a Docker deployment, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

## New features and enhancements

### ICAP filter improvements

We have made further improvements for Internet Content Adaption Protocol (ICAP) support as embedded AV filters are deprecated. For more information, see [Deprecated features](#deprecated-features).

### Improved upgrade instructions

We have improved documentation with more detailed information to upgrade from previous versions to the latest version of the product.

* Added [checklists for upgrading single-node environment](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/#upgrade-steps---single-node-upgrade-example).
* Created a [list of API Gateway scripts that you must run after upgrade your product](/docs/apim_reference/scripts_changelog_sp/), which provides visibility of the changes from release to release.

### MSSQL Database Support

We have tested and validated `MSSQL 2016/7/9` databases for use with API Gateway and API Analytics.

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.

<!-- Use this section to describe any changes in the behavior of the product (as a result of features or fixes), for example, new Java system properties in the jvm.xml file. This section could also be used for any important information that doesn't fit elsewhere. -->

### Filebeat v6.2.2

This update uses Filebeat v6.2.2. Before installing this update, you must delete the Filebeat folder  `/apigateway/tools/filebeat-5.2.0`. When using Filebeat, follow the [official Filebeat documentation](https://www.elastic.co/guide/en/beats/filebeat/6.6/index.html).

### OpenJDK JRE

API Gateway and API Manager 7.7 and later support OpenJDK JRE, and this update includes Zulu OpenJDK 1.8 JRE instead of Oracle JRE 1.8.

### OpenSSL and FIPS support

This update uses OpenSSL 1.1.1, as OpenSSL 1.0.2 is EOL.

OpenSSL 1.1.1 does not support FIPS, and running API Gateway in FIPS mode is not supported in this update. OpenSSL 3.0 (when available) will support FIPS, and FIPS support will be available again in a future update.

References to FIPS in the documentation will not be removed, but this does not mean that FIPS is still supported and the references should be ignored.

### Increased validation of WSDLs

The Xerces library has been updated to `xerces 2.12.0`. This library enforces stricter rules when validating malformed schemas. This means that some WSDLs that were previously imported successfully by API Manager might not import successfully in this version.

To suppress schema validation errors and relax the stricter validation of XML files, set the flag `-DwsdlImport.suppressSchemaValidationErrors` to `true` in the `policystudio.ini` file. The default value is `false`.

### API Gateway behavior

#### Anonymous cipher suites

The JRE included in API Gateway disables undesirable cipher suites when using SSL/TLS by default. Users using RSA Access Manager (formerly known as RSA ClearTrust) with API Gateway might experience SSL/TLS handshake issues where no common cipher suites can be found. In this case, you should reconfigure SSL/TLS of the RSA Access Manager to support stronger cipher suites.

Alternatively, to re-enable the anonymous cipher suites in JRE for successful SSL/TLS connections with the RSA Access Manager, remove `anon`  from the  `jdk.tls.disabledAlgorithms` Java security property in the  `INSTALL_DIR/Linux.x86_64/jre/lib/security/java.security` file.

#### Endpoint identification property

The JRE included in API Gateway enables endpoint identification algorithms for LDAPS (secure LDAP over TLS) by default to  improve the robustness of the connections. This might cause API Gateway LDAP filters to fail to connect to an LDAPS server. To disable endpoint identification add the  `<VMArg  name="-Dcom.sun.jndi.ldap.object.disableEndpointIdentification=true"/>`  line to the  `INSTALL_DIR/system/conf/jvm.xml` file.

#### Connect to URL Filter

If the Connect to URL filter Request Protocol Headers field is not configured correctly, a runtime exception (`unexpected IO error`) is thrown when the filter is executed. In earlier versions, this error did not appear in the logs.

### API Manager behavior

#### Increased validation of `/users` endpoint

In earlier versions of API Manager, the `/users` API returns a list of all the users in an organization. This endpoint allowed a user to share an application with other users in their organization. This was identified as a security risk,  as only organization administrators and API administrators should have access to a list of users, and not user roles. The ability for user roles to view all other user names in the organization has been removed.

To reduce the impact of this change, you can relax this restriction using a configuration flag. Set the flag in the `jvm.xml` file (it does not exist by default) under `groups/group-x/instance-y/conf`.

```xml
<ConfigurationFragment>
    <VMArg name="-DAPIGW_TOGGLE_FEATURE_GET_ALL_USERS=true" />
</ConfigurationFragment>
```

#### Trailing slash property

An inbound API request with a trailing slash can match an API path with no trailing slash. To activate this feature set the Java property `com.vordel.apimanager.uri.path.trailingSlash.preserve` to `true`. The default value is `false`.

#### Content type property

An API method's Content-Type is checked against the API method's defined MIME type when performing path matching. To allow legacy API method matching and disable this check, set the Java property `com.coreapireg.apimethod.contenttype.legacy` to `true`. The default value is `false`.

#### Caching property

Enable caching to improve general system performance and speed. Set the `com.axway.apimanager.api.data.cache` Java system property to `true`. External clients, API keys, and OAuth credentials cache are optimized so that updates to the cache no longer block API Manager runtime traffic, resulting in performance improvements for corresponding API Manager APIs. As a result of the non-blocking cache updates, API Manager memory consumption will increase, particularly in systems with large numbers of external clients, API keys or OAuth credentials.

#### No match property

To configure the status code of an unsuccessful match of an API to 404 when authentication is successful, set the Java property `com.axway.apimanager.use404AuthSuccessNoMatch`  to `true`. The default value is `false`.

#### MIME types

To import API Gateway Management API Swagger into API Manager API Catalog, you must add the `application/x-download` MIME type to the default list of MIME types in API Gateway. Select **Server Settings > General > Mime configuration** in the Policy Studio tree and add `application/x-download` to the MIME list. After the configuration is deployed to API Gateway, you can import the API Gateway Manager API Swagger into API Manager API Catalog.

#### Confidential fields property

Fields that contain confidential information are no longer returned in some API calls. For example, a call to `GET /api/portal/v1.3/proxies/` does not return the password in the `AuthenticationProfile.parameters\["password"]` field. For compatibility with earlier versions, you can continue to return confidential fields. Set the system property `com.axway.apimanager.api.model.disable.confidential.fields` to `false` in the `jvm.xml` file (it does not exist by default) under `groups/group-x/instance-y/conf`.

```xml
<ConfigurationFragment>
    <VMArg name="-Dcom.axway.apimanager.api.model.disable.confidential.fields=false"/>
</ConfigurationFragment>
```

{{< alert title="Note" color="primary" >}}Setting this property to `false` is not recommended as it could pose a security risk to your API Gateway installation.{{< /alert >}}

### Security

#### `nosniff` header

The X-Content-Type-Options HTTP header with value `nosniff` is not included in a HTTP response serving static content from the API Gateway or API Manager. This static content is served from the API Gateway or API Manager `webapps` directory. No dynamic content is served from the `webapps` directory. This means that there is no risk of the browser making an incorrect assumption of the content type and exposing a security vulnerability. The X-Content-Type-Options response header with the value `nosniff` is included with HTTP responses serving dynamic content by default.

#### CSRF check property

If you are using the API Manager Management APIs, Client Application Registry APIs, and API Gateway APIs you might need to disable the CSRF token check implemented in v7.5.3 SP9 and later. To disable this check, set the Java system property `com.axway.apimanager.csrf`  to  `false`. The default is `true`.

### Custom filters

If you have written a custom filter using the extension kit, you might need to update your custom code as a result of changes in the classes.

The following interfaces deprecate `_()` and `__()` in favor of a new `resolve()` method:

* `com.vordel.client.manager.ResourceResolver`
* `com.vordel.client.manager.attr.ScreenAttribute`
* `com.vordel.client.manager.wizard.EntityContext`

Both `DefaultGUIFilter` and `VordelWizard` classes do not implement the `ResourceResolver` interface. As a result, any classes extending either of these must replace `_()` and `__()` method calls with `resolve()`.

## Deprecated features

<!-- Add features that are deprecated here -->

As part of our software development life cycle we constantly review our API Management offering.

The following capabilities have been deprecated.

### Antivirus scanners

API Gateway already supports the industry standard Internet Content Adaption Protocol (ICAP). From the November 2020 update the following embedded antivirus scanners will be removed:

* McAfee
* Sophos
* Clam AV

Content scanning is still supported using the ICAP filter, which provides out-of-the-box integration with ICAP-capable servers provided by Symantec, McAfee, OPSWAT and others, promoting ease of deployment and operational control.

## Removed features

<!-- Add features that are removed here -->

To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, no capabilities have been removed.

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID                                          | CVE Identifier | Description                                                                                                                                                                                                                                                                                                                                   |
| ----------- | ------------------------------------------------ | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-15490 | 01159022  01031336  01116092  01033168  01080239 |                | **Issue**: Headers were being reflected in the request response. **Resolution**: A new step has been added to policies which removes input headers from the response (only if not modified). This security feature is disabled by default, and you can enable it using the system property `-Dcom.axway.apigw.request.headers.reflect=false`. |

### Other fixed issues

| Internal ID | Case ID                                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----------- | -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-17431 | 01090329  01092098                     | **Issue**: API Gateway Policy Studio and `projdeploy` script attempt to redeploy configuration on error response from Admin Node Manager. **Resolution**: Policy Studio and `projdeploy` script attempt to deploy configuration only once as expected.                                                                                                                                                                                                                                                                                               |
| RDAPI-18519 | 01108119  01110455                     | **Issue**: The API Manager UI is too slow when using SSO. **Resolution**: The API Manager UI performance is improved by using existing cache of users when querying for a user by DN.                                                                                                                                                                                                                                                                                                                                                                |
| RDAPI-18649 | 01103847  01119176                     | **Issue**: KPS table is not updated with the current user role from the identity provider. **Resolution**: KPS is now updated with the latest user role after the user logs in using SSO.                                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-18735 | 01119493  01123904  01122491  01119522 | **Issue**: Previous versions of API Gateway do not allow underscores in host names unless system property `com.axway.apimanager.backend.url.validation.hostname.allowunderscore` is set to `true` in  `jvm.xml`. This contravenes <https://tools.ietf.org/html/rfc7230#section-5.4> and <https://tools.ietf.org/html/rfc3986#section-3.2.2> that allow underscores in host names. **Resolution**: Underscore characters are allowed in host names and `com.axway.apimanager.backend.url.validation.hostname.allowunderscore` is no longer evaluated. |
| RDAPI-18776 | 01125844                               | **Issue**: Regex provided as an example in custom property sample in `INSTALL_DIR/apigateway/webapps/apiportal/vordel/apiportal/app/app.config` does not work as expected. **Resolution**: Regex is now fixed.                                                                                                                                                                                                                                                                                                                                       |
| RDAPI-19028 | 01132045                               | **Issue**: API Manager reflected headers if there was a HTTP 429 Too Many Requests error, or a request body in generated responses for 400, 404, 405, and 429 HTTP errors, which were not handled by Global Fault handlers. **Resolution**: API Manager now uses the GenericFaultProcessor for these specific errors to stop the headers or request body from being reflected in a response. If the GenericFaultProcessor is not to be used for handling of API Manager specific errors, then set both `com.axway.apimanager.fault.legacy` and `com.axway.apimanager.fault.removeContentBody` Java system properties to true to stop API Manager reflecting the request body in the response. Also set `com.axway.apimanager.fault.resetHeaders.http429` Java system property to true to remove the headers from the response when there is HTTP 429 Too Many Requests error.
| RDAPI-19055 | 01169350  01134247  01129352  01134331                                                             | **Issue**: API Manager API Service Context is missing in open traffic and event logs in some scenarios. **Resolution**: The API Manager Service Context is now available for failed API method requests. The failed API method requests are shown as blocked in the API Manager monitoring.                                                                                                                                                                                                                                                          |
| RDAPI-19139 |                                                                                                    | **Issue**: The API Gateway ICAP client filter did not support the optional acceptance of 204 responses from an ICAP server, as described in RFC 3507 Section 4.6. This caused inefficiencies in ICAP transactions where no threat was found by the ICAP server.  **Resolution**: The API Gateway ICAP filter supports the optional acceptance of 204 responses from an ICAP server.                                                                                                                                                                  |
| RDAPI-19163 | 01136717                                                                                           | **Issue**: When editing an application in API Manager that was shared with the current user, descriptions for viewing the app appeared, although it was editable. **Resolution**: UI issue in API Manager was resolved and correct description appears when editing or viewing an application.                                                                                                                                                                                                                                                       |
| RDAPI-19216 | 01138498                                                                                           | **Issue**: Applications in pending approval state can incorrectly be exported from the UI, causing errors. **Resolution**: Applications in pending state cannot be exported from the UI.                                                                                                                                                                                                                                                                                                                                                             |
| RDAPI-19258 | 01140116  01140111                                                                                 | **Issue**: Swagger export adds default serialization attributes to query and path parameters when they are not present in the original file. **Resolution**: Default serialization attributes are no longer added to Swagger exports.                                                                                                                                                                                                                                                                                                                |
| RDAPI-19361 | 01139791                                                                                           | **Issue**: Missing validation for duplicate response codes in API creation in API Manager. **Resolution**: Added validation for duplicate response codes in API creation in API Manager.                                                                                                                                                                                                                                                                                                                                                             |
| RDAPI-19418 | 01142515                                                                                           | **Issue**: In API Manager, when the routing to a back-end API fails, attributes about the failure are missing in the message whiteboard. **Resolution**: The attributes `api.error.source` and `api.error.reason` are available in the message whiteboard.                                                                                                                                                                                                                                                                                           |
| RDAPI-19422 | 01144118                                                                                           | **Issue**: Incorrect name of parameter type in API Catalog. **Resolution**: Name changed from Location to Type to be consistent with other pages.                                                                                                                                                                                                                                                                                                                                                                                                    |
| RDAPI-19498 | 01145314                                                                                           | **Issue**: API Gateway Manager UI shows buttons **Show** and **Save** in Traffic view in cases where there is nothing to show or save. **Resolution**: Buttons are hidden in cases where there is no data to show or save.                                                                                                                                                                                                                                                                                                                           |
| RDAPI-19587 | 01148369                                                                                           | **Issue**: API Manager UI issues when a user tries to disable themselves. **Resolution**:  UI issues were resolved.                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| RDAPI-19588 | 01148385                                                                                           | **Issue**: Inconsistency in API Manager user management as self deletion was possible, but self disablement was not. **Resolution**: Self deletion is no longer possible.                                                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-19598 |                                                                                                    | **Issue**: TLS session resumption with preshared keys is not fully supported and sessions are never reused for outbound connections over TLS v1.3. **Resolution**: Session resumption with preshared keys is fully supported.                                                                                                                                                                                                                                                                                                                        |
| RDAPI-19609 | 01140447                                                                                           | **Issue**: A managedomain log message was incorrectly being thrown as an error, causing customer scripts to fail. **Resolution**: Updated the log message so that an error is no longer thrown.                                                                                                                                                                                                                                                                                                                                                      |
| RDAPI-19826 | 01176159  01151502  01154309  01167045  01171625  01173954  01154256  01155268  01176635  01156544 | **Issue**: Policy Studio performance is slow when a project has environmentalized parameters defined. **Resolution**: Performance of Policy Studio is improved for a project that has environmentalized parameters defined.                                                                                                                                                                                                                                                                                                                          |
| RDAPI-20017 | 01157558                                                                                           | **Issue**: In the **Compare Attribute** filter, negated match types return the wrong boolean value. **Resolution**: Updated the filter to return the correct boolean value.                                                                                                                                                                                                                                                                                                                                                                          |
| RDAPI-20021 | 01132032                                                                                           | **Issue**: Crash may occur in ConnectToUrl filter when configured with a static request body. **Resolution**: Message body is no longer shared between threads, and recreated from static configuration when used.                                                                                                                                                                                                                                                                                                                                                                          |
| RDAPI-20055 | 01154776                                                                                           | **Issue**: The Sentinel filters option **Use the following tracked object** did not work correctly. **Resolution**: The option works correctly and validation has been added to ensure that the Sentinel filters are configured correctly.                                                                                                                                                                                                                                                                                                           |
| RDAPI-20074 | 01157283  01156833                                                                                 | **Issue**: Unable to update API Manager back-end WSDL APIs. **Resolution**: Use `-Dcom.axway.apimanager.apirepo.allowWSDLUpdate=true` system property to allow updates to backend WSDL APIs.                                                                                                                                                                                                                                                                                                                                                         |
| RDAPI-20175 | 01157334  01158423                                                                                 | **Issue**: API Manager could not save APIs with no host set. **Resolution**: API Manager now saves APIs with no host set.                                                                                                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-20218 | 01101486  01168599                                                                                 | **Issue**: In API Manager, all API access could not be revoked from applications through the `api-promote` script. **Resolution**: A new property, `application.apis.remove.all`, has been added to the script to allow users to remove all API access from applications.                                                                                                                                                                                                                                                                            |
| RDAPI-20292 | 01127526  01133840                                                                                 | **Issue**: API Manager was not encoding special characters when downloading a Swagger definition. **Resolution**: API Manager is now encoding special characters when downloading a Swagger definition.                                                                                                                                                                                                                                                                                                                                              |
| RDAPI-20459 | 01163040                                                                                           | **Issue**: API Gateway returning a body response to an OPTIONS request while headers specify a content length of 0 causes clients to abort the connection. **Resolution**: API Gateway OPTIONS handling has been corrected to no longer generate a body when specifying a content length of 0.                                                                                                                                                                                                                                                       |
| RDAPI-20461 | 01158374  01153597                                                                                 | **Issue**: API Manager decodes multipart form parameters as strings in some cases. **Resolution**: API Manager handles multipart form parameters as files without string decoding.                                                                                                                                                                                                                                                                                                                                                                   |
| RDAPI-20490 | 01160858                                                                                           | **Issue**: Domain Audit APIs documentation do not include Node Manager as a possible instance parameter. **Resolution**: API Server or Node Manager instances included in the documentation.                                                                                                                                                                                                                                                                                                                                                         |
| RDAPI-20495 | 01165403                                                                                           | **Issue**: On Linux, managedomain command fails to run openssl command when non-root privileges are granted to the vshell binary. **Resolution**: The managedomain command has been corrected to use vrun script.                                                                                                                                                                                                                                                                                                                                    |
| RDAPI-20506 | 01167562  01162999  01166742                                                                       | **Issue**: In API Manager, under certain circumstances,  when authenticating a request using multiple security devices in a security profile, an authentication failure in any of the security devices causes the request to be rejected. **Resolution**: Authentication is now successful when one of the security devices in the security profile successfully authenticates the caller.                                                                                                                                                           |
| RDAPI-20515 | 01165136                                                                                           | **Issue**: Policy Studio is very slow to open projects with polices containing many **Set Attribute** filters. **Resolution**: Policy performance is improved when opening projects with polices containing many **Set Attribute** filters.                                                                                                                                                                                                                                                                                                          |
| RDAPI-20565 | 01168618  01169310  01147875                                                                       | **Issue**: `update_apigw.sh` script fails with `NotImplementedError: passwd.pw_passwd unimplemented` message. **Resolution**: `update_apigw.sh` script works correctly.                                                                                                                                                                                                                                                                                                                                                                              |
| RDAPI-20572 | 01168594                                                                                           | **Issue**: After upgrading to the May20 release, API Manager SSO login fails and shows an error to map an organization. **Resolution**: API Manager SSO login maps the organization correctly, and the login is successful.                                                                                                                                                                                                                                                                                                                          |
| RDAPI-20901 | 01154736  01163509                                                                                 | **Issue**: `Content-*` header parameters are not properly validated in API Manager. **Resolution**:  `Content-*` header parameters can be used as any other header parameter.                                                                                                                                                                                                                                                                                                                                                                        |
| RDAPI-20920 | 01172884                                                                                           | **Issue**:  `getCertFromP12` and  `getKeyFromP12` methods of class `CertStoreUtil` defined in `$INSTALL_DIR/apigateway/system/lib/jython/cert.py` are using classes not imported in the jython file. This causes scripts that invoke such methods to fail. **Resolution**: Imported clauses have been added to `cert.py` file.                                                                                                                                                                                                                       |
| RDAPI-20927 | 01147261                                                                                           | **Issue**: Errors are raised by CRL validation when there are multiple CAs with same DN in the certificate store. **Resolution**: CRL validation will no longer raise errors when multiple issuers are found for a certificate. In such case the certificate and its issuers must be filtered before invoking the right CRL filter.                                                                                                                                                                                                                  |
| RDAPI-20958 | 01122859  01132092  01133854  01064716                                                             | **Issue**: Changes to an organization name or organization that an application is associated with causes duplication in the API Manager metrics reports. **Resolution**: API Manager metrics reports restrict report totals using the organization name the application is associated with.                                                                                                                                                                                                                                                          |
| RDAPI-20959 | 01129922  01107031                                                                                 | **Issue**: API Manager metrics reports group database results differently causing processing time averages calculated to differ between reports. **Resolution**: Processing time averages are now processed independent of database grouping.                                                                                                                                                                                                                                                                                                        |
| RDAPI-21025 | 01172150                                                                                           | **Issue**: When product traces are configured with a level of DEBUG, traces contain a lot of data related messages such as `new chunk ...`. **Resolution**: This trace is only be logged with level DATA.                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-21079 | 01147692  01129486  01129357                                                                       | **Issue**: In API Manager, ApiShunt is not present in the message whiteboard when the fault handler is being executed. **Resolution**: An `api.error.reason` message attribute is added to the message whiteboard prior to the fault handler policy being executed.                                                                                                                                                                                                                                                                                  |
| RDAPI-21085 | 01160271  01143897  01153011  01160321  01160146                                                   | **Issue**: API Manager was creating sessions incorrectly resulting in possible authentication issues during high traffic. **Resolution**: API Manager only creates a session upon successful login.                                                                                                                                                                                                                                                                                                                                                  |
| RDAPI-21136 | 01109290  01071882  01108056                                                                       | **Issue**: Using the JVM property `http.nonProxyHosts` in API Manager did not work as expected. It had no effect on the routing of requests through a proxy server. **Resolution**: Enabled the use of the `http.nonProxyHosts` property in API Manager and it can now be used to define the hosts that should not be routed through a proxy.                                                                                                                                                                                                        |
| RDAPI-21212 | 01089014                                                                                           | **Issue**: In the Export Dialog tree, for Environment Settings, all filter items were named as **Environmentalized Fields** so you do not know what is being selected. **Resolution**: Updated the labeling to replace **Environmentalized Fields** with a description of the associated filter.                                                                                                                                                                                                                                                     |

## Known issues

The following are known issues for this update.

| Internal ID | Description                                                                                                                                                    |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-11143 | Discrepancy with API retirement dates.                                                                                                                         |
| RDAPI-15981 | Scopes fields for API Key remain visible even if Application Scopes are disabled.                                                                              |
| RDAPI-16486 | Changes in the mapper always require a reload in the Execute Data Maps filter and when reloaded providing values for the required parameters must be repeated. |
| RDAPI-17282 | Connector for Salesforce APIs in API Manager does not work or is impossible to configure.                                                                      |
| RDAPI-18198 | CORS preflight fails for WSDL based API Manager APIs, and Try-it fails.                                                                                        |
| RDAPI-18332 | Try-it for API-Method is not working.                                                                                                                          |
| RDAPI-18431 | HTTP 409 Resource already exists in Applications - External Credentials.                                                                                       |
| RDAPI-18523 | Inconsistent application search behavior relating to application sharing.                                                                                      |
| RDAPI-18674 | Insufficient data validation when importing an Application.                                                                                                    |
| RDAPI-18777 | Overriding the quota for an application and then removing the setting causes incorrect behavior.                                                               |
| RDAPI-18986 | `projpack` is unable to merge projects after `projupgrade`; likely due to default API Manager policies.                                                        |
| RDAPI-18990 | `Failed to delete undefined` appears unexpectedly when attempting to delete application.                                                                       |
| RDAPI-19006 | Delete API `not found` after changing Application Org.                                                                                                         |
| RDAPI-19132 | Issue with selection of Retirement date when deprecating API.                                                                                                  |
| RDAPI-19150 | Try-it in API Manager only shows first 10 API keys.                                                                                                            |
| RDAPI-19240 | Users in pending approval state are visible in the Sharing tab.                                                                                                |
| RDAPI-19262 | X-Rate-Limit header shows inconsistent values in a multi-node Manager environment.                                                                             |
| RDAPI-19278 | API access removed from app during org migration.                                                                                                              |
| RDAPI-19292 | When an API Manager admin user's login name is changed, the user is directed to a blank page.                                                                  |
| RDAPI-19293 | API Catalog Try-it shows only the first security device of a security profile.                                                                                 |
| RDAPI-19354 | Issue with back-end API import regarding Duplicate parameters value.                                                                                           |
| RDAPI-19433 | Line breaks in outbound parameter (type header) value not escaped.                                                                                             |
| RDAPI-19442 | Saving Mode Stuck for the Application Creation in different session.                                                                                           |
| RDAPI-19453 | Uploading an invalid image type when creating an Application leads to an internal server error.                                                                |
| RDAPI-19580 | Trial option in the Organization does not work.                                                                                                                |
| RDAPI-19586 | Retired API appears as published in Catalog.                                                                                                                   |
| RDAPI-19601 | Sharing section of Application issue when Organization of application changed.                                                                                 |
| RDAPI-19787 | Custom filter jabber sample missing but mentioned in docs.                                                                                                     |
| RDAPI-19788 | Issue with pagination across multiple sections.                                                                                                                |
| RDAPI-19833 | Okta SSO integration and email mapping.                                                                                                                        |
| RDAPI-19849 | OAuth2 Client Credential cache not considering the scope or user.                                                                                              |
| RDAPI-19915 | Update minimum system requirements in regards to cassandra production use.                                                                                     |
| RDAPI-19971 | nodemanager pid file is created with permissions that are too restrictive.                                                                                     |
| RDAPI-20091 | In Policy Studio, when importing a policy fragment, deselected items are imported anyway.                                                                      |
| RDAPI-20127 | Selector `${content.body.getJSON().get(0)}` not working.                                                                                                       |
| RDAPI-20255 | Application Quota not promoted by `apimanger-promote`.                                                                                                         |
| RDAPI-20294 | Authorization code flow with OpenID produces invalid query string as response.                                                                                 |
| RDAPI-20464 | PolicyStudio deployment errors (WSDL) after applying March 20 release.                                                                                         |
| RDAPI-20474 | Swagger file size limit.                                                                                                                                       |
| RDAPI-20480 | Swagger enum query parameters not validated.                                                                                                                   |
| RDAPI-20526 | Groovy bug GROOVY-6975.                                                                                                                                        |
| RDAPI-20594 | When a token is revoked with an incorrect Authorization header, the response is 400 instead of 401.                                                            |
| RDAPI-20919 | Add timeout type and origin in trace message.                                                                                                                  |
| RDAPI-20921 | Incorrect `Bad encoding for integer value` errors reported during sysupgrade.                                                                                  |
| RDAPI-20923 | API Manager application selection is broken.                                                                                                                   |
| RDAPI-20925 | Defect OAS3: API Manager lacks free-form parameter support.                                                                                                    |
| RDAPI-21030 | API Manager ignores `dont.expect.100.continue` flag if the Outbound Security is HTTP Basic.                                                                    |
| RDAPI-21171 | Minor UI issue affecting pagination in API keys and Oauth client credentials.                                                                                  |

### Policy Studio help documentation

This update includes changes to the Policy Studio documentation, but the version of the documentation (Eclipse Help) shipped with Policy Studio is out of sync with the online documentation and does not include these changes.

We are working to rectify this issue in a future update. In the meantime, you can find the up to date documentation online at [Develop in Policy Studio](/docs/apim_policydev/).

## Update a classic (non-container) deployment

These instructions apply to API Gateway and API Manager classic deployments only. For container deployments, see [Update a container deployment](#update-a-container-deployment).

### Prerequisites

This update has the following prerequisites in addition to the [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

1. Shut down any Node Manager or API Gateway instances on your existing installation.
2. Back up your existing installation. You can use the `update_apigw.sh` script to take a backup of your entire API Gateway installation directory as detailed in the [API Gateway server install steps](#install-the-api-gateway-server-update), or you can manage your own backups as detailed in [API Gateway backup and disaster recovery](/docs/apim_administration/apigtw_admin/manage_operations/#api-gateway-backup-and-disaster-recovery). Ensure that you back up any customized files. You should merge updated files instead of copying them back directly to avoid any regex matching issues, whether you manage your own backups or not. For example, the following directories might contain customized files:

   ```
   webapps/apiportal/vordel/apiportal
   webapps/emc/vordel/manager/app
   webapps/emc
   system/conf/apiportal/email
   system/conf
   samples/scripts/
   tools/filebeat-VERSION-PLATFORM
   ```
3. If you have an existing Apache Cassandra installation, ensure that you back up your data (Cassandra and `kpsadmin`), and that the `JAVA_HOME` variable is set correctly in `cassandra.in.sh` and `cassandra.in.bat`.
4. Remove the old Filebeat folder `/apigateway/tools/filebeat-5.2.0`. Check any customized files to see if they are compatible with the new version. See [Filebeat](#filebeat-v6-2-2) for more information.
5. On Linux, remove existing capabilities on product binaries (which might prevent overwriting files):

   ```
   setcap -r INSTALL_DIR/apigateway/platform/bin/vshell
   ```

### FIPS mode only

If FIPS mode is enabled, you must also perform the following steps to install the update:

1. Run `togglefips --disable` to turn FIPS mode off.
2. Start the Node Manager to move the JARs.
3. Stop the Node Manager.
4. Install the API Gateway update as described in the [Installation](#installation) section.
5. Start the Node Manager.

In Policy Studio, If FIPS mode is enabled, you must also perform the following steps to install the update:

1. Open Policy Studio and select **Window > Preferences**.
2. Select the **FIPS Mode** setting.
3. Deselect the **Enable FIPS Mode in Axway Policy Studio** option and click **OK**.
4. Restart Policy Studio using the `policystudio -clean` command.

In Configuration Studio, If FIPS mode is enabled, you must also perform the following steps to install the update:

1. Open Configuration Studio and select **Window > Preferences**.
2. Select the **FIPS Mode** setting.
3. Deselect the **Enable FIPS Mode in Axway Configuration Studio** option and click **OK**.
4. Restart Configuration Studio using the `configurationstudio -clean` command.

### Installation

This section describes how to install the update on existing 7.7 installations of API Gateway or API Manager.

* If you have installed an existing version of API Manager, installing the API Gateway server update automatically also installs the updates and fixes for API Manager.
* If you have installed a licensed version of API Gateway or API Manager 7.7, you do not require a new license to install updates.

#### Install the API Gateway server update

To install the update on your existing API Gateway 7.7 server installation, perform the following steps:

1. Ensure that your existing API Gateway instance and Node Manager have been stopped.
2. Remove any previous patches from your `INSTALL_DIR/ext/lib` and `INSTALL_DIR/META-INF` directories (or the `ext/lib` directory in an API Gateway instance). These patches have already been included in this update. You do not need to copy patches from a previous version.
3. Download and unpack the API Gateway 7.7 server Update file into a new directory. For example:

   ```
   mkdir 77update
   tar -xzvf APIGateway_7.7.YYYYMMDD_Core_linux-x86-64_BNnn.tar.gz -C 77update
   ```

    {{< alert title="Note" color="primary" >}}You must extract the file into a new directory and not into the existing API Gateway installation directory.{{< /alert >}}
4. Run the [`update_apigw.sh` script](#update-apigw-sh-script) from the directory into which you extracted the Update file (for example, `77update`) and specify  your API Gateway installation directory using the `--install_dir` option. For example:

   ```
   ./update_apigw.sh --install_dir /opt/Axway-7.7/
   ```
5. Restart your Node Manager and API Gateway instances on the local machine.

##### `update_apigw.sh` script

Run the `update_apigw.sh` script with the `--help` option to see the available options:

```
./update_apigw.sh --help
```

The script generates a trace file in the `update-output/trace` directory. Use the `--tracelevel` option to change the level of tracing.

The script takes a backup of your entire API Gateway installation directory and places it in a `tar` file in the `update-output/backups` directory. Specify a different directory using the `--backup_dir` option. To manage your own backups, use the `--no_backup` option.

To run the script without user interaction, specify `--mode unattended` option.

Running the `update_apigw.sh` script performs the following steps:

1. Check that the installation directory is valid.
2. Check that the user who owns the API Gateway binaries is the same user running the `update_apigw.sh` script.
3. Check that the Node Manager and API Gateway instances that run on the local machine are not running.
4. Take a backup unless `--no_backup` has been specified.
5. Install the update content into the API Gateway installation directory.
6. Perform all of the steps that were performed by a post installation script in earlier updates. This includes JRE cleanup, `system/lib/modules` cleanup, third-party and Axway JAR cleanup, apply `acl.json` fix, apply passphrase obfuscation fix, and apply modifications to Node Manager entity store configuration. Fixes are only applied if they have not previously been applied.

#### Install the Policy Studio update

To install the update on your existing Policy Studio installation, an update script is provided. The update script is located inside the API Gateway 7.7 Policy Studio Update pack (for example, `APIGateway_7.7.YYYYMMDD_PolicyStudio_linux-x86-64_BNnn.tar.gz`).

Download and unpack the API Gateway 7.7 Policy Studio Update file into a new directory. For example:

```
mkdir 77update
tar -xzvf APIGateway_7.7.YYYYMMDD_PolicyStudio_linux-x86-64_BNnn.tar.gz -C 77update
```

{{< alert title="Note" color="primary" >}}
You must extract the file into a new directory and not into the existing API Gateway installation directory.

You must also remove the files `libeay32.dll` and `ssleay32.dll` if they exist in the directory `INSTALL_DIR/policystudio`.

For installations running on Windows 7, you must manually unzip the Policy Studio update.
{{< /alert >}}

Run the `update_policy_studio.sh` script from the directory into which you extracted the Update file (for example, `77update`) and specify your API Gateway installation directory as an argument:

```
./update_policy_studio.sh INSTALL_DIR
```

`INSTALL_DIR` is the base API Gateway 7.7 installation directory that contains the `policystudio` directory.

For example:

```
./update_policy_studio.sh /opt/Axway-7.7/
```

If you had applied any modifications to the `policystudio.ini` file, you must reapply them after upgrade.

{{< alert title="Note" color="primary" >}}You must execute the update script using the same user who installed Policy Studio.

An update script is also available for Windows. It is called `update_policy_studio.bat` and it is located in the API Gateway 7.7 Policy Studio Update pack for Windows (`.zip`).{{< /alert >}}

Running this script performs the following steps:

1. Back up your existing `INSTALL_DIR/policystudio` directory.
2. Remove old JRE versions by deleting the `INSTALL_DIR/policystudio/jre` directory.
3. Unzip and extract API Gateway 7.7 Policy Studio Update over the `policystudio` directory in your existing API Gateway 7.7 installation directory.
4. Start Policy Studio with `policystudio -clean`.

A backup of the installation is created at `INSTALL_DIR/backups/policystudio/<date_time>`.

#### Install the Configuration Studio update

To install the update on your existing Configuration Studio installation, an update script is provided. The update script is located inside the API Gateway 7.7 Configuration Studio Update pack (for example, `APIGateway_7.7.YYYYMMDD_ConfigurationStudio_linux-x86-64_BNnn.tar.gz`).

Download and unpack the API Gateway 7.7 Configuration Studio Update file into a new directory. For example:

```
mkdir 77update
tar -xzvf APIGateway_7.7.YYYYMMDD_ConfigurationStudio_linux-x86-64_BNnn.tar.gz -C 77update
```

{{< alert title="Note" color="primary" >}}
You must extract the file into a new directory and not into the existing API Gateway installation directory.

You must also remove the files `libeay32.dll` and `ssleay32.dll` if they exist in the directory `INSTALL_DIR/configurationstudio`.

For installations running on Windows 7, you must manually unzip the Configuration Studio update.
{{< /alert >}}

Run the `update_configuration_studio.sh` script from the directory into which you extracted the Update file (for example, `77update`) and specify your API Gateway installation directory as an argument:

```
./update_configuration_studio.sh INSTALL_DIR
```

`INSTALL_DIR` is the base API Gateway 7.7 installation directory that contains the `configurationstudio` directory.

For example:

```
./update_configuration_studio.sh /opt/Axway-7.7/
```

If you had applied any modifications to the `configurationstudio.ini` file, you must reapply them after upgrade.

{{< alert title="Note" color="primary" >}}You must execute the update script using the same user who installed Configuration Studio.

An update script is also available for Windows. It is called `update_configuration_studio.bat` and it is located in the API Gateway 7.7 Configuration Studio Update pack for Windows (`.zip`).{{< /alert >}}

Running this script performs the following steps:

1. Back up your existing `INSTALL_DIR/configurationstudio` directory.
2. Remove old JRE versions by deleting the `INSTALL_DIR/configurationstudio/jre` directory.
3. Unzip and extract API Gateway 7.7 Configuration Studio Update over the `configurationstudio` directory in your existing API Gateway 7.7 installation directory.
4. Start Configuration Studio with `configurationstudio -clean`

A backup of the installation is created at `INSTALL_DIR/backups/configurationstudio/<date_time>`.

#### Install the API Gateway Analytics update

To install the update on your existing API Gateway Analytics 7.7 installation, perform the following steps:

1. Ensure that your existing API Gateway Analytics instance and Node Manager have been stopped.
2. Remove old third-party libraries by deleting the following directories:

   ```
   INSTALL_DIR/analytics/system/lib/modules
   ```
3. Remove old JRE versions by deleting the following directories:

   ```
   INSTALL_DIR/analytics/platform/jre
   ```
4. Verify the owners of API Gateway binaries before extracting the update.

   ```
   ls -l INSTALL_DIR/analytics/posix/bin
   ```
5. Using the same user who owns the API Gateway Analytics binaries, unzip and extract API Gateway 7.7 Analytics Update over the `analytics` directory in your existing API Gateway 7.7 installation directory. For example:

   ```
   tar -xzvf APIGateway_7.7.YYYYMMDD_Analytics_linux-x86-64_BNnn.tar.gz -C /opt/Axway-7.7/analytics/
   ```
6. Change to the `analytics` directory in your installation:

   ```
   cd INSTALL_DIR/analytics
   ```
7. Run the post-install script for API Gateway Analytics.

   ```
   apigw_analytics_sp_post_install.sh
   ```

You must also install an update for your existing API Gateway 7.7 server.

### After installation

The following steps apply after installing the update.

#### API Gateway

To allow an unprivileged user to run the API Gateway on a Linux system, perform the following steps:

1. Add the following line to the `INSTALL_DIR/system/conf/jvm.xml` file:

   ```
   <VMArg name="-Djava.library.path=$VDISTDIR/$DISTRIBUTION/jre/lib/amd64/server:$VDISTDIR/$DISTRIBUTION/jre/lib/amd64:$VDISTDIR/$DISTRIBUTION/lib/engines:$VDISTDIR/ext/$DISTRIBUTION/lib:$VDISTDIR/ext/lib:$VDISTDIR/$DISTRIBUTION/jre/lib:system/lib:$VDISTDIR/$DISTRIBUTION/lib"/>
   ```
2. Run the command `setcap 'cap_net_bind_service=+ep cap_sys_rawio=+ep' INSTALL_DIR/platform/bin/vshell` to allow the API Gateway to listen on privileged ports.

#### API Manager

When API Manager is installed, you must run the `update-apimanager` script after the API Gateway post-install script to ensure that all paths are up-to-date. For details, see [Run update-apimanager](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/#run-update-apimanager).

{{< alert title="Caution" color="warning" >}} Before executing the `update-apimanager` script:

* Apply the update to all API Gateways.
* Ensure that all Node Managers and API Gateway instances are running.

{{< /alert >}}

## Update a container deployment

If a `fed` file is provided as part of building the API Manager container, you must follow these steps to update the `fed` with the configuration changes:

1. Install the update on a installation of the API Gateway.
2. Run the following command:

   ```
   /opt/Axway-7.7/apigateway/posix/bin/update-apimanager --fed <path to old file>.fed --oa <path to update file>.fed
   ```

You do not need to run any API Manager instances.

The `fed` now contains the updates for the API Manager configuration and can be used to build containers.

## Documentation

This section describes documentation enhancements and related documentation.

### Documentation enhancements

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
