---
title: API Gateway and API Manager 7.7 March 2020 Release Notes
linkTitle: API Gateway and API Manager March 2020
weight: 150
date: 2020-03-11T00:00:00.000Z
---

## Summary

API Gateway is available as a software installation or a virtualized deployment in Docker containers. API Manager is a licensed product running on top of API Gateway, and has the same deployment options as API Gateway.

The software installation is available on Linux. For more details on supported platforms for software installation, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

Docker deployment is supported on Linux. For a summary of the system requirements for a Docker deployment, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

## New features and enhancements

No new features and enhancements are available in this update.

<!-- Add the new features here -->

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.

<!-- Use this section to describe any changes in the behavior of the product (as a result of features or fixes), for example, new Java system properties in the jvm.xml file. This section could also be used for any important information that doesn't fit elsewhere. -->

### Filebeat v6.2.2

Filebeat has been updated to use v6.2.2. Before installing this update, you must delete the Filebeat folder  `/apigateway/tools/filebeat-5.2.0`. When using Filebeat, follow the [official Filebeat documentation](https://www.elastic.co/guide/en/beats/filebeat/6.6/index.html).

### OpenJDK JRE

API Gateway and API Manager 7.7 and later support OpenJDK JRE, and this update includes Zulu OpenJDK 1.8 JRE instead of Oracle JRE 1.8.

### OpenSSL and FIPS support

In this update OpenSSL has been upgraded to OpenSSL 1.1.1, as OpenSSL 1.0.2 is EOL.

OpenSSL 1.1.1 does not support FIPS, and running API Gateway in FIPS mode is not supported in this update. OpenSSL 3.0 (when available) will support FIPS, and FIPS support will be available again in a future update.

References to FIPS in the documentation will not be removed, but this does not mean that FIPS is still supported and the references should be ignored.

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

This version of API Gateway and API Manager includes the fixes from all 7.5.3, 7.6.2, and 7.7 service packs or updates released prior to this version. For details of all the service pack fixes included, see corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).

### Fixed security vulnerabilities

There are no fixed security vulnerabilities in this version.

### Other fixed issues

