---
title: API Portal 7.7 November 2021 Release Notes
linkTitle: API Portal November 2021
weight: 95
date: 2021-09-21
description: API Portal updates are cumulative, comprising new features and changes delivered in previous updates unless specifically indicated otherwise in the Release notes.
---
## Installation

API Portal is available as a software installation or a virtualized deployment in a Docker container. For more information, see the following options:

* If you are installing API Portal for the first time using this update, see [Install API Portal](/docs/apim_installation/apiportal_install/).
* If you are already using API Portal (7.5.x, 7.6.x, 7.7.x) and want to install this update, see [Upgrade API Portal](/docs/apim_installation/apiportal_install/upgrade_automatic/).
* You can use the [cumulative upgrade script](/docs/apim_installation/apiportal_install/upgrade_automatic/#upgrade-api-portal-using-the-cumulative-upgrade-script) to upgrade directly from earlier versions (for example, 7.5.5, 7.6.2) to API Portal [7.7 November](/docs/apim_relnotes/20201130_apip_relnotes/), then apply this update package to update your API Portal to the May 21 release.
* See [API Portal single version upgrade](/docs/apim_installation/apiportal_install/upgrade_automatic/#upgrade-from-api-portal-7-6-2) to upgrade versions incrementally.

### Docker containers

* To deploy API Portal in Docker containers, see [Deploy API Portal in containers](/docs/apim_installation/apiportal_docker/docker_portal_upgrade/).
* If you are already using API Portal in Docker containers and you want to install this update, see [Upgrade API Portal in Docker containers](/docs/apim_installation/apiportal_docker/docker_portal_upgrade/).

## New features and enhancements

The following new features and enhancements are available in this update.

### Centos 8 is replaced by Alpine BaseOS

Because Centos 8 is end of support in 2021, we are now providing API Portal in a Docker container using the Alpine BaseOS instead.

### Improved Docker image

Pagination and Elasticsearch are now enabled in API Docker containers. The pagination capability requires a connection to an Elasticsearch instance. For more information, see [Configure Elasticsearch in API Portal](/docs/apim_installation/apiportal_install/install_software_elastic).

### OAuth client credentials flow improvements

Configuration for [OAuth client credentials flow](/docs/apim_administration/apimgr_admin/api_mgmt_virtualize_web/#oauth) was added in API Manager, so API Portal now supports client credentials flow in line with API Manager adding support.

### New installation parameters

Admin account credential control parameters were added in API Portal installer to allow users to change the JAI admin password and password reset on first login. For more information, see [Unattended installation](/docs/apim_installation/apiportal_install/install_unattended/).

## Limitations of this update

This update has the following limitations:

* API Portal 7.7.20211130 is compatible with API Gateway and API Manager 7.7.20211130 only.
* To upgrade from earlier versions (for example, 7.5.5, 7.6.2) you must first upgrade to [API Portal 7.7](/docs/apim_relnotes/201904_release/apip_relnotes/) only.
* This update is not available as a virtual appliance or as a managed service on Axway Cloud.

## Important changes

<!-- It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.. -->

* API Portal Docker image is now build on top of Alpine BaseOS. The Docker image was previosly built on top of Centos 8.

## Deprecated features

There are no deprecated features in this update.

## End of support notices

There are no end of support notices in this update.

## Removed features

<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, the following features have been removed: -->

No capabilities have been removed in this update.

## Fixed issues

This version of API Portal includes:

* Fixes from all 7.5.5, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID | CVE Identifier | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| ----------- | ------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-4659    |         |                | **Issue**: Axios libraby was reported as vulnerable to Inefficient Regular Expression Complexity CVE-2021-3749. **Resolution**: Axios libraby was upgraded to axios-0.21.4.                                                                                                                                                                                                                                                                                                                     |
| IAP-4605    |         |                | **Issue**: Encryption secret key is kept in the same database where the password it encrypts is. **Resolution**: Remove the secret Key from the database, and move it to filesystem under appropriate access permissions.                                                                                                                                                                                                                                                                       |
| IAP-4606    |         |                | **Issue**: API Portal does not validate the Elasticsearch SSL/TLS certificate to verify that the ES server is in fact "who" it claims to be. As a result, if an attack of type "Man-in-the -Middle" is performed and the ES certificate is changed, API Portal can be tricked to send/receive data to/from a compromised server. **Resolution**: A validation of the Elastic Search SSL/TLS certificate is implemented, and API Portal can now verify the Elastic Search instance authenticity. |

### Other fixed issues

| Internal ID | Case ID            | Description |
| ----------- | ------------------ | - |
| IAP-4289    |                    | **Issue**: Sorting in the Applications page works only on the current page, when API Portal is connected to an Elasticsearch server. **Resolution**: Now sorting works on all pages.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| IAP-4378    | 1262854            | **Issue**: On entering some special symbols like ' (single quote) or &, docker container can't connect to the configured database. Also, API Portal software installer doesn't allow single quotes. **Resolution**: Allow all symbols for database password.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| IAP-4474    | 1265156            | **Issue**: 1. Custom properties permissions not respected when public API mode is enabled. 2. Custom properties permissions are fetched always for the primary user organization role. They should be fetched by role that the user has in the organization that the application/API is assigned to. **Resolution**: 1. Custom properties permissions are always respected. 2. Custom properties permissions are respected per role for the organization that the application is assigned to. Known issue: 1. Custom properties permissions will not be respected per role for the organization the API is assigned to because the fix for that will cause significant performance impact on the try it page. The custom properties permissions will be as per the role of the user's primary organization. 2. On create application, the App has no organization yet, so the custom properties permissions can't be respected per role of the org. They will be as the permissions for the role of the primary org of the user. |
| IAP-4571    | 1259376            | **Issue**: Group mapping causes an issue when organizations and email patterns are added. The issue was caused due to a wrong SQL Query. **Resolution**: The SQL Query is fixed and the email patterns are displaced correctly.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| IAP-4630    | 1287592            | **Issue**: Documentation contained instructions for specific Redis version. **Resolution**: Documentation was updated and all specifics for Redis versions has been removed. For more information, see [Install API Portal](/docs/apim_installation/apiportal_install/install_software)   |
| IAP-4638    |                    | **Issue**: Elasticsearch index is not updated when approving application. As a result, when you approve an application and go to applications catalog, you'll still see the application in pending status until the next run of scheduled indexation cron job execution. **Resolution**: Elasticsearch index is properly updated when approving application from the API Portal.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| IAP-4639    |                    | **Issue**: Newly created application, in pending state, is not immediately visible in the Applications page when API Portal is connected to an Elasticsearch server. **Resolution**: The newly created application is immediately visible in the Applications page, despite of being in pending state, when API Portal is connected to an Elasticsearch server.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| IAP-4651    | 1290127            | **Issue**: Theme Magic in JAI doesn't render styling because of CSP meta tags. **Resolution**: Disable meta tags in Theme Magic page in JAI.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| IAP-4665    | 1284486            | **Issue**: The "Last visit date" is not updated for newly created users until you log out API Portal. **Resolution**: "Last visit date" has been updated for newly created users.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| IAP-4667    |                    | **Issue**: Client secret is set empty when external Oauth is chosen with Authz code flow. **Resolution**: Client secret is now correctly sent to the Authorization Server.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| IAP-4670    | 1295011            | **Issue**: Message for approve/reject user is not shown. **Resolution**: Message for approve/reject user is shown.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| IAP-4680    | 01295149, 01296641 | **Issue**: Only the first OAuth credential redirect URL is respected. **Resolution**: All OAuth credential redirect URL are respected.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| IAP-4681    | 01295149, 01296641 | **Issue**: Unpublishing Authorization code (access code in Swagger 2.0) OAuth flow breaks client credentials (application in Swagger 2.0) flow in API Portal. **Resolution**: User is able to configure both client credentials and authorization code flows, and the configuration of one flow does not affect the other.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| IAP-4684    | 1288655            | **Issue**: Request body is formatted before being sent to API Manager. **Resolution**: Request body is not formatted before being sent to API Manager.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| IAP-4698    | 1283785            | **Issue**: Unable to set translation to the "Create Application" modal dialog when having multiple API managers set. **Resolution**: Translation keys added to com\_apiportal.ini file: \*COM\_APIPORTAL\_APPLICATION\_CREATE\_CHOOSE\_MANAGER\_LABEL for modal title \*COM\_APIPORTAL\_APPLICATION\_CREATE\_DIALOG\_PRIMARY\_ACTION\_LABEL for submit button caption                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| IAP-4839    | 1303483            | **Issue**: Whitelist field was restricted to 250 characters and it prevented adding more than 10 hosts. **Resolution**: Whitelist character limit is removed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| IAP-4848    |                    | **Issue**: API Catalog sorting is incorrect when Elasticseacrh is enabled because of an issue with the default index settings and fields mapping. **Resolution**: A case insensitive normalizer was added to the index mapping, and the sorting will work correctly after deleting the index and executing the indexation of APIs again. On the first trigger of "index APIs job" (either automated or manual) the index will be created with proper mapping and the normalizer will be applied for the sortable fields.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

## Known issues

The following are known issues for this update.

### Custom properties permissions per role are not respected in some scenarios

Custom properties permissions per role will not be respected for the organization in the following scenarios:

* Permissions will not be respected for the organization the API is assigned to because respecting them will cause significant performance impact on the Try-it page. The custom properties permissions will follow the role of the user's primary organization.
* On creating an application, the App has no organization yet, so the custom properties permissions cannot be respected per role of the organization. They will follow the permissions for the role of the primary organization of the user.

Related Issue: IAP-4474

### When Multi Manager feature is configured, API Portal users are no longer able to login

After a recent bug fix in API Manager (RDAPI-20021), the `Authenticate to Master` policy is no longer working after upgrading from releases earlier than [API Portal July 2020](/docs/apim_relnotes/20200730_apip_relnotes/). To fix this, perform the following steps:

1. Open all slave managers configurations in Policy Studio, and click to **Edit** the `AuthenticateToMaster` policy.
2. Click the **Login to Master** (Connect to URL) filter, and enter `Accept: */*` for the **Request Protocol Header**.
3. Click the **Enter** key twice to create two blank lines after `Accept: */*`.

Alternatively you can take the updated `AuthenticateToMaster` policy and apply again your configurations.

Related issue: IAP-3435

### Page layout and alignment for Arabic language

Changing the API Portal language to Arabic (or any other right to left language) results in issues with page layout and alignment on the API Portal Home and Pricing pages, and some buttons are not visible. As a workaround, you can turn on the development mode in JAI. Follow these steps:

1. Log in to Joomla! Admin Interface (JAI).
2. In the JAI top navigation bar, click **Extensions > Templates**.
3. Click your template style (for example, `purity_III * Default`) to open it.
4. Click the **General** tab.
5. Change **Development Mode** to `ON`.
6. Click **Save** and click **Close** to close the template style.

Related issue: IAP-308

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
