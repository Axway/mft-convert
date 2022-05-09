---
title: API Portal 7.7 July 2020 Release Notes
linkTitle: API Portal July 2020
weight: 130
date: 2020-06-09T00:00:00.000Z
---
## Summary

API Portal provides an API consumer-facing interface that you can customize to match your corporate brand. API Portal is a layered product linked to API Manager, and requires both API Manager and API Gateway. For more information, see the API Gateway and API Manager documentation.

## Installation

API Portal is available as a software installation or a virtualized deployment in Docker containers. For more information, see:

* If you are installing API Portal for the first time using this update, see [Install API Portal](/docs/apim_installation/apiportal_install/).
* If you are already using API Portal (7.5.x, 7.6.x, 7.7.x) and want to install this update, see [Upgrade API Portal](/docs/apim_installation/apiportal_install/upgrade_automatic/).
* You can use the [cumulative upgrade script](/docs/apim_installation/apiportal_install/upgrade_automatic/#upgrade-api-portal-using-the-cumulative-upgrade-script) to upgrade directly from earlier versions (for example, 7.5.5, 7.6.2) to API Portal 7.7 July.
* See [API Portal single version upgrade](/docs/apim_installation/apiportal_install/upgrade_automatic/#upgrade-from-api-portal-7-6-2) to upgrade versions incrementally.

API Portal in Docker containers is available for production environment from [API Portal 7.7 November 2020](/docs/apim_relnotes/20201130_apip_relnotes/) update. For more information, see [Production-ready Docker container](/docs/apim_relnotes/20201130_apip_relnotes/#production-ready-docker-container).

*This section was updated to amend the information about API Portal in Docker containers.*

## New features and enhancements

### Non-root installer​

To expand compliance with many customers internal security guidelines, which discourage or disallow root access, we have added the ability to install and run API Portal using `sudo` commands.​ To learn more, watch [How to install API Portal without being a root user](https://www.youtube.com/watch?v=H5RZjP9Zl7k&list=PLSlCpG9zsECpo8-JMZ2Cx4REDyUvpwy9v)​.

### CentOS 8​ support

We have expanded official support to include CentOS 8 for the standalone (non-docker) installation of API Portal.

Supporting CentOS8 in a docker container is not yet supported.​

### UI/UX improvements

User interface (UI) and user experience (UX) improvements in this release:

* Better filtering options available to **User** roles on the applications catalog to allow easier searching and management of large catalogs​.
* New API state indicators for *Published* and *Unpublished* to make the current status of the endpoint clearer.
* Improved visibility of *Deprecated* APIs​.

### Cumulative upgrade script

This update is shipped with a [cumulative upgrade script](/docs/apim_installation/apiportal_install/upgrade_automatic/#upgrade-api-portal-using-the-cumulative-upgrade-script) to help customers to upgrade from 7.5.5 and 7.6.2 to the 7.7 July 2020​ update.

<!-- Add the new features here -->

## Limitations of this update

This update has the following limitations:

* API Portal 7.7.20200730 is compatible with API Gateway and API Manager 7.7.20200730 only.
* Upgrade to API Portal 7.7.20200730 is supported from [API Portal 7.7](/docs/apim_relnotes/201904_release/apip_relnotes/).
* This update is not available as a virtual appliance or as a managed service on Axway Cloud.

*This section was updated to remove the information about API Portal in Docker containers.*

## Deprecated features

<!-- Add features that are deprecated here -->

As part of our software development life cycle we constantly review our API Management offering. As part of this review, no capabilities have been deprecated.

## Removed features

<!-- Add features that are removed here -->

To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, no capabilities have been removed.

## Fixed issues

This version of API Portal includes:

* Fixes from all 7.5.5, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID | CVE Identifier | Description                                                                                                                                                                                                                                                                                                                                                                   |
| ----------- | ------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-1934    |         |                | **Issue**: Servers should be synchronize to an internal or external NTP server in order to correlate logs and data from different internal and external systems. **Resolution**: Documentation was updated with this best practice. See [Utilize synchronized time source](/docs/apim_installation/apiportal_install/secure_harden_portal/#utilize-synchronized-time-source). |
| IAP-1929    |         |                | **Issue**: User sessions were not revoked on application level even when the user was active. **Resolution**: Added a configuration option in JAI to invalidate all user sessions with the application after defined time period.                                                                                                                                             |
| IAP-3315    |         |                | **Issue**: Auto completion in JAI login was enable and this is against best practice in security. **Resolution**: Disable auto completion in JAI login.                                                                                                                                                                                                                       |
| IAP-1732    |         |                | **Issue**: API Portal HTTPS request to API Manager REST API wasn't protected with certificate verification option. **Resolution**: As per Security practice we have introduced mutual authentication between API Portal and API Manager.                                                                                                                                      |
| IAP-3324    |         |                | **Issue**: `Swagger-ui` is using vulnerable third-party dependency - `remarkable@1.7.4`. **Resolution**: API Portal is now using `Swagger-ui` with upgraded version of this package which is no longer vulnerable - `remarkable@2.0.1` and respectively `swagger-ui@3.26.0`.                                                                                                  |
| IAP-3282    | 1157947 |                | **Issue**: There is no recommendation in the documentation that `Referrer-Policy` header should be set with proper policy. **Resolution**: Documentation was updated. See [Configure Apache section](/docs/apim_installation/apiportal_install/secure_harden_portal/#configure-apache) for recommendation on how to set this policy.       |
| IAP-3283    | 1157947 |                | **Issue**: There is no recommendation in the documentation how to set `SameSite` cookie attribute. **Resolution**: Documentation was updated. See [Configure Apache section](/docs/apim_installation/apiportal_install/secure_harden_portal/#update-apiportal-conf) for recommendation on how to set this attribute.                                                          |

### Other fixed issues

| Internal ID | Case ID | Description                                                                                                                                                                                                                                                                                                                                                    |
| ----------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-3086    |         | **Issue**: While changing the password, if wrong current password is filled out an empty page is displayed. **Resolution**: When wrong current password is filled out, the user sees the error message along with the edit user form.                                                                                                                          |
| IAP-3313    |         | **Issue**: When trying an API and the response is 500, that response code is not shown. **Resolution**: The response code was shown.                                                                                                                                                                                                                           |
| IAP-3331    |         | **Issue**: DB password encryption script is not executable.  **Resolution**: Proper permission were given to the script so it could be executed.                                                                                                                                                                                                               |
| IAP-3333    |         | **Issue**: The passphrase for the encryption of the database password is not respected while installing API Portal via unattended mode. **Resolution**: The passphrase is now respected if it is provided at unattended mode installation.                                                                                                                     |
| IAP-3368    | 1165789 | **Issue**: While performing Try-It requests API Portal sends Cookie header which is not needed in API Manager. **Resolution**: Cookie header is not longer sent along with the requests.                                                                                                                                                                       |
| IAP-3371    |         | **Issue**: Global Configuration in JAI cannot be saved when MySQL password encryption or MySQL SSL modes are enabled. **Resolution**: Global Configuration persists with no errors.                                                                                                                                                                            |
| IAP-3387    |         | **Issue**: Global Configuration in JAI cannot be saved when encryption key for database cache is not generated. **Resolution**: Cache is not invalidated on save when encryption key is not generated.                                                                                                                                                         |
| IAP-3404    | 1172998 | **Issue**: An error is shown on API Try it page when the default language of API Portal is changed. **Resolution**: API Try it page is successfully loaded when the default language of API Portal is changed.                                                                                                                                                 |
| IAP-3416    | 1174950 | **Issue**: The values of the custom properties are not displayed for SOAP APIs. **Resolution**: As we can not get these values, we have hidden the labels. This is noted in the documentation, see [API Manager custom properties support](/docs/apim_administration/apiportal_admin/customize_page_content/#api-manager-custom-properties-support). |
| IAP-3419    |         | **Issue**: API Portal text is shown in the header after the logo image when upgrading from version 7.6.2. **Resolution**: There's no API Portal text in the header after the logo image when upgrading from version 7.6.2.                                                                                                                                     |

## Known issues

The following are known issues for this update.

### When Multi Manager feature is configured, API Portal users are no longer able to login

After a recent bug fix in API Manager (RDAPI-20021), the `Authenticate to Master` policy is no longer working. To fix this, perform the following steps:

1. Open all slave managers configurations in Policy Studio, and click to **Edit** the `AuthenticateToMaster` policy.
2. Click the **Login to Master** (Connect to URL) filter, and enter `Accept: */*` for the **Request Protocol Header**.
3. Click the **Enter** key twice to create two blank lines after `Accept: */*`.

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