| Internal ID | Case ID                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                |
| ----------- | ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RDAPI-16631 | 01111760  01059684  01134442 | **Issue**: Weak validation of Password Lifetime in password policy configuration of API Gateway Manager. **Resolution**: Validation of Password Lifetime has been improved.                                                                                                                                                                                                                                                |
| RDAPI-18473 | 01113996                     | **Issue**: Customers cannot define `aud` JWT claim as an Additional JWT Claim in Get OAuth Access Token filter. **Resolution**: `aud` JWT claim can be defined in Get OAuth Access Token filter and overwrites the default value set by API Gateway.                                                                                                                                                                       |
| RDAPI-18485 | 01055643  01137377           | **Issue**: Message body is lost in case of OAuth token and refresh token expiry case. **Resolution**: Message body is preserved in case of OAuth token and refresh token expiry.                                                                                                                                                                                                                                           |
| RDAPI-18737 | 01118672                     | **Issue**: In API Manager, when flags **Delegate user management** and **Delegate application management** are turned off, the email address field of a user profile stays enabled and modifying its value leads to a deadlock forcing the user to refresh the browser. **Resolution**: Turning off **Delegate user management** and **Delegate application management** completely disables all fields on a user profile. |
| RDAPI-18774 | 01078776  01109328  01110676 | **Issue**: A change in the behavior of nested relative paths was causing customer policies to fail. **Resolution**: The invocation of policies for nested relative paths in API Gateway has been corrected.                                                                                                                                                                                                                |
| RDAPI-18812 | 01110362                     | **Issue**: Wildcard user name and password database configuration does not work when used for OAuth access token store. **Resolution**: Database connector fixed to support wildcard user name and password when used in OAuth access token store.                                                                                                                                                                         |
| RDAPI-18983 | 01126984                     | **Issue**: Configuring Certificate Chain filter with a large number of CA certificates results in high CPU consumption and high response time. **Resolution**: Certificate Chain filter has been improved to reduce the number of signature calculations performed when validating a certificate chain.                                                                                                                    |
| RDAPI-18999 | 01130847  01131098           | **Issue**: In API Manager, when authenticating a request using multiple security devices in a security profile, an authentication failure in any of the security devices causes the request to be rejected. **Resolution**:  Authentication succeeds when one of the security devices in the security profile successfully authenticates the caller.                                                                       |
| RDAPI-19004 | 01131049                     | **Issue**: External Client ID can be set to null when a new external credential for an application is created and no value for External Client ID is provided. **Resolution**: A random value is generated for External Client ID if no value is provided.                                                                                                                                                                 |
| RDAPI-19019 | 01128662                     | **Issue**: When using the XML to JSON filter and enabling the conversion of number/boolean/null elements to primitives in a policy, the XML into JSON filter treats certain strings incorrectly. **Resolution**: The third party `de.odysseus.staxon` jar has been updated to latest codebase (Version 1.4-AXWAY-1).                                                                                                       |
| RDAPI-19020 | 01121197                     | **Issue**: Application Export DAT file is encrypted if the password field in the UI dialog contains data. Encrypt option is ignored if a password was previously set. **Resolution**: Application Export DAT file is only be encrypted if the encrypt option is set and a password is provided.                                                                                                                            |
| RDAPI-19032 | 01122840  01102901           | **Issue**: The `projpack` utility is extremely slow to process large numbers of projects as it merges the same dependent projects multiple times. **Resolution**: Duplicate dependent projects are removed from the projects to be merged and this reduces the merge time.                                                                                                                                                 |
| RDAPI-19033 | 01109440                     | **Issue**: Validation in UI allows zero and decimal-values, but service is not able to handle them. **Resolution**: Improved validation in UI does not allow zero or decimal values in quota settings.                                                                                                                                                                                                                     |
| RDAPI-19034 | 01109437                     | **Issue**: UI validation of quotas gets stuck in invalid state even when correct values are entered again. **Resolution**: Validation gets correctly triggered on quota changes and behaves as it should.                                                                                                                                                                                                                  |
| RDAPI-19048 | 01080989                     | **Issue**: API Gateway Analytics metrics include the start and end time data point, causing an overlap when combining consecutive time frames. **Resolution**: Reports exclude the end time data point so that consecutive reports' metrics match the combined report totals.                                                                                                                                              |
| RDAPI-19066 | 01114559                     | **Issue**: In API Gateway, calling the function `removeAttribute` on an XML element in a Scripting filter causes the API Gateway to crash. **Resolution**: Using the function `removeAttribute` removes the attribute correctly.                                                                                                                                                                                           |
| RDAPI-19106 | 01119257  01135525           | **Issue**: Front-end organization object information becomes outdated when the name is changed. **Resolution**: Changed organization information is retrieved from the KPS when attempting to update.                                                                                                                                                                                                                      |
| RDAPI-19107 | 01116180                     | **Issue**: In API Gateway, when using an ICAP filter, sending a file larger than the **Maximum Sent Bytes per Transaction** results in a crash. **Resolution**: Sending a file larger than the **Maximum Sent Bytes per Transaction** to an ICAP server stops the transaction and logs that the limit has been reached.                                                                                                    |
| RDAPI-19170 | 01108034                     | **Issue**: Get requests for WSDLs are not validating that the request path contains `?WSDL` before path matching. **Resolution**: Get requests for WSDLs check for `?WSDL` before path matching.                                                                                                                                                                                                                           |
| RDAPI-19176 | 01111271                     | **Issue**: Extract REST Request Attribute incorrectly validating the URI path as a host when attempting to decode the extracted attribute. **Resolution**: Extract REST Request Attribute treats the URI path correctly when decoding the extracted attribute.                                                                                                                                                             |
| RDAPI-19209 | 01114234  01103799           | **Issue**: API Gateway Analytics cannot handle a time range greater than one year. **Resolution**: Analytics has been updated to handle time ranges greater than one year.                                                                                                                                                                                                                                                 |
| RDAPI-19223 | 01139040  01139863           | **Issue**: Export of OAS3 (Swagger 3.0) is not restricted and fails in some cases. **Resolution**: Export of OAS3 (Swagger 3.0) is restricted to back-end APIs created with the same version and does not fail when some attribute values are missing.                                                                                                                                                                     |
| RDAPI-19236 | 01127538                     | **Issue**: Performance issues on Dashboard of API Gateway Manager UI. **Resolution**: Performance of API Gateway Manager Dashboard has been improved by reducing the amount of data sent to the UI.                                                                                                                                                                                                                        |
| RDAPI-19264 | 01121103  01120682           | **Issue**: In API Gateway, when using a Execute External Process filter, the timeout attribute cannot be configured using a selector. **Resolution**: The timeout attribute of the Execute External Process filter now accepts selectors.                                                                                                                                                                                  |
| RDAPI-19424 | 01141965                     | **Issue**: SSO LoginName is not set when using Okta as an Identity Provider, causing a Name field validation error when adding the user to the KPS. **Resolution**: The Name field is now set to the Identity Provider defined user name by default.                                                                                                                                                                       |

