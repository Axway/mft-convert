---
title: API Portal 7.7 May 2020 Release Notes
linkTitle: API Portal May 2020
weight: 140
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

The following new features and enhancements are available in this update:

### Integration with Amplify Unified Catalog

This integration between API Portal and the Unified Catalog is a proof of concept feature, and it is being released separately in a docker container as it is not general availability (GA) quality approved just yet. We are releasing it early to gather feedback.

This is the first client implementation of the Unified Catalog API.

API Portal connects to the Unified Catalog by way of a DevOps service account from Amplify, and it exposes the APIs published on the catalog, alongside APIs already exposed via any connected API Manager instances.

Target use cases include:

* Amplify customers that want a more customizable and extendable API developer portal.
* Existing API Portal customers who also have APIs published into the Unified Catalog can now see and interact with those assets in their current API Portal.

For more information on how to integrate API Portal and Amplify Unified Catalog, see [Expose APIs published from the Unified Catalog](/docs/apim_administration/apiportal_admin/expose_apis_published_from_catalog/).

Check out the demo video available at [How to show APIs from Amplify Unified Catalog](https://www.youtube.com/watch?v=b3F-WatXxUE&list=PLSlCpG9zsECpo8-JMZ2Cx4REDyUvpwy9v).

### Database password encryption

We have added the ability to encrypt the database password that is stored in a configuration file. You can choose to encrypt the password during the installation, or any time later.

For more information, see [Encrypt database password](/docs/apim_installation/apiportal_install/secure_harden_portal/#encrypt-database-password) or check out the demo video [How to encrypt the database password](https://www.youtube.com/watch?v=ID_qi4huAVw&list=PLSlCpG9zsECpo8-JMZ2Cx4REDyUvpwy9v).

### Other enhancements

* Removal of unused templates (`Beez3` and `Protostar`) from the Joomla! Admin Interface (JAI).
* Various security fixes, including better validation on input fields to prevent potential attacks.

## Limitations of this update

This update has the following limitations:

* API Portal 7.7.20200530 is compatible with API Gateway and API Manager 7.7.20200530 only.
* Upgrade to API Portal 7.7.20200530 is supported from [API Portal 7.7](/docs/apim_relnotes/201904_release/apip_relnotes/). To upgrade from earlier versions, you must first upgrade to API Portal 7.7.
* This update is not available as a virtual appliance or as a managed service on Axway Cloud.

*This section was updated to remove the information about API Portal in Docker containers.*

## Deprecated features

<!-- Add features that are deprecated here -->

As part of our software development life cycle we constantly review our API Management offering. As part of this review, no capabilities have been deprecated.

## Removed features

<!-- Add features that are removed here -->

To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, no capabilities have been removed.

## Fixed issues

This version of API Portal includes the fixes from all 7.5.5, 7.6.2, and 7.7 service packs or updates released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).

### Fixed security vulnerabilities

| Key      | Case Id | Release Note                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| -------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-3182 |         | **Issue**: Users could create or delete applications even in the cases when this functionality is disabled from JAI. **Resolution**: An additional check verifies whether creating or deleting applications is added when the corresponding form is submitted.                                                                                                                                                                                                                           |
| IAP-3184 |         | **Issue**: Email field when creating or editing applications was validated only on client side. **Resolution**: Email validation is added on back-end side.                                                                                                                                                                                                                                                                                                                              |
| IAP-3172 |         | **Issue**: There is no Content-Security-Policy configured. **Resolution**: A new section, [Define a restrictive Content Security Policy](/docs/apim_installation/apiportal_install/secure_harden_portal/#define-a-restrictive-content-security-policy), in the documentation explains why and where to add this policy, which helps to detect and mitigate certain types of attacks, including Cross Site Scripting (XSS) and data injection attacks. |
| IAP-3181 |         | **Issue**: Size of application/user images was verified only on client side. **Resolution**: Back-end validation was added.                                                                                                                                                                                                                                                                                                                                                              |
| IAP-3183 |         | **Issue**: When there are configured custom properties it appears that they are not secured enough. **Resolution**: The custom fields are now properly escaped before outputting.                                                                                                                                                                                                                                                                                                        |
| IAP-3163 |         | **Issue**: Database password was stored in plain text in configuration file. **Resolution**: We have added the ability to encrypt the database password that is stored in the configuration file.                                                                                                                                                                                                                                                                                        |

### Other fixed issues

| Key      | Case Id            | Release Note                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| -------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-2881 |                    | **Issue**: Joomla administrator is redirected to login page when trying to update a user, which has the same value for `username` and `name`. **Resolution**: User is successfully updated from Joomla! Administrator Interface (JAI).                                                                                                                                                                                                       |
| IAP-3037 |                    | **Issue**: After user is successfully logged in, the button on the home page is still called 'Sign-in'. **Resolution**: New settings were added to the home page module in JAI to allow to rename this button and set where it redirects to.                                                                                                                                                                                                 |
| IAP-3121 |                    | **Issue**: SwaggerUI cannot render when OAS 3.0 definitions has missing "component" key. **Resolution**: The "component" key is first checked for existence, and then used.                                                                                                                                                                                                                                                                  |
| IAP-3145 |                    | **Issue**: Logged users do not see public APIs in the catalog when Public API Mode is enabled. **Resolution**: You must make sure that all organizations in API Manager are granted to access the Public APIs. To learn more, see [Configure settings for Public API mode in API Manager](/docs/apim_administration/apiportal_admin/public_api_configure/#configure-settings-for-public-api-mode-in-api-manager) in the Axway documentation. |
| IAP-3242 |                    | **Issue**: Logo is customizable from Home menu item instead of from Template menu. **Resolution**: Logo is customizable from Template menu. For more information, see [Customize your logo](/docs/apim_administration/apiportal_admin/customize_getting_started/#customize-your-logo).                                                                                                                                                       |
| IAP-3245 | 01136358, 01138114 | **Issue**: On Swagger 2.0 base path is duplicating the host or it is shown several times. **Resolution**: The base path and the host are displayed separately.                                                                                                                                                                                                                                                                               |
| IAP-3253 |                    | **Issue**: Error message is not clear to indicate that the scope is missing when the user is testing an API, and tries to authorize with OAuth without selecting any scope. **Resolution**: A specific error message is shown if the scope is missing while requesting OAuth token.                                                                                                                                                          |
| IAP-3256 | 1152557            | **Issue**: The discovery Swagger file from API Manager contains defaults and examples for SOAPAction and SOAPBody, but these are not displayed in API Portal. **Resolution**: SOAPAction and SOAPBody values are correctly imported and displayed in API Portal.                                                                                                                                                                             |
| IAP-3257 | 1155022            | **Issue**: API Portal version downgrades after docker container restart. **Resolution**: Version comparison fixed in docker entry point so that API Portal version doesn't downgrade on container restart.                                                                                                                                                                                                                                   |
| IAP-3264 |                    | **Issue**: 'Explore' button should replace the 'Sign In' button in Home Page layout when Public API Mode is enabled. **Resolution**: 'Sign In' button is displayed in Home Page layout when Public API Mode is enabled.                                                                                                                                                                                                                      |
| IAP-3269 |                    | **Issue**: Home page is not shown correctly when having multiple languages installed. **Resolution**: All Home page sections are correctly displayed when having multiple languages installed.                                                                                                                                                                                                                                               |

## Known issues

The following are known issues in this version of API Portal.

### Page layout and alignment for Arabic language

Changing the API Portal language to Arabic (or any other right to left language) results in issues with page layout and alignment on the API Portal Home and Pricing pages, and some buttons are not visible. As a workaround, you can turn on the development mode in JAI. Follow these steps:

1. Log in to Joomla! Admin Interface (JAI).
2. In the JAI top navigation bar, click **Extensions > Templates**.
3. Click your template style (for example, `purity_III * Default`) to open it.
4. Click the **General** tab.
5. Change **Development Mode** to `ON`.
6. Click **Save** and click **Close** to close the template style.

Related Issue: IAP-308

### API Portal label is shown in header when upgrading from version 7.6.2

Upgrading API Portal from 7.6.2 to 7.7 May release might result in scenario where additional text of 'API Portal' is displayed in the header.

Related issue: IAP-3419. Fixed in the 7.7 July release.

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