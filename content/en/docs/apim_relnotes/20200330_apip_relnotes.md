---
title: API Portal 7.7 March 2020 Release Notes
linkTitle: API Portal March 2020
weight: 150
date: 2020-03-11T00:00:00.000Z
---

## Summary

API Portal provides an API consumer-facing interface that you can customize to match your corporate brand. API Portal is a layered product linked to API Manager, and requires both API Manager and API Gateway. For more information, see the API Gateway and API Manager documentation.

## Installation

API Portal is available as a software installation or a virtualized deployment in Docker containers. For more information, see:

* If you are installing API Portal for the first time using this update, see [Install API Portal](/docs/apim_installation/apiportal_install/).
* If you are already using API Portal (7.5.x, 7.6.x, 7.7.x) and want to install this update, see [Upgrade API Portal](/docs/apim_installation/apiportal_install/upgrade_automatic/).

API Portal in Docker containers is available for production environment from [API Portal 7.7 November 2020](/docs/apim_relnotes/20201130_apip_relnotes/) update. For more information, see [Production-ready Docker container](/docs/apim_relnotes/20201130_apip_relnotes/#production-ready-docker-container).

*This section was updated to amend the information about API Portal in Docker containers.*

## New features and enhancements

The following new features and enhancements are available in this update.

### API Details page improvements

* You can set a payload limit size to download the response as a file if the response exceeds the size you have set.
* You can configure the colors of the different methods for both SOAP and REST APIs.
* You can set a flag to include or exclude the value for the `externalDocs` attribute. The flag defaults to **On**. When the flag is enabled, the value for the externalDocs attribute is appended to the **Description** field and rendered as part of that field.
* You can configure whether to show **Try it** for groups of HTTP methods per connected API Manager instance. This allows for finer grained control of the **Try it** functionality.

For more information, see [Customize API Catalog](/docs/apim_administration/apiportal_admin/customize_apicatalog_overview/)

## Limitations of this update

This update has the following limitations:

* API Portal 7.7.20200330 is compatible with API Gateway and API Manager 7.7.20200330 only.
* Upgrade to API Portal 7.7.20200330 is supported from [API Portal 7.7](/docs/apim_relnotes/201904_release/apip_relnotes/). To upgrade from earlier versions, you must first upgrade to API Portal 7.7.
* This update is not available as a virtual appliance or as a managed service on Axway Cloud.

*This section was updated to remove the information about API Portal in Docker containers.*

## Removed features

* Documentation is no longer provided in PDF format. You can continue to save individual topics or entire guides in PDF format using the **Save as PDF** icon on the [Axway documentation portal](https://docs.axway.com/).

## Fixed issues

This version of API Portal includes the fixes from all 7.5.5, 7.6.2, and 7.7 service packs or updates released prior to this version. For details of all the service pack fixes included, see the corresponding SP Readme attached to each service pack on [Axway Support](https://support.axway.com).

### Fixed security vulnerabilities

<!-- TODO copy and paste the list from confluence -->

| Internal ID | Case ID | Description                                                                                                                                                                                     |
| ----------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-3082    |         | **Issue:** `node-sass` package is vulnerable to uncontrolled recursion. **Resolution:** `node-sass` was moved to development packages.                                                          |
| IAP-2878    |         | **Issue**: XSS vulnerability because arbitrary (non-existing) URIs can be accepted with `Itemid` query parameter. **Resolution**: *Page Not Found* is shown when non-existing URI is requested. |

### Other fixed issues

| Internal ID | Case ID  | Description                                                                                                                                                                                                                                      |
| ----------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| IAP-2741    |          | **Issue**: There were two query parameters with same value on Try It page (itemId and menuId). **Resolution**: `menuId` is removed in favor of `itemId`.                                                                                    |
| IAP-2871    | 1106851  | **Issue**: Users are unexpectedly logged out and redirected to the Sign In page after password change. **Resolution**: Users are informed that they will be logged out after successful password change, and a result message is displayed. |
| IAP-2952    |          | **Issue**: While testing endpoints, when the Content-Type is set to application/octet-stream the upload of files is not possible. **Resolution**: Files are always uploaded successfully despite the Content-Type header.                  |
| IAP-3075    |          | **Issue**: Users are not redirected to login page when they try a request with expired session due to legacy authentication mechanism. **Resolution**: The legacy authentication mechanism is replaced with the newest possible.                 |
| IAP-3121    |          | **Issue**: SwaggerUI cannot render when OAS 3.0 definitions have missing *component* key. **Resolution**: The *component* key is first checked for existence, and then used.                                                                     |
| IAP-3132 | | **Issue:** PHP version requirements are not specific. **Resolution:** Added clarification to the documentation that PHP 7.4 is supported after Jan20 version and in API Portal 7.6.2 the supported versions for PHP are from 7.1 to 7.3. |
| IAP-3139    | 01140234 | **Issue**: There was a blank space between site navigation and page title when system messages were closed. **Resolution**: Container of system messages is removed when all of messages are closed. |

## Known issues

The following are known issues in this version of API Portal.

### Updating API Portal from January 2020 to March 2020 requires the clean up of the temporary folder

Before updating your API Portal 7.7.20200130 (January 2020) to API Portal 7.7.20200330 (March 2020), you must first clean up the `/tmp/axway-apiportal` folder. After that, you can run the update script.

### Page layout and alignment for Arabic language

Changing the API Portal language to Arabic (or any other right to left language) results in issues with page layout and alignment on the API Portal Home and Pricing pages, and some buttons are not visible. As a workaround, you can turn on the development mode in JAI. Follow these steps:

1. Log in to Joomla! Admin Interface (JAI).
2. In the JAI top navigation bar, click **Extensions > Templates**.
3. Click your template style (for example, `purity_III * Default`) to open it.
4. Click the **General** tab.
5. Change **Development Mode** to `ON`.
6. Click **Save** and click **Close** to close the template style.

Related Issue: IAP-308

### Sign in button is shown after user logs in to API Portal

After user logs in to API Portal, the **Sign in** button is shown in the banner section instead of the **Explore** button.

Related Issue: IAP-3037

## Documentation

This section describes documentation enhancements and related documentation.

### Documentation enhancements

The latest version of API Gateway, API Manager, and API Portal documentation has been migrated to Markdown format and is available in a [public GitHub repository](https://github.com/Axway/axway-open-docs) to prepare for future collaboration using an open source model. As part of this migration, the documentation has been restructured to help users navigate the content and find the information they are looking for more easily.

Documentation change history is now stored in GitHub. To see details of changes on any page, click the link in the **Last modified** section at the bottom of the page.

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

Email [support@axway.com](mailto:support@axway.com) or visit [Axway Support](https://support.axway.com/).

See [Get help with API Gateway](/docs/apim_administration/apigtw_admin/trblshoot_get_help/) for the information that you should be prepared to provide when you contact Axway Support.