## Known issues

The following are known issues for this update.

| Internal ID | Description                                                                                                                                                        |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| RDAPI-11143 | Discrepancy with API retirements dates                                                                                                                             |
| RDAPI-13653 | API Portal incorrect Content-Type for SOAP and empty model schema                                                                                                  |
| RDAPI-14100 | Update tuning recommendations for 'High availability with local storage' config                                                                                    |
| RDAPI-15607 | Cannot access NodeManager after submitting external CA signed certs                                                                                                |
| RDAPI-15669 | Stored XSS in the application's Oauth Redirect URL. Encode OAuth Redirect URLs on output                                                                           |
| RDAPI-15760 | Request headers reflected as response headers                                                                                                                      |
| RDAPI-15981 | Scopes fields for API Key remain visible even if Application Scopes are disabled                                                                                   |
| RDAPI-16486 | Changes in the mapper always require a reload in the Execute Data Maps filter and once reloaded then providing values for the required parameters must be repeated |
| RDAPI-16576 | Duplicate headers returned when calling API Gateway REST API                                                                                                       |
| RDAPI-17395 | API Gateway Analytics - no data in DB during DB unavailability                                                                                                     |
| RDAPI-17431 | Deployment starting twice                                                                                                                                          |
| RDAPI-18082 | Regression: Policy Shortcut filters no longer automatically renamed in 7.7                                                                                         |
| RDAPI-18123 | Forgot password should force password change like first time login                                                                                                 |
| RDAPI-18128 | Trusted Certificates in API Manager configuration-instance restart required if changed                                                                             |
| RDAPI-18198 | CORS preflight fails for WSDL based API Manager APIs, and Try-It fails                                                                                             |
| RDAPI-18294 | KPS REST API documentation missing info                                                                                                                            |
| RDAPI-18376 | Message "you do not have permission to access this resource" when a user creates an application                                                                    |
| RDAPI-18379 | Spurious "forbidden" error in Manager UI                                                                                                                           |
| RDAPI-18469 | API Manager response contains sensitive request headers when calling a non existing path                                                                           |
| RDAPI-18519 | Performance issue with API Manager /applications API in SSO                                                                                                        |
| RDAPI-18649 | Updated SSO role does not display properly in dev users view                                                                                                       |
| RDAPI-18674 | Insufficient data validation when importing an Application                                                                                                         |
| RDAPI-18776 | regex for custom property in API Manager                                                                                                                           |
| RDAPI-18777 | Overriding the quota for an application and then removing the setting causes incorrect behavior                                                                    |
| RDAPI-18823 | 2 way SSL connection to the backend does not work when upgraded to 7.7                                                                                             |
| RDAPI-18876 | Extremely slow Swagger virtualization when Cassandra is under load                                                                                                 |
| RDAPI-19005 | Spelling mistakes or inconsistencies in Manager UI                                                                                                                 |
| RDAPI-19006 | Delete API "not found" after changing Application Org                                                                                                              |
| RDAPI-19028 | Sensitive headers not scrubbed from 405 responses; regression from 7.5.3 SP10                                                                                      |
| RDAPI-19119 | Set attribute in opsdb for hand made transaction                                                                                                                   |
| RDAPI-19126 | API Manager echoes request and headers on "404 Not found"                                                                                                          |
| RDAPI-19132 | Issue with selection of Retirement date when deprecating API                                                                                                       |
| RDAPI-19142 | "Skip authorization" option in Authorization Request filter causes requested scopes to be ignored                                                                  |
| RDAPI-19150 | "Try it" in API Manager only shows first 10 API keys                                                                                                               |
| RDAPI-19190 | update-apimanager fails for customer 7.7 to 7.7SP2                                                                                                                 |
| RDAPI-19220 | kpsadmin - delete row does not delete row                                                                                                                          |
| RDAPI-19240 | Users in "pending approval" state are visible in the Sharing tab                                                                                                   |
| RDAPI-19254 | Regular crashes SIGSEGV / SEGV_MAPERR                                                                                                                              |
| RDAPI-19258 | OAS 3.0: Default parameter serialization data seems redundant and bloats API exports                                                                               |
| RDAPI-19292 | When an API Manager admin user's login name is changed, the user is directed to a blank page                                                                       |
| RDAPI-19295 | Enabled/disabled status of Oauth credentials in an application is lost when exported                                                                               |
| RDAPI-19332 | KPS caching seems to not use the table name as part of the cache-key, resulting in undesired behavior                                                              |
| RDAPI-19334 | Access to retired APIs is not removed from other organizations as expected                                                                                         |
| RDAPI-19418 | api.error.source not available in APIManager fault handler                                                                                                         |
| RDAPI-19433 | Line breaks in outbound parameter (type header) value not escaped                                                                                                  |
| RDAPI-19459 | "Jump to configuration" does nothing for certain environmentalized settings                                                                                        |
| RDAPI-19480 | RDAPI-17007 Not fixed for API Gateway - Event file client wrong                                                                                                    |
| RDAPI-19490 | Modifying the email template $subject variable does not work                                                                                                       |
| RDAPI-19491 | KPS Run Diagnostic Check is failing with error "HTTP 410 Gone"                                                                                                     |
| RDAPI-19525 | Bad obs-fold parsing when API Gateway receives an empty header                                                                                                     |
| RDAPI-19572 | Large numbers of Oauth authorizations (oauth_authorizations table) cause API Manager to become unresponsive                                                        |
| RDAPI-19585 | Okta SSO not working with Microsoft Edge and Internet Explorer                                                                                                     |

