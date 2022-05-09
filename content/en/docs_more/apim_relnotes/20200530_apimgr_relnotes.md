---
title: API Gateway and API Manager 7.7 May 2020 Release Notes
linkTitle: API Gateway and API Manager May 2020
weight: 140
date: 2020-03-11T00:00:00.000Z
---
## Summary

API Gateway is available as a software installation or a virtualized deployment in Docker containers. API Manager is a licensed product running on top of API Gateway, and has the same deployment options as API Gateway.

The software installation is available on Linux. For more details on supported platforms for software installation, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

Docker deployment is supported on Linux. For a summary of the system requirements for a Docker deployment, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

## New features and enhancements

### Updated OS Support

The operating systems CentOS 7, CentOS 8, RedHat 7, and RedHat 8 have been tested and validated.

### Improved upgrade experience

* You can use the script `update_apigw.sh` for updating API Gateway and API Manager installations, replacing the post installation script `apigw_sp_post_install.sh`. This automates some of the previous manual steps. It supports backups and can run in unattended mode. For more information about the usage of the script see the [installation steps](#installation) and the `--help` output
* Automation of some manual steps for update.
* The `update_apimanager.sh` script still required.
* The update is still applied manually to each node in the topology.
* There are [new scripts](/docs/apim_reference/scripts_changelog_sp/) to apply updates for Policy Studio and Configuration Studio.

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.

<!-- Use this section to describe any changes in the behavior of the product (as a result of features or fixes), for example, new Java system properties in the jvm.xml file. This section could also be used for any important information that doesn't fit elsewhere. -->

### TLS v1.3 support

This update supports TLS v1.3 protocol for HTTPS listeners and connection filters. All communication between Node Managers, API Gateways, and other services now uses TLS v1.3 protocol by default.

### Filebeat v6.2.2

This update uses Filebeat v6.2.2. Before installing this update, you must delete the Filebeat folder  `/apigateway/tools/filebeat-5.2.0`. When using Filebeat, follow the [official Filebeat documentation](https://www.elastic.co/guide/en/beats/filebeat/6.6/index.html).

### OpenJDK JRE

API Gateway and API Manager 7.7 and later support OpenJDK JRE, and this update includes Zulu OpenJDK 1.8 JRE instead of Oracle JRE 1.8.

### OpenSSL and FIPS support

This update uses OpenSSL 1.1.1, as OpenSSL 1.0.2 is EOL.

OpenSSL 1.1.1 does not support FIPS, and running API Gateway in FIPS mode is not supported in this update. OpenSSL 3.0 (when available) will support FIPS, and FIPS support will be available again in a future update.

References to FIPS in the documentation will not be removed, but this does not mean that FIPS is still supported and the references should be ignored.

### Log4j 2 changes

The Log4j 2 version has been updated from 2.8.2 to 2.13.2. In addition, the Log4j 2 file format has been changed to YAML.

| 7.7 releases prior to May 2020                           | 7.7 May 2020 and later                                    |
| -------------------------------------------------------- | --------------------------------------------------------- |
| `$VDISTIR/system/conf/loggers/eventLog.xml`              | `$VDISTIR/system/conf/loggers/eventLog.yaml`              |
| `$VDISTIR/system/conf/loggers/openTrafficLog.xml`        | `$VDISTIR/system/conf/loggers/openTrafficLog.yaml`        |
| `$VDISTDIR/system/conf/loggers/topologyLog.xml`          | `$VDISTDIR/system/conf/loggers/topologyLog.yaml`          |
| `$VDISTDIR/system/conf/log4j2.xml`                       | `$VDISTDIR/system/conf/log4j2.yaml`                       |
| `$VDISTDIR/rcplatform/plugin-filter-base/lib/log4j2.xml` | `$VDISTDIR/rcplatform/plugin-filter-base/lib/log4j2.yaml` |

If you have made any customizations to these XML files you must translate them to the equivalent YAML files.

### Swagger parser

The Swagger parser libraries have been updated as follows.

| Parser                | Previous version | New version |
| --------------------- | ---------------- | ----------- |
| Swagger Legacy Parser | 1.0.48           | 1.0.50      |
| OpenAPI 3 Parser      | 2.0.16           | 2.0.19      |

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

### Deprecated libraries

From the July 2020 update the following API Gateway libraries will be replaced by the listed artifacts under the `system/lib` folder.

| May 2020 library              | July 2020 artifact                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `anttasks.jar`                | `vordel-core-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `apiportal.jar`               | `vordel-core-7.7.0.*.jar`, `vordel-apigateway-7.7.0.*.jar`, `vordel-apibroker-7.7.0.*.jar`, `vordel-apimanager-7.7.0.*.jar`, `vordel-api-io-7.7.0.*.jar`, `vordel-api-model-7.7.0.*.jar`, `vordel-core-api-7.7.0.*.jar`, `vordel-core-controller-7.7.0.*.jar`, `vordel-core-model-7.7.0.*.jar`, `vordel-metrics-7.7.0.*.jar`, `vordel-persistence-7.7.0.*.jar`, `vordel-swagger-builder-7.7.0.*.jar`, `vordel-swagger-model-7.7.0.*.jar`, `plugins/vordel-common-7.7.0.*.jar`                                                                                                                |
| `circuit.jar`                 | `vordel-core-circuit-7.7.0.*.jar`, `vordel-core-precipitate-7.7.0.*.jar`, `vordel-core-runtime-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `client.jar`                  | `vordel-core-circuit-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `common.jar`                  | `vordel-apibroker-7.7.0.*.jar`, `vordel-apimanager-7.7.0.*.jar`, `vordel-apigateway-7.7.0.*.jar`, `vordel-api-model-7.7.0.*.jar`, `vordel-core-7.7.0.*.jar`, `vordel-core-api-7.7.0.*.jar`, `vordel-core-circuit-7.7.0.*.jar`, `vordel-core-circuit-format-7.7.0.*.jar`, `vordel-core-controller-7.7.0.*.jar`, `vordel-core-model-7.7.0.*.jar`, `vordel-core-runtime-7.7.0.*.jar`, `plugins/vordel-common-7.7.0.*.jar`                                                                                                                                                                       |
| `com.vordel.jms.jar`          | `vordel-core-circuit-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `jwkjose.jar`                 | `vordel-core-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `jwsdl-emf-impl.jar`          | `vordel-xml-utils-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `logger.jar`                  | `vordel-core-7.7.0.*.jar`, `vordel-apigateway-7.7.0.*.jar`, `vordel-core-runtime-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `manager.jar`                 | `vordel-apibroker-7.7.0.*.jar`, `vordel-core-circuit-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `oauthclient.jar`             | `vordel-core-circuit-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `openidconnect.jar`           | `vordel-core-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `precipitate.jar`             | `vordel-core-precipitate-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `security.jar`                | `vordel-core-7.7.0.*.jar`, `vordel-apigateway-7.7.0.*.jar`, `vordel-core-circuit-7.7.0.*.jar`, `vordel-core-http-7.7.0.*.jar`, `plugins/vordel-common-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `server.jar`                  | `vordel-core-7.7.0.*.jar`, `vordel-apibroker-7.7.0.*.jar`, `vordel-apigateway-7.7.0.*.jar`, `vordel-api-model-7.7.0.*.jar`, `vordel-common-db-7.7.0.*.jar`, `vordel-core-circuit-7.7.0.*.jar`, `vordel-core-http-7.7.0.*.jar`, `vordel-core-model-7.7.0.*.jar`, `vordel-core-precipitate-7.7.0.*.jar`, `vordel-core-runtime-7.7.0.*.jar`, `vordel-core-servletcontainer-7.7.0.*.jar`, `vordel-kps-7.7.0.*.jar`, `vordel-metrics-7.7.0.*.jar`, `vordel-nodemanager-client-7.7.0.*.jar`, `vordel-persistence-7.7.0.*.jar`, `vordel-xml-utils-7.7.0.*.jar`, `plugins/vordel-common-7.7.0.*.jar` |
| `upgrade.jar`                 | `vordel-apigateway-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `wspolicy.jar`                | `vordel-core-circuit-7.7.0.*.jar`, `vordel-xml-utils-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `xmlutils.jar`                | `vordel-xml-utils-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `embeddedAMQ/embeddedAMQ.jar` | `vordel-core-7.7.0.*.jar`, `vordel-apigateway-7.7.0.*.jar`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

### Renamed artifacts

From the July 2020 update the following artifacts under the `system/lib` folder will be renamed.

| May 2020 artifact                       | July 2020 artifact                       |
| --------------------------------------- | ---------------------------------------- |
| `com.vordel.circuit.cache-7.7.0.*.jar`  | `vordel-core-circuit-cache-7.7.0.*.jar`  |
| `com.vordel.circuit.format-7.7.0.*.jar` | `vordel-core-circuit-format-7.7.0.*.jar` |
| `com.vordel.circuit.smtp-7.7.0.*.jar`   | `vordel-core-circuit-smtp-7.7.0.*.jar`   |

### SDK libraries

From the July 2020 update the SDK libraries will be updated.

| Java package                         | Old JAR                        | New JAR                                                                                                     |
| ------------------------------------ | ------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| `com.vordel.api.adminusers.client`   | `server.jar`                   | `vordel-apigateway-7.7.0.*.jar`                                                                             |
| `com.vordel.api.adminusers.model`    | `server.jar`                   | `vordel-apigateway-7.7.0.*.jar`                                                                             |
| `com.vordel.api.configuration.model` | `server.jar`                   | `vordel-apigateway-7.7.0.*.jar`                                                                             |
| `com.vordel.api.deployment.client`   | `server.jar`                   | `vordel-apigateway-7.7.0.*.jar`                                                                             |
| `com.vordel.api.deployment.model`    | `server.jar`                   | `vordel-apigateway-7.7.0.*.jar`                                                                             |
| `com.vordel.api.monitoring.model`    | `server.jar`                   | `vordel-apigateway-7.7.0.*.jar`                                                                             |
| `com.vordel.api.nm`                  | `server.jar`                   | `vordel-apigateway-7.7.0.*.jar`, `vordel-nodemanager-client-7.7.0.*.jar`                                    |
| `com.vordel.api.topology.client`     | `server.jar`                   | `vordel-nodemanager-client-7.7.0.*.jar`                                                                     |
| `com.vordel.api.topology.model`      | `server.jar`                   | `vordel-core-model-7.7.0.*.jar`                                                                             |
| `com.vordel.circuit`                 | `circuit.jar`                  | `vordel-core-circuit-7.7.0.*.jar`, `vordel-core-precipitate-7.7.0.*.jar`, `vordel-core-runtime-7.7.0.*.jar` |
| `com.vordel.circuit.oauth.token`     | `com.vordel.circuit.oauth.jar` | `vordel-core-circuit-7.7.0.*.jar`                                                                           |
| `com.vordel.config`                  | `vordel-config-7.7.0.*.jar`    | `vordel-config-7.7.0.*.jar`                                                                                 |
| `com.vordel.dwe`                     | `server.jar`                   | `vordel-core-7.7.0.*.jar`                                                                                   |
| `com.vordel.env`                     | `vordel-env-7.7.0.*.jar`       | `vordel-env-7.7.0.*.jar`                                                                                    |
| `com.vordel.mime`                    | `server.jar`                   | `vordel-core-circuit-7.7.0.*.jar`, `vordel-core-runtime-7.7.0.*.jar`                                        |
| `com.vordel.oauth.client.providers`  | `oauthclient.jar`              | `vordel-core-circuit-7.7.0.*.jar`                                                                           |
| `com.vordel.reporting.rtm.api`       | `server.jar`                   | `vordel-metrics-7.7.0.*.jar`                                                                                |
| `com.vordel.statistics`              | `server.jar`                   | `vordel-core-runtime-7.7.0.*.jar`                                                                           |

## Removed features

<!-- Add features that are removed here -->

To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, no capabilities have been removed.

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID                                          | Cve Identifier | Description                                                                                                                                                                                                                                                                                                                                   |
| ----------- | ------------------------------------------------ | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-18123 | 01100683  01094763                               |                | **Issue**: Forgot password functionality generates a temporary password, but never forces the user to change it. **Resolution**: Generated passwords are now marked as temporary and users are forced to change the password on first login.                                                                                                  |
| RDAPI-18383 | 01147927                                         |                | **Issue**: Session timeout currently configured is absolute timeout of the session, but idle session timeout is also needed for API Gateway Manager. **Resolution**: New environment variable introduced `env.WEBMANAGER.SESSION.IDLE.TIMEOUT` that defines 30 minutes default idle session timeout.                                          |
| RDAPI-19525 | 01115803                                         |                | **Issue**: In API Gateway, a header with an empty value was incorrectly subjected to RFC 7230 "obs-fold" parsing, concatenating it with the next header. **Resolution**: API Gateway now conforms to RFC 7230 and empty headers are left unchanged.                                                                                           |
| RDAPI-19895 | 01155074                                         |                | **Issue**: Proxy authorization appears in product traces. **Resolution**: Authentication information has been removed from traces.                                                                                                                                                                                                            |
| RDAPI-20011 | 01159022  01116092  01160978  01033168  01080239 |                | **Issue**: The request body was being reflected in the response. **Resolution**: Added property `com.axway.apimanager.fault.removeContentBody` that removes the body from the response.                                                                                                                                                       |
| RDAPI-20086 |                                                  |                | **Issue**: Token Information Services allow GET request meaning that `access_token` can be sent as a query string parameter in the URL. This is a security issue as sensitive information should never be included in the URL. **Resolution**: Only a POST request with `access_token` sent in the body of the POST request are now accepted. |

### Other fixed issues

| Internal ID | Case ID                                                                                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ----------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-16183 | 01055370                                                                                 | **Issue**: KPS cache does not include the table name in the cache key, causing collisions for entries with the same column name and value pair. **Resolution**: Include the KPS table name in the cache key.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| RDAPI-18376 | 01141105  01111969                                                                       | **Issue**: A user with the user role encounters a permission error when creating an application. **Resolution**: A user with the user role no longer encounters unexpected errors when creating an application.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-18379 | 01105501                                                                                 | **Issue**: Editing of application sharing details prevents further editing of other application details. **Resolution**: Editing of application details works as expected.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| RDAPI-18876 | 01049408                                                                                 | **Issue**: API Manager actions requiring many writes are too slow when the Cassandra node CPU usage is too high. **Resolution**: Reduced the number of reads when writing new objects into Cassandra.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| RDAPI-19005 | 01131477                                                                                 | **Issue**: Misspelled word in API Manager UI. **Resolution**:  Misspelled word is corrected.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| RDAPI-19119 | 01132142                                                                                 | **Issue**: Cannot create a HTTP traffic monitoring transaction leg record from a script filter or the Java layer. **Resolution**: Missing HTTP monitoring fields have been added to the Java layer.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| RDAPI-19142 | 01136068  01134950  01097471                                                             | **Issue**: When the **Skip Authorization** checkbox in the Authorization Request filter is used, scopes already authorized and stored in the Authorizations DB for this application and user are accepted in addition to the requested scopes. **Resolution**:  When the **Skip Authorization** checkbox in the Authorization Request filter is used, only requested scopes are accepted.                                                                                                                                                                                                                                                                                                                  |
| RDAPI-19190 | 01136100                                                                                 | **Issue**: Running the `update-apimanager` script with the `--fed` option fails for a fed with a missing type. **Resolution**: A flag has been added that checks for the type first before attempting to access it.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| RDAPI-19220 | 01110388                                                                                 | **Issue**: `kpsadmin` tool was unable to delete rows. **Resolution**: `kpsadmin` tool is now able to delete rows.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| RDAPI-19254 | 01138607  01139565  01139664                                                             | **Issue**: Large number of open socket descriptors on Linux 64-bit can cause API Gateway to crash. **Resolution**: API Gateway can now handle a large number of socket descriptors on Linux 64-bit.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| RDAPI-19295 | 01140630                                                                                 | **Issue**: OAuth credentials are always enabled when an application is imported and the original state of the credentials when the application was exported is lost. **Resolution**: OAuth credentials state is preserved when the application is exported.                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| RDAPI-19319 | 01139929                                                                                 | **Issue**: Connect To URL filter treats 304 requests as redirect and performs the request twice. **Resolution**: 304 status code is no longer treated as a redirect.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| RDAPI-19435 | 01139572                                                                                 | **Issue**: The SFTP File Upload filter was not retrieving the correct selector value for the host finger print field. **Resolution**: Updated the filter to retrieve the correct selector value.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| RDAPI-19450 | 01143338  01143298                                                                       | **Issue**: API Gateway documentation incorrectly states that certificates from Java keystore can also be loaded and trusted by setting the `javax.net.ssl.trustStore` Java system property. **Resolution**: The statement has been removed from the *Import certificates and keys to the API Gateway trusted certificate store* section of API Gateway documentation.                                                                                                                                                                                                                                                                                                                                      |
| RDAPI-19459 | 01120523                                                                                 | **Issue**: In Policy Studio, there was a broken link when clicking **Jump to Configuration** on an environmentalized API Manager setting. **Resolution**: Clicking **Jump to Configuration** on an environmentalized API Manager setting correctly redirects to the API Manager settings.                                                                                                                                                                                                                                                                                                                                                                                                                  |
| RDAPI-19463 | 01144528  01137208                                                                       | **Issue**: Query parameters are not validated correctly by API Manager. **Resolution**: Query parameters are now validated correctly.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| RDAPI-19480 | 01121284                                                                                 | **Issue**: Event logging was not writing the required application ID to the service context `client` field when using OAuth. **Resolution**: A new attribute `authentication.application.id` has been added, which you can use to set the client value correctly in the event logs for OAuth. You must also add a Java system property to the `jvm.xml` file in the `conf/` directory of the instance to disable writing username to the service context in the event logs, which is a requirement for Embedded Analytics for API Manager to work correctly. For example: `<ConfigurationFragment>    <VMArg name="-Dcom.axway.coreapi.method.servicecontext.clientattr=true" /></ConfigurationFragment>`. |
| RDAPI-19491 | 01102542                                                                                 | **Issue**: Running `kpsadmin` with the diagnostic option was outputting a 410 GONE error message when attempting to retrieve data that did not exist. **Resolution**: The `kpsadmin` script has been updated to exit when all data is retrieved.                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| RDAPI-19499 | 01151544  01151502  01161250  01142544  01156544                                         | **Issue**: Upgrading a fed file from version 7.6.2 to version 7.7 using Policy Studio is very slow on Windows. **Resolution**: Policy Studio has been updated to reduce upgrade time on Windows.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| RDAPI-19526 | 01142179                                                                                 | **Issue**: In Policy Studio, if a field was environmentalized, a database connection test failed to use the environmentalized value. **Resolution**: If a field is environmentalized, a database connection test uses the environmentalized value.                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| RDAPI-19572 | 01129719  01128960                                                                       | **Issue**: Large numbers of Oauth authorizations (oauth_authorizations table) cause API Manager to become unresponsive. **Resolution**: Support for pagination headers has been implemented in the `api/portal/v1.3/authorizations` API and API Manager UI.                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| RDAPI-19573 | 01156521  01143940                                                                       | **Issue**: Upgrade of Nimbus JWT library to version 8.5 caused the JWT Verify filter to stop working when verifying JWT sets. **Resolution**:  JWT Verify filter is compatible with Nimbus JWT 8.5 and properly verifies JWT sets.                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| RDAPI-19603 | 01142622                                                                                 | **Issue**: When redaction is enabled, the HTTP response stored in Traffic Monitor is truncated after the first "100 Continue" header. **Resolution**: "100 Continue" support has been added to the HTTP redaction layer and fully redacted HTTP responses are stored in Traffic Monitor.                                                                                                                                                                                                                                                                                                                                                                                                                   |
| RDAPI-19691 | 01100006  01130690                                                                       | **Issue**: API Gateway Analytics throws an error when values greater than the limit for a signed integer are returned by the database. **Resolution**: Analytics now handles database response values as long data types.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| RDAPI-19730 | 01041571                                                                                 | **Issue**: User's mobile number is not synchronized when sent in a policy during API Manager master/slave synchronization. **Resolution**: User's mobile number is gathered and saved in slave.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-19753 | 01151768                                                                                 | **Issue**: HTTP response stored by Open Traffic Logger is truncated after first "100 Continue" header. **Resolution**: "100 Continue" support has been added to Open Traffic Logger layer and all headers are correctly stored.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-19757 | 01096753  01111767  01094639                                                             | **Issue**: In Policy Studio or Configuration Studio, the KPS Table Structure view shows black checkboxes for table rows on Windows. **Resolution**: The KPS Table Structure view correctly shows checkboxes for table rows.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| RDAPI-19759 | 01111766                                                                                 | **Issue**: A generic "Invalid Data" error message is displayed upon entering an invalid Security Certificate URL. **Resolution**: A detailed error message is displayed in this scenario.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| RDAPI-19774 | 01152955  01153905  01149611                                                             | **Issue**: In API Gateway requests to a virtual host are confined to the virtual host. Matching is not attempted against the parent listener if not found in the virtual host configured handlers. **Resolution**: Requests to a virtual host are now matched against the parent listener if a handler is not found in the virtual host configuration.                                                                                                                                                                                                                                                                                                                                                     |
| RDAPI-19776 | 01153049                                                                                 | **Issue**: `$INSTALL_DIR/apigateway/posix/bin/jython` script is not provided as part of API Gateway Package and Deploy Tools installer. **Resolution**: `jython` script is now part of the Package and Deployment Tools distribution.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| RDAPI-19791 | 01090929                                                                                 | **Issue**: Failed login attempts to API Manager are not stored in audit log. **Resolution**: Failed login attempts to API Manager are properly logged in audit log.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| RDAPI-19795 | 01136358  01153566                                                                       | **Issue**: Incorrect naming in UI of Base path and Base path URL for values that are not Base Path according to swagger 2.0 and later. **Resolution**: Naming in UI is fixed to comply with Swagger 2.0 and later.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| RDAPI-19803 | 01139463                                                                                 | **Issue**: When all methods of an API have their inbound security profile overridden, an invocation of an non-exiting method returns 500 Authentication Not Configured. This can allow a malicious user to guess the methods of a secure API. **Resolution**: Invoking a non-existing method of an API uses the default API inbound security profile defined in that API to validate the caller.                                                                                                                                                                                                                                                                                                           |
| RDAPI-19812 | 01153995 01142801                                                                        | **Issue**: The API image was not displaying in API Catalog view. **Resolution**: The API image is correctly displayed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| RDAPI-19813 | 01103156                                                                                 | **Issue**: In API Gateway, using the method `getContent` from a `NodeImpl` object results in a crash when content is null. **Resolution**: The method has been fixed to handle null content.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| RDAPI-19855 | 01145864  01148092                                                                       | **Issue**: The kps composite key query endpoint was not decoding query parameters. **Resolution**: The kps composite key query endpoint now decodes query parameters.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| RDAPI-19899 | 01114864  01116592                                                                       | **Issue**: The backend was not validating int query parameters. **Resolution**: The backend now validates int query parameters.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| RDAPI-19926 | 01096567                                                                                 | **Issue**: Content-types in API requests cause incorrect path matching for multiple ambiguous API matches. **Resolution**: Path matching now determines the best path by comparing API matches with and without content-type equally.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| RDAPI-19939 | 01102029  01104054                                                                       | **Issue**: Invocation of Store Message and Restore Message filters might result in an empty message body when the message body content is too large. **Resolution**: The filters correctly process large message body content.                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| RDAPI-19969 | 01155966  01151956  01155475  01155982  01146306  01156860  01156316  01141111  01160432 | **Issue**: In API Gateway, when using a Connect or Connect to URL Filter in a REST API, if an exception occurs in the backend, the API Gateway overrides any fault handler configured in the policy and overwrites the response body. This does not apply for global fault handlers. **Resolution**: API Gateway respects any fault handler configured in the policy.                                                                                                                                                                                                                                                                                                                                      |
| RDAPI-19990 | 01064458  01064765                                                                       | **Issue**: In a Policy Studio project, when error response codes are added to a REST API method, the method cannot be edited after the project is closed and reopened. **Resolution**: The method can be edited as expected after the project is closed and reopened.                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| RDAPI-20015 | 01156482                                                                                 | **Issue**: API Manager configuration can only be updated on a live installation. **Resolution**: Additional options have been added to `update-apimanager` so that a project directory, federated files, or policy and environment files can be updated.                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

## Known issues

The following are known issues for this update.

| Internal ID | Description                                                                                                                                                        |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| RDAPI-20320 | Analytics PDF report generation fails with UnknownContentError on Ansible OS                                                                                       |
| RDAPI-20292 | Special character in API name causes filename issue when saving swagger                                                                                            |
| RDAPI-20234 | `update-apimanager` re-enables the Oauth HTTPS port if disabled                                                                                                    |
| RDAPI-20218 | `apimanager-promote` does not remove API Access from application                                                                                                   |
| RDAPI-20127 | Selector `${content.body.getJSON().get(0)}` not working                                                                                                            |
| RDAPI-20091 | In Policy Studio, when importing a policy fragment, deselected items are imported anyway                                                                           |
| RDAPI-20055 | Sentinel filter - "Use the following tracked object" option not working                                                                                            |
| RDAPI-20017 | Incorrect semantics of negated match types like IS_NOT in Compare Attribute filter                                                                                 |
| RDAPI-19838 | Manager rejects valid OAS documents that use space characters in the keys of an "examples" object                                                                  |
| RDAPI-19833 | Okta SSO integration and email mapping                                                                                                                             |
| RDAPI-19788 | Issue with pagination across multiple sections                                                                                                                     |
| RDAPI-19598 | Session resumption does not work for TLSv1.3 when API Gateway acts as a client                                                                                     |
| RDAPI-19580 | Trial option in the Organization does not work                                                                                                                     |
| RDAPI-19490 | Modifying the email template `$subject` variable does not work                                                                                                     |
| RDAPI-19453 | Uploading an invalid image type when creating an application leads to an internal server error                                                                     |
| RDAPI-19433 | Line breaks in outbound parameter (type header) value not escaped                                                                                                  |
| RDAPI-19418 | `api.error.source` not available in APIManager fault handler                                                                                                       |
| RDAPI-19334 | Access to retired APIs is not removed from other organizations as expected                                                                                         |
| RDAPI-19293 | API Catalog Try It shows only the first security device of a security profile                                                                                      |
| RDAPI-19292 | When an admin user's login name is changed, the user is directed to a blank page                                                                                   |
| RDAPI-19262 | X-Rate-Limit header shows inconsistent values in a multi-node API Manager environment                                                                              |
| RDAPI-19258 | OAS 3.0: Default parameter serialization data seems redundant and bloats API exports                                                                               |
| RDAPI-19240 | Users in "pending approval" state are visible in the Sharing tab                                                                                                   |
| RDAPI-19217 | Inconsistency between Application Developers and Account Settings pages in API Manager                                                                             |
| RDAPI-19216 | Applications in pending approval state cannot be exported, but the UI is unclear                                                                                   |
| RDAPI-19150 | Try It in API Manager only shows first 10 API keys                                                                                                                 |
| RDAPI-19132 | Issue with selection of Retirement date when deprecating API                                                                                                       |
| RDAPI-19126 | API Manager echoes request and headers on "404 Not found"                                                                                                          |
| RDAPI-19006 | Delete API "not found" after changing Application Org                                                                                                              |
| RDAPI-18990 | "Failed to delete undefined" pop-up pops up unexpectedly when attempting to delete application                                                                     |
| RDAPI-18777 | Overriding the quota for an application and then removing the setting causes incorrect behavior                                                                    |
| RDAPI-18674 | Insufficient data validation when importing an Application                                                                                                         |
| RDAPI-18649 | Updated SSO role does not display properly in dev users view                                                                                                       |
| RDAPI-18431 | HTTP 409 Resource already exists in Applications - External Credentials                                                                                            |
| RDAPI-18294 | KPS REST API documentation missing info                                                                                                                            |
| RDAPI-18198 | CORS preflight fails for WSDL based API Manager APIs, and Try-It fails                                                                                             |
| RDAPI-18082 | Regression: Policy Shortcut filters no longer automatically renamed in 7.7                                                                                         |
| RDAPI-17282 | Connector for Salesforce APIs in API Manager does not work or is impossible to configure                                                                           |
| RDAPI-16486 | Changes in the mapper always require a reload in the Execute Data Maps filter and once reloaded then providing values for the required parameters must be repeated |
| RDAPI-15981 | Scopes fields for API Key remain visible even if Application Scopes are disabled                                                                                   |
| RDAPI-11143 | Discrepancy with API retirements dates                                                                                                                             |
| RDAPI-23063 | Policy Studio 2020 May release can open projects from future releases                                             |

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
6. Remove any subdirectories in `INSTALL_DIR/groups/group-id/conf` that do not contain extracted federated entity stores. For example, if a `policy-archives` directory exists, remove it.

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

{{< alert title="Note" color="primary" >}}You must extract the file into a new directory and not into the existing API Gateway installation directory.

You must also remove the files `libeay32.dll` and `ssleay32.dll` if they exist in the directory `INSTALL_DIR/policystudio`. {{< /alert >}}

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

{{< alert title="Note" color="primary" >}}You must extract the file into a new directory and not into the existing API Gateway installation directory.

You must also remove the files `libeay32.dll` and `ssleay32.dll` if they exist in the directory `INSTALL_DIR/configurationstudio`.{{< /alert >}}

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

You can find the latest information and up-to-date user guides at the Axway Documentation portal at <https://docs.axway.com>.

This section describes documentation enhancements and related documentation.

### Documentation enhancements

The latest version of API Gateway, API Manager, and API Portal documentation has been migrated to Markdown format and is available in a [public GitHub repository](https://github.com/Axway/axway-open-docs) to prepare for future collaboration using an open source model. As part of this migration, the documentation has been restructured to help users navigate the content and find the information they are looking for more easily.

Documentation change history is now stored in GitHub. To see details of changes on any page, click the link in the last modified section at the bottom of the page.

### Related documentation

To find all available documents for this product version:

1. Go to <https://docs.axway.com/bundle>.
2. In the left pane Filters list, select your product or product version.

Customers with active support contracts need to log in to access restricted content.

The following reference documents are also available:

* [Supported Platforms](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en) - Lists the different operating systems, databases, browsers, and thick client platforms supported by each Axway product.
* [Interoperability Matrix](https://docs.axway.com/bundle/Axway_Products_InteroperabilityMatrix_allOS_en) - Provides product version and interoperability information for Axway products.

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email [support@axway.com](mailto:support@axway.com) or visit Axway Support at <https://support.axway.com>.

See [Get help with API Gateway](/docs/apim_administration/apigtw_admin/trblshoot_get_help/) for the information that you should be prepared to provide when you contact Axway Support.