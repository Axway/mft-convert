---
title: API Portal 7.7 February 2022 Release Notes
linkTitle: API Portal February 2022
weight: 95
date: 2022-01-21
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

### Support for PHP 8.0

We are adding support for PHP 8.0 in API Portal. This is a major update of the PHP language.

### Support for MySql 8.0

We are adding support for MySQL 8.0, but note that you must have PHP 8.0 installed before using MySQL 8.0. For more information, see [Install Apache HTTP server and PHP](/docs/apim_installation/apiportal_install/install_dependencies/)

## Limitations of this update

This update has the following limitations:

* API Portal 7.7.20220228 is compatible with API Gateway and API Manager 7.7.20220228 only.
* To upgrade from earlier versions (for example, 7.5.5, 7.6.2) you must first upgrade to [API Portal 7.7](/docs/apim_relnotes/201904_release/apip_relnotes/) only.
* This update is not available as a virtual appliance or as a managed service on Axway Cloud.

## Important changes

<!-- It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.. -->

There are no major changes in this update.

## Deprecated features

As part of our software development life cycle we constantly review our API Management offering. As part of this update, the following capabilities have been deprecated

### PHP 7.1 and PHP 7.2

PHP 7.1 and PHP 7.2 are no longer actively supported by the PHP, therefore we are deprecating these versions from API Portal in this update, and they will be removed from official support in the next update, in May 2022.

For more information, see [PHP Supported versions](https://www.php.net/supported-versions.php).

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

| Internal ID | Case ID | CVE Identifier | Description                                                                                                                                                            |
| ----------- | ------- | -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-5011    |         |                | **Issue:** Vulnerable version 1.9.3-r0 of lz4 library is used. **Resolution:** lz4 is upgraded to version 1.9.3-r1.                                                            |
| IAP-4891    |         |                | **Issue:** Vulnerable version 0.6.1 of json-pointer vdependency was used. **Resolution:** json-pointer was updated to a version 0.7.0 which is not vulnerable.                 |
| IAP-5068    |         |                | **Issue:** A vulnarable version of golang was used to compile Jobber. **Resolution:** We replace alpine-based golang official base image instead for the "build jobber" layer. |
| IAP-5066    |         |                | **Issue:** API Portal uses marked package version 1.1.1, which was reported as vulnerable. **Resolution:** Marked package was upgraded to a newer non-vulnerable version.      |

### Other fixed issues

| Internal ID | Case ID | Description                                                                                                                                                                                               |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-4646    |         | **Issue:** API Manager HTTP response headers were handled in case-sensitive manner by API Portal. **Resolution:** API Manager HTTP response headers are now handled in case-insensitive manner by the API Portal. |
| IAP-4850    | 1302607 | **Issue:** Аccented characters in application names are viewed as HTML codes. **Resolution:** Аccented character are viewed correctly.                                                                            |
| IAP-4918    | 1317116 | **Issue:** Read only custom properties are erased on submitting the page. **Resolution:** Read only custom properties where preserved on saving the page.                                                         |

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