## Update a classic (non-container) deployment

These instructions apply to API Gateway and API Manager classic deployments only. For container deployments, see [Update a container deployment](#update-a-container-deployment).

### Prerequisites

This update has the following prerequisites in addition to the [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

1. Shut down any Node Manager or API Gateway instances on your existing installation.
2. Back up your existing installation. For details on backing up, see [API Gateway backup and disaster recovery](/docs/apim_administration/apigtw_admin/manage_operations/#api-gateway-backup-and-disaster-recovery). Ensure that you back up any customized files. You should merge updated files instead of copying them back directly to avoid any regex matching issues. For example, the following directories might contain customized files:

   ```
   webapps/apiportal/vordel/apiportal
   webapps/emc/vordel/manager/app
   webapps/emc
   system/conf/apiportal/email
   system/conf
   samples/scripts/
   tools/filebeat-VERSION-PLATFORM
   ```
3. Remove old third-party libraries by deleting the following directories:

   ```
   INSTALL_DIR/apigateway/system/lib/modules
   INSTALL_DIR/analytics/system/lib/modules
   ```
4. Remove old JRE versions by deleting the following directories:

   ```
   INSTALL_DIR/apigateway/platform/jre
   ```
5. If you have an existing Apache Cassandra installation, ensure that you back up your data (Cassandra and `kpsadmin`), and that the `JAVA_HOME` variable is set correctly in `cassandra.in.sh` and `cassandra.in.bat`.
6. Remove the old Filebeat folder `/apigateway/tools/filebeat-5.2.0`. Check any customized files to see if they are compatible with the new version. See [Filebeat](#filebeat-v6-2-2) for more information.
7. On Linux, remove existing capabilities on product binaries (which might prevent overwriting files):

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
3. Verify the owners of API Gateway binaries before extracting the update.

   ```
   ls -l INSTALL_DIR/apigateway/posix/bin
   ```
4. Using the same user who owns the API Gateway binaries, unzip and extract API Gateway 7.7 server Update over the `apigateway` directory in your existing installation directory . For example:

   ```
   tar -xzvf APIGateway_7.7.YYYYMMDD_Core_linux-x86-64_BNnn.tar.gz -C /opt/Axway-7.7/apigateway/
   ```
5. Change to the `apigateway` directory in your installation.

   ```
   cd INSTALL_DIR/apigateway
   ```
6. Run the post-install script, and ensure that the correct permissions are set:

   ```
   apigw_sp_post_install.sh
   ```

#### Install the Policy Studio update

To install the update on your existing Policy Studio installation, perform the following steps:

1. Shut down Policy Studio.
2. Back up your existing `INSTALL_DIR/policystudio` directory.
3. Remove old JRE versions by deleting the following directories:

   ```
   INSTALL_DIR/policystudio/jre
   ```
4. Remove files `libeay32.dll` and `ssleay32.dll` from directory:

   ```
   INSTALL_DIR/policystudio
   ```
5. Unzip and extract API Gateway 7.7 Policy Studio Update over the `policystudio` directory in your existing API Gateway 7.7 installation directory. For example:

   ```
   tar -xzvf APIGateway_7.7.YYYYMMDD_PolicyStudio_linux-x86-64_BNnn.tar.gz -C /opt/Axway-7.7/policystudio/
   ```
6. Start Policy Studio with `policystudio -clean`

#### Install the Configuration Studio update

To install the update on your existing Configuration Studio installation, perform the following steps:

1. Shut down Configuration Studio.
2. Back up your existing `INSTALL_DIR/configurationstudio` directory.
3. Remove old JRE versions by deleting the following directories:

   ```
   INSTALL_DIR/configurationstudio/jre
   ```
4. Remove files `libeay32.dll` and `ssleay32.dll` from directory:

   ```
   INSTALL_DIR/configurationstudio
   ```
5. Unzip and extract API Gateway 7.7 Configuration Studio Update over the `configurationstudio` directory in your existing API Gateway 7.7 installation directory. For example:

   ```
   tar -xzvf APIGateway_7.7.YYYYMMDD_ConfigurationStudio_linux-x86-64_BNnn.tar.gz -C /opt/Axway-7.7/configurationstudio/
   ```
6. Start Configuration Studio with `configurationstudio  -clean`

#### Install the API Gateway Analytics update

To install the update on your existing API Gateway Analytics 7.7 installation, perform the following steps:

1. Ensure that your existing API Gateway Analytics instance and Node Manager have been stopped.
2. Verify the owners of API Gateway binaries before extracting the update.

   ```
   ls -l INSTALL_DIR/analytics/posix/bin
   ```
3. Using the same user who owns the API Gateway Analytics binaries, unzip and extract API Gateway 7.7 Analytics Update over the `analytics` directory in your existing API Gateway 7.7 installation directory. For example:

   ```
   tar -xzvf APIGateway_7.7.YYYYMMDD_Analytics_linux-x86-64_BNnn.tar.gz -C /opt/Axway-7.7/analytics/
   ```
4. Change to the `analytics` directory in your installation:

   ```
   cd INSTALL_DIR/analytics
   ```
5. Run the post-install script for API Gateway Analytics.

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

<!-- Add a summary of doc changes or enhancements here-->

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
