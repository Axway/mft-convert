---
title: API Portal 7.7 January 2021 Release Notes
linkTitle: API Portal January 2021
weight: 100
date: 2021-01-30
description: API Portal updates are cumulative, comprising new features and changes delivered in previous updates, unless specifically indicated otherwise in the Release notes.
---

## Installation

API Portal is available as a software installation or a virtualized deployment in a Docker container. For more information, see the following options:

* If you are installing API Portal for the first time using this update, see [Install API Portal](/docs/apim_installation/apiportal_install/).
* If you are already using API Portal (7.5.x, 7.6.x, 7.7.x) and want to install this update, see [Upgrade API Portal](/docs/apim_installation/apiportal_install/upgrade_automatic/).
* You can use the [cumulative upgrade script](/docs/apim_installation/apiportal_install/upgrade_automatic/#upgrade-api-portal-using-the-cumulative-upgrade-script) to upgrade directly from earlier versions (for example, 7.5.5, 7.6.2) to API Portal [7.7 November](/docs/apim_relnotes/20201130_apip_relnotes/), then apply this package to update your API Portal to the January 21 release.
* See [API Portal single version upgrade](/docs/apim_installation/apiportal_install/upgrade_automatic/#upgrade-from-api-portal-7-6-2) to upgrade versions incrementally.
* To deploy API Portal in Docker containers, see [Deploy API Portal in containers](/docs/apim_installation/apiportal_docker/docker_portal_upgrade/).
* If you are already using API Portal in Docker containers and you want to install this update, see [Upgrade API Portal in Docker containers](/docs/apim_installation/apiportal_docker/docker_portal_upgrade/).

## New features and enhancements

### Docker container improvements

This update includes a sample docker-compose.yml script, which once configured for your environment, enables an easier start up of API Portal, MariaDB, and Redis containers. Now, you can build your own API Portal docker container using the same build scripts that Axway use internally.

### Simplified language file upload

Previously, to upload language files you had to connect to the API Portal server, download the English translation files, translate them, rename them correctly, and upload them to the correct directory on the server. To simplify this process, we have added an upload capability to the Joomla! Admin Interface (JAI). For more information, see [Add a translated UI string file](/docs/apim_administration/apiportal_admin/localize_language/#add-a-translated-ui-string-file).

### Security improvements

* Improved readability of all logs redirected to stdout.
* Added support for the anti-virus service (ClamAV) running on a remote machine. Previously, it had to be running on the same server as API Portal.

### UI modernization improvements

* New two-column layout available for API details view.

  ![Two-column layout](/Images/docbook/images/release_notes/dual-pane-layout-jan21.png)

* Swagger UI layout configuration available from [API Catalog settings](/docs/apim_administration/apiportal_admin/customize_apicatalog_overview/).

  ![API Catalog settings](/Images/docbook/images/release_notes/dual-pane-layout-config-jan21.png)

* New configuration to hide the **Try-it** button for APIs with specific tags, for example, "Coming soon". Wildcards such as `?` and `*` are supported.

  ![Hide Try-it](/Images/docbook/images/release_notes/hide-try-it-for-specific-tags.png)

* User account fields (Name, Login name, Password, Email) are now read-only in JAI, as this data is managed from API Manager or an external IDP.
* Custom properties are now displayed in Public mode.
* A consistent loading icon (spinner) is now rendered across the API details, Applications, and Usage tabs.

## Limitations of this update

This update has the following limitations:

* API Portal 7.7.20210130 is compatible with API Gateway and API Manager 7.7.20210130 only.
* To upgrade from earlier versions (for example, 7.5.5, 7.6.2) you must first upgrade to [API Portal 7.7](/docs/apim_relnotes/201904_release/apip_relnotes/) only.
* This update is not available as a virtual appliance or as a managed service on Axway Cloud.

## Important changes

There are no major changes in this update.

## Deprecated features

No capabilities have been deprecated in this update.

## Removed features

No capabilities have been removed in this update.

<!-- To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, the following features have been removed: -->

## Fixed issues

This version of API Portal includes:

* Fixes from all 7.5.5, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| **Internal ID** | **Case ID** | **CVE Identifier** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                    |
| --------------- | ----------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| IAP-3173        |             |                    | **Issue**: `HTTP OPTIONS` method is enabled by default. **Resolution**: The documentation for "[Allow requests from only used HTTP methods](/docs/apim_installation/apiportal_install/secure_harden_portal/#allow-requests-from-only-used-http-methods)" was updated with instruction how to disable `HTTP OPTIONS` method.                                                                                        |
| IAP-3174        |             |                    | **Issue**: The HTTP response contains duplicate attributes for `Set-Cookie header` and `X-Frame-Options header` was shown several times. **Resolution**: Duplication of the `X-Frame-Options header` and of the cookie attributes are prevented now. The documentation for "[Update apiportal.conf](/docs/apim_installation/apiportal_install/secure_harden_portal/#update-apiportalconf)" is updated accordingly. |
| IAP-3784        | 1215016     |                    | **Issue**: Arbitrary redirection on sign-in page was exploitable. **Resolution**: The issue has been fixed as now API Portal checks if the host of the encoded return url is the same as the API Portal's host.                                                                                                                                                                                                    |
| IAP-3757        | 1212990     |                    | **Issue**: Sensitive directories accessible through web. **Resolution**: htaccess file modification done to block sensitive directory access.                                                                                                                                                                                                                                                                      |
| IAP-3745        | 1212603     |                    | **Issue**: XSS was possible on API Details page while creating an application. **Resolution**: The XSS is remediated on API Details page for `apiId` parameter.                                                                                                                                                                                                                                                    |
| IAP-3846        |             |                    | **Issue**: Private key file is publicly available via URL. **Resolution**: `htaccess` modification done to block sensitive data access. See the [API Portal private key accessible through web](https://support.axway.com/en/articles/article-details/id/181367) article to learn how to manually apply the fix.                                                                                                   |

### Other fixed issues

| **Internal ID** | **Case ID** | **Description**                                                                                                                                                                                                                                                                  |
| --------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-3732        |             | **Issue**: Informative messages where not logged in a log file when password was reset. **Resolution**: Informative messages where not logged in a `com_apiportal.error.log` file.                                                                                               |
| IAP-3776        |             | **Issue**: JavaScript error is shown in console when submitting login form. **Resolution**: `components/com_apiportal/assets/js/apiportal.js` loaded in login page.                                                                                                              |
| IAP-3887        | 1220931     | **Issue**: Installation fails when installed with MySQL lower than 5.7. **Resolution**: `UTF8 COLLATE` was added to docker file in order to support MySQL lower than 5.7.                                                                                                        |
| IAP-3889        | 1221136     | **Issue**: The documentation describing secure PHP configuration have mistakes and not valid options. **Resolution**: The documentation has been updated following best security practices applicable to API Portal.                                                             |
| IAP-3911        |             | **Issue**: Apache virtual host configuration was not correctly set when installing API Portal using plain HTTP and as a result API Portal cannot be loaded. **Resolution**: Now Apache virtual host configuration is correct and API Portal is able to install using plain HTTP. |
| IAP-3875        | 1203778     | **Issue**: IPs are logged hashed when session hijack attempt happens. **Resolution**: IPs will be logged without being hashed when session hijack attempt happens.                                                                                                               |

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
