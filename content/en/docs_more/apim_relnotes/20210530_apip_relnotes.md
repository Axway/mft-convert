---
title: API Portal 7.7 May 2021 Release Notes
linkTitle: API Portal May 2021
weight: 95
date: 2021-04-26
description: API Portal updates are cumulative, comprising new features and
  changes delivered in previous updates unless specifically indicated otherwise
  in the Release notes.
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

There are no new feature or enhancements in this update. This is a maintenance update consisting of defect fixes.

For a complete list of fixed issues, see [Fixed issues](#fixed-issues) section.

## Limitations of this update

This update has the following limitations:

* API Portal 7.7.20210530 is compatible with API Gateway and API Manager 7.7.20210530 only.
* To upgrade from earlier versions (for example, 7.5.5, 7.6.2) you must first upgrade to [API Portal 7.7](/docs/apim_relnotes/201904_release/apip_relnotes/) only.
* This update is not available as a virtual appliance or as a managed service on Axway Cloud.

## Important changes

<!-- It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.. -->

There are no major changes in this update.

## Deprecated features

There are no deprecated features in this update.

## Removed features

<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, the following features have been removed: -->

No capabilities have been removed in this update.

## Fixed issues

This version of API Portal includes:

* Fixes from all 7.5.5, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID | CVE Identifier | Description                                                                                                                                                               |
| ----------- | ------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-4106    |         | CVE-2020-7760  | **Issue**: Joomla 3.9.25 is using vulnerable version of codemirror@5.56.0. **Resolution**: Joomla@3.9.26 has updated codemirror version to 5.60.0.                        |
| IAP-4312    |         |                | **Issue**: Docker image was using vulnerable openssl-libs package. **Resolution**: openssl-libs package version has been upgraded to a safer version.                            |
| IAP-4051    |         |                | **Issue**: API Portal is having transitive dependency of vulnerable version of lodash. **Resolution**: All dependencies of lodash are now upgraded.                       |
| IAP-4323    |         |                | **Issue**: Docker image was using vulnerable Nettle package. **Resolution**: Nettle package has been upgraded to a non-vulnerable version.                                |
| IAP-4315    |         |                | **Issue**: Docker image was using vulnerable RPM package. **Resolution**: RPM package has been upgraded to a non-vulnerable version.                                      |
| IAP-4335    |         |                | **Issue**: Vulnerable library is found in API Portal docker image - bind-export-libs@9.11.20-5.el8_3.1. **Resolution**: bind-export-libs has been removed from the image. |

### Other fixed issues

| Internal ID | Case ID | Description     |
| ----------- | ------- | ---------------- |
| IAP-4062    | 1240639 | **Issue**: When API Portal is uninstalled, its Database was also uninstalled. **Resolution**: An option for uninstalling the Database was added, the default value is true (Database will be removed) when API Portal is removed.                          |
| IAP-4076    | 1231091 | **Issue**: 1) The way API manager and API Portal handle RegEx in custom properties is inconsistent: API Manager does not support slashes, but API Portal supports them. 2) The custom required field is not enforced in the API Portal - missing implementation. **Resolution**: 1) Custom properties RegEx ID is handled in a consistent way between the products now. 2) The custom required property is now implemented to be enforced in API Portal. |
| IAP-4286    | 1240482 | **Issue**: SQL errors during install/upgrade were not logged. **Resolution**: Add `database-error` category for logging during install/upgrade.                                                                                                                                                                                                                                                                                                        |
| IAP-4288    | 1253710 | **Issue**: Users are not able to configure the redirect destination in case of unauthorized request. **Resolution**: A new field is added in JAI for redirect on unauthorized request.                                                                                                                                                                                                                                                                   |
| IAP-4298    | 1255208 | **Issue**:  It's not possible to configure CSP from API Portal, and the documentation described how to do it using server configuration, which did not work for inline scripts. **Resolution**: A proper Content Security Policy is configured in API Portal with ability to be disabled and edited from JAI; and the documentation is updated.                                                                                                          |
| IAP-4300    | 1257209 | **Issue**: Error message of custom property is not translatable. **Resolution**: Error message of custom property can be translated.     |
| IAP-4329    | 01261309 | **Issue**: `apiportal_db_pass_encryption.sh` can't finish execution because of not supported message digest hashing algorithm. **Resolution**: The "message digest" option is removed from the script because it is not needed.     |

## Known issues

The following are known issues for this update.

### When Multi Manager feature is configured, API Portal users are no longer able to login

After a recent bug fix in API Manager (RDAPI-20021), the `Authenticate to Master` policy is no longer working after upgrading from releases earlier than [API Portal July 2020](/docs/apim_relnotes/20200730_apip_relnotes/). To fix this, perform the following steps:

1. Open all slave managers configurations in Policy Studio, and click to **Edit** the `AuthenticateToMaster` policy.
2. Click the **Login to Master** (Connect to URL) filter, and enter `Accept: */*` for the **Request Protocol Header**.
3. Click the **Enter** key twice to create two blank lines after `Accept: */*`.

Alternatively you can take the updated `AuthenticateToMaster` policy and apply again your configurations.

Related Issue:IAP-3435

### Page layout and alignment for Arabic language

Changing the API Portal language to Arabic (or any other right to left language) results in issues with page layout and alignment on the API Portal Home and Pricing pages, and some buttons are not visible. As a workaround, you can turn on the development mode in JAI. Follow these steps:

1. Log in to Joomla! Admin Interface (JAI).
2. In the JAI top navigation bar, click **Extensions > Templates**.
3. Click your template style (for example, `purity_III * Default`) to open it.
4. Click the **General** tab.
5. Change **Development Mode** to `ON`.
6. Click **Save** and click **Close** to close the template style.

Related Issue: IAP-308

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
