---
title: API Portal 7.7 August 2021 Release Notes
linkTitle: API Portal August 2021
weight: 95
date: 2021-06-21
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

### Pagination support for API and Applications catalogs

The API and Applications catalogs now support pagination controls to allow a better experience with catalog management and to improve performance. The pagination capability requires a connection to an Elasticsearch instance. For more information, see [Configure Elasticsearch in API Portal](/docs/apim_installation/apiportal_install/install_software_elastic).

{{< alert title="Note" color="primary" >}}Pagination is only available for software installation of API Portal. This is not available for API Portals deployed in containers.{{< /alert >}}

### Database password encryption improvements

The passphrase for database password encryption is stored encrypted.

Now, the passphrase typed in the command line is masked. Then, the user is asked to verify the passphrase (type the same passphrase to confirm), and if the re-type of the passphrase does not match the passphrase typed, the command line displays an error message.

## Limitations of this update

This update has the following limitations:

* API Portal 7.7.20210830 is compatible with API Gateway and API Manager 7.7.20210830 only.
* To upgrade from earlier versions (for example, 7.5.5, 7.6.2) you must first upgrade to [API Portal 7.7](/docs/apim_relnotes/201904_release/apip_relnotes/) only.
* This update is not available as a virtual appliance or as a managed service on Axway Cloud.

## Important changes

<!-- It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.. -->

There are no major changes in this update.

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

| Internal ID | Case ID | CVE Identifier | Description                                                                                                                                                                                                                                         |
| ----------- | ------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-4553    |         |                | **Issue**: API Portal was using older version of Daterangepicker library. **Resolution**: Daterangepicker was upgraded to v3.1.0.                                                                            |
| IAP-3176    |         |                | **Issue**: The maximum number of unicode characters permitted in passwords was 33. **Resolution**: Passwords now can have 64 or more unicode characters.                                                                                                    |
| IAP-4552    | 1280382 |                | **Issue**: When CSP is disabled a reflected XSS is possible in API Details. **Resolution**: The reflected XSS vulnerability has been successfully remediated using proper sanitisation.                                                                     |
| IAP-4513    | 1275480 | CVE-2021-26034 | **Issue**: API Portal May release was released with Joomla version 3.9.26, which was [found to be vulnerable](https://www.joomla.org/announcements/release-news/5836-joomla-3-9-27.html). **Resolution**: Joomla was updated to version 3.9.29. |
| IAP-4569    | 1284337 |                | **Issue**: API Portal contains deprecated code, which uses deprecated PHP functions. **Resolution**: Even though this code wasn't being used, it uses deprecated security PHP functions. Therefore, it is removed from the code base now.                    |

### Other fixed issues

| Internal ID | Case ID | Description                                                                                                                                                                                                                                                                                                                                         |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-3883    |         | **Issue**: When a user is added to a new organization while still logged in API Portal, the Applications Catalog page shows an empty organization column for the applications of the newly-added organization. **Resolution**: When elastic search is enabled, the organization name will always be shown for each application from the Applications Catalog page. |
| IAP-4297    | 1233608 | **Issue**: When the Swagger definition has properties, which are empty objects, API Portal transformed them to empty arrays, which invalidated the definition and displayed alert messages. **Resolution**: API Portal respects the definition as it comes and does not modify it.                                                                          |
| IAP-4325    |         | **Issue**: Multi-column Swagger UI layout navigation is not working when the method name includes slash character. **Resolution**: Multi-column Swagger UI layout navigation is working properly when the method name includes slash character.                                                                                                             |
| IAP-4383    | 1264453 | **Issue**: An error is shown on API details page when API Portal tag is used as "Show APIs with tags" value. **Resolution**: API Portal tags can be used as "Show APIs with tags" value.                                                                                                                                                                    |
| IAP-4477    | 1253753 | **Issue**: Values of custom properties of type `switch` are not shown on View User profile page. **Resolution**: Custom properties of type `switch` are properly shown on View User profile page.                                                                                                                                                           |
| IAP-4524    | 1273554 | **Issue**: API Portal was adding a slash in the Origin header value when an API endpoint is executed. **Resolution**: No slash is added in the Origin header value when an API endpoint is executed.                                                                                                                                                        |

## Known issues

The following are known issues for this update.

### Elasticsearch datastore is not updated immediately after approving applications in API Portal

For API Portals connected to an Elasticsearch server, the Elastic index is not automatically updated after an application is approved. As a result, the application is listed in *Pending* state in the applications catalog until the next execution of scheduled indexation cron job.

You can perform the following steps to manually trigger the indexation of the applications:

1. In JAI, click **Components > API Portal > Elasticsearch Settings**.
2. Click **Index Applications now**.

For more information, see [Configure Elasticsearch in API Portal](/docs/apim_installation/apiportal_install/install_software_elastic).

Related issue: IAP-4638

### Application is not immediately visible in the applications catalog when it is created and it is in pending state

For API Portals connected to an Elasticsearch server, the Elastic index is not automatically updated after an application is created in *Pending* state. As a result, the application is not listed in the applications catalog until the next execution of scheduled indexation cron job.

You can perform the following steps to manually trigger the indexation of the applications:

1. In JAI, click **Components > API Portal > Elasticsearch Settings**.
2. Click **Index Applications now**.

For more information, see [Configure Elasticsearch in API Portal](/docs/apim_installation/apiportal_install/install_software_elastic).

Related issue: IAP-4639

### Sorting the Applications catalog works only on the current page

API Portals connected to an Elasticsearch server have pagination enabled in the Applications catalog. Sorting the list of applications works only on the current page. If you go to the next or previous page, the sorting is lost and you have to apply it again every time you navigate to another page.

Related Issue: IAP-4289

### When Multi Manager feature is configured, API Portal users are no longer able to login

After a recent bug fix in API Manager (RDAPI-20021), the `Authenticate to Master` policy is no longer working after upgrading from releases earlier than [API Portal July 2020](/docs/apim_relnotes/20200730_apip_relnotes/). To fix this, perform the following steps:

1. Open all slave managers configurations in Policy Studio, and click to **Edit** the `AuthenticateToMaster` policy.
2. Click the **Login to Master** (Connect to URL) filter, and enter `Accept: */*` for the **Request Protocol Header**.
3. Click the **Enter** key twice to create two blank lines after `Accept: */*`.

Alternatively you can take the updated `AuthenticateToMaster` policy and apply again your configurations.

Related issue:IAP-3435

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
