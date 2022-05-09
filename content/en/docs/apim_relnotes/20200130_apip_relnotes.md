---
title: API Portal 7.7 January 2020 Release Notes
linkTitle: API Portal January 2020
weight: 160
date: 2019-08-08T00:00:00.000Z
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

The following new features and enhancements are available in this release.

### Custom install directory

The restriction to install API Portal into the `/opt/axway/apiportal/htdoc` default directory has been removed, and you can now perform the installation in a directory of your choice.

### Unattended mode installation

Installing API Portal using unattended mode no longer requires you to know the correct positions of the parameters for the install script to work. Named parameters are now supported, which allows you to specify the parameters by name rather than position.

Validation and error messaging for command line errors were improved. For more information, see [Unattended installation](/docs/apim_installation/apiportal_install/install_unattended/).

### Home page customization

The home page has been completely rebuilt using Joomla! modules, which allow for a more extensively customization using the configuration settings in the Joomla! Admin Interface (JAI). No source code changes are required.

For more information, see [Customize your home page layout](/docs/apim_administration/apiportal_admin/customize_getting_started/#customize-your-home-page-layout).

### Application tab improvements

You can customize a description to the **Applications** page header.

A new message is shown upon application's creation.

The style is consistent across all Info, Warning, and Error messages.

### Change of behavior for the API `Information source` setting

Previously, this setting applied to both the list of APIs and the API details view at the same time. Now, it applies to the list of APIs only. This is useful if you want to display only a summary of the API on the API listing, but want a full description when viewing the API details. You can set the API detail's view in API Manager.

For more information, see [Customize source of API descriptions](/docs/apim_administration/apiportal_admin/customize_apicatalog_overview/#customize-source-of-api-descriptions).

### Change of behavior to customize standard footer

Previously, this setting used text field in **Extensions > Languages > Overrides**. Now, you can [customize API Portal standard footer, and add rich text to it](/docs/apim_administration/apiportal_admin/customize_getting_started/#customize-standard-footer).

### Control the visibility of APIs in the catalog

A new setting, **Do not show APIs with tags**, was added to the API Catalog menu options. This setting supports `*` and `?` wildcards, and it is a powerful option when used in combination with **Show APIs with tags** setting.

For more information, see [Group APIs with tags](/docs/apim_administration/apiportal_admin/customize_apicatalog_overview/#group-apis-with-tags).

### Open API Specification (OAS) 3.0 Support

OAS3 support is enabled and integrated with the Swagger.io UI component to bring the standardized look and feel of Swagger.io right into the core of API Portal. The additional configuration added on top of the basic integration allows for more control than ever over your favorite Swagger interface.

For more information, see [Additional features for API Catalog view](/docs/apim_administration/apiportal_admin/apip_overview/#additional-features-api-catalog-view).

### Further Enhancements

* Support for PHP 7.4 was introduced in this update.

*This section was updated to amend the information about Support for PHP versions.*

## Limitations of this release

This release has the following limitations:

* API Portal 7.7.20200130 is compatible with API Gateway and API Manager 7.7.20200130 only.
* Upgrade to API Portal 7.7.20200130 is supported from [API Portal 7.7](/docs/apim_relnotes/201904_release/apip_relnotes/) only. To upgrade from earlier versions, you must first upgrade to API Portal 7.7.
* This update is not available as a virtual appliance or as a managed service on Axway Cloud.

*This section was updated to remove the information about API Portal in Docker containers.*

## Removed features

* Documentation is no longer provided in PDF format. You can continue to save individual topics or entire guides in PDF format using the **Save as PDF** icon on the [Axway documentation portal](https://docs.axway.com/).

## Fixed issues

This version of API Portal includes the fixes from all 7.5.5, 7.6.2, and 7.7 service packs or updates released prior to this version. For details of all the service pack fixes included, see the corresponding SP Readme attached to each service pack on [Axway Support](https://support.axway.com).

### Fixed security vulnerabilities

| Internal ID   | Case ID            | Description             |
| ------------ | ------------------ | -----------------------|
| IAP-2551     |                    | **Issue**: API Portal was vulnerable to session hijacking. **Resolution**: Session ID is checked against the client IP when the user is logged in, and API Portal is no longer vulnerable to session hijacking. |
| IAP-1938     |                    | **Issue**: Missing section for accepted HTTP request methods. **Resolution**: Added section "Allow requests from only used HTTP methods" into the documentation.                                                |
| IAP-1930     |                    | **Issue**: Missing documentation section on how to reject requests containing unexpected or missing content type headers. **Resolution**: New documentation section was added to describe this best practice.   |
| IAP-2556     |                    | **Issue**: `swagger-oauth.js` file does not open encoded URL properly. **Resolution**: `swagger-oauth.js` file has been deleted as it is not used anywhere.                                                              |  
| IAP-2538     |                    | **Issue**: API Portal is using old vulnerable version of jQuery DataTables. **Resolution**: API Portal is using latest version of jQuery DataTables with all security vulnerabilities remediated.               |
| IAP-2539     |                    | **Issue**: Tablesorter library triggered four critical vulnerabilities based on static code scanning. **Resolution**: Tablesorter library is removed and replaced with DataTables.                                |
| IAP-2048     |                    | **Issue**: Missing section for integrity of the logging system. **Resolution**: Added section "Protect the integrity of the logging system" into the documentation.                                             |
| IAP-2013     | 1045070            | **Issue**: HTML source of Application Details page contained secret keys for both API Key and OAuth authentication methods. **Resolution**: Secrets are sent in a separate request.                             |
| IAP-2541     | 1087799            | **Issue**: Missing information for IP/Network disclosure. **Resolution**: New section with "Recommended settings for Apache" was added.                                                                         |
| IAP-2879     |                    | **Issue**: URL builder library introduced XSS-vulnerable strings. **Resolution**: XSS-introducing characters are escaped in URL builder library.                                                                |
| IAP-2705     | 01090752, 01090952 | **Issue**: Buffer overflow may occur in API Try It page. **Resolution**: Unnecessary regular expression check is removed, and concerned query parameters are properly cast.                                                   |

### Other fixed issues

| Internal ID | Case ID            | Description                                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------ | ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IAP-2157     | 985413             | **Issue**: When entering external credentials for an application a generic 'Something Went Wrong!' error page was displayed if the external credentials were already used by another application. **Resolution**: API Portal now displays a specific error message when duplicate external credentials are entered.                                                                                          |
| IAP-2160     | 1016059            | **Issue**: When the user is on API Details page the active API Catalog menu is not highlighted. **Resolution**: The correct API Catalog nav menu item is highlighted when an API from it is opened.                                                                                                                                                                                                          |
| IAP-2167     | 1031445            | **Issue**: When Authorization code flow is disabled in API Manager this API can not be loaded in API Portal. **Resolution**: There is no search explicitly for authorization code data anymore, and the page is properly loaded.                                                                                                                                                                                     |
| IAP-2224     | 1034603            | **Issue**: Some strings from API description in markdown are not properly encode in API Details page. **Resolution**: The whole API description is encoded and sanitized using the markdown language.                                                                                                                                                                                                        |
| IAP-2226     |                    | **Issue**: Metrics buttons were shown in the Applications page even if the corresponding manager does not support them. **Resolution**: Metrics buttons in the Applications page are shown only if the corresponding manager supports them.                                                                                                                                                                   |
| IAP-2233     | 1024460            | **Issue**: API Portal overwrites Content-Type header with multipart-formdata. **Resolution**: API Portal does not change the value of the Content-Type header anymore.                                                                                                                                                                                                                                       |
| IAP-2256     | 1066320            | **Issue**: When there is an example body with some really long string in it, for example, for some value, the example body editor changes its width. **Resolution**: Despite the length of the key or value in the example body, the body editor is with fixed width.                                                                                                                                     |
| IAP-2259     | 01064113, 01054690 | **Issue**: When the user is on monitoring tab in the Application page and there are no requests the description is misleading - it shows that the requests are from None organization. Also the requests table shows application id but its column is Application name. **Resolution**: When there are no requests the description is not shown. The column is renamed to "Application".                     |
| IAP-2272     |                    | **Issue**: Tags, Type, and Download definitions do not show up on API Test page in Internet Explorer 11. **Resolution**: The correct data for Tags, Type, and Download definitions are recognized on API Test page in Internet Explorer 11.                                                                                                                                                                     |
| IAP-2277     |                    | **Issue**: When recaptcha appears on login page and correct credentials are entered, user is redirected to password reset page (happens when password expire and password change on first login are enabled in API Manager and the user has expired password or this is his/her first login). **Resolution**: When trying to submit the login form without validating the recaptcha, error message is shown. |
| IAP-2287     | 1063673            | **Issue**: Application and API images are not displaying in Applications and API Catalog pages in Internet Explorer 11. **Resolution**: Images are correctly displayed in Application and API Catalog pages for Internet Explorer 11.                                                                                                                                                                        |
| IAP-2296     | 1068022            | **Issue**: Swagger definition is downloaded along with axway specific sensitive data. **Resolution**: Swagger definition is downloaded without sensitive data.                                                                                                                                                                                                                                               |
| IAP-2301     |                    | **Issue**: Everywhere when we have accordion with JsonSchema in it, the area below it is collapsed by default and it has to be explicitly clicked so it could fill with data. **Resolution**: Now when you expand an accordion it fills with JsonSchema immediately.                                                                                                                                         |
| IAP-2305     | 1064337            | **Issue**: SSO was unsuccessful in use case of sticky session configuration of Load Balancer in-front of two or more API Managers. **Resolution**: Using a load balancer with sticky session as a entry point for several instances of API Managers, does not cause issues with SSO.                                                                                                                          |
| IAP-2309     |                    | **Issue**: Newly created menu items have incorrect segments added to the URL. **Resolution**: Newly created menu items use the alias as URL.                                                                                                                                                                                                                                                                 |
| IAP-2327     |                    | **Issue**: Non-API Manager users can sign in to API Portal. **Resolution**: Disable Non-API Manager user login.                                                                                                                                                                                                                                                                                 |
| IAP-2328     |                    | **Issue**: API Portal stops working when the cache handler is changed to File. **Resolution**: Cache data is wiped out when the cache handler is changed.                                                                                                                                                                                                                                                    |
| IAP-2337     | 1054699            | **Issue**: API Portal user could not login to slave API Manager when the password contains special characters: `%` and `&`. **Resolution**: API Portal user successfully login to slave API Manager when the password contains special characters. (On upgrade from RTM 7.7 you must manually download and apply new Sample Policy Configuration file.)                                                      |
| IAP-2365     | 1068031            | **Issue**: Invoke policy name and description are not shown. **Resolution**: Invoke policy name and description are displayed where they are supposed to be and when they are needed.                                                                                                                                                                                                                        |
| IAP-2444     | 1075963            | **Issue**: Description from Swagger objects properties does not show up in API details page. **Resolution**: Description for Swagger object properties are displayed next to properties types. (Displaying an description for Array of objects is supported only for first level of nested objects).                                                                                                          |
| IAP-2464     | 1082546            | **Issue**: Last Visited field in user profile page always shows "Never". **Resolution**: Last Visited field in user profile shows last login time.                                                                                                                                                                                                                                                           |
| IAP-2466     | 1082179            | **Issue**: Typo in SSO login error message. **Resolution**: Typo is fixed.                                                                                                                                                                                                                                                                                                         |
| IAP-2467     | 1081280            | **Issue**: Application tab in API Tester page is always accessible, even if Applications page is disabled in Public API Mode. **Resolution**: Guest users never see the Application tab with enabled Public API mode. Tab mechanism is improved.                                                                                                                                                             |
| IAP-2469     | 01072660, 01062000 | **Issue**: Browser freezes on printing big response payloads. **Resolution**: Big response payloads to be downloaded as a file instead of printing them on API test page.                                                                                                                                                                                                                                    |
| IAP-2471     | 1079904            | **Issue**: Installation fails when log_bin_trust_function_creators is set to false. **Resolution**: Installation is successful no matter what the value of log_bin_trust_function_creators is.                                                                                                                                                                                                               |
| IAP-2479     | 1081962            | **Issue**: Markdown is not rendered for the description of custom inbound security policy. **Resolution**: Markdown is rendered for the description of custom inbound security policy.                                                                                                                                                                                                                       |
| IAP-2487     |                    | **Issue**: Organization Administrators coming from SSO flow can't set password on user creation due to invalid user type check. **Resolution**: User type check was fixed.                                                                                                                                                                                                                                   |
| IAP-2563     | 01067635, 01068119 | **Issue**: on Download definition for WSDL imported WSDL files are not included to downloaded zip archive, only imported XSD. **Resolution**: on Download definition imported WSDL files are included to downloaded zip along with imported XSD.                                                                                                                                                             |
| IAP-2581     | 01060674, 01072225 | **Issue**: Visualizing the same APIs metrics both in API Manager and API Portal displayed different results. **Resolution**: Cashing was removed to show correct metrics for each API in API Portal.                                                                                                                                                                                                         |
| IAP-2620     | 1081927            | **Issue**: Disabling edit "My Profile" in JAI is not reflected in API Portal. **Resolution**: Disabling edit "My Profile" in JAI disallows editing user profile.                                                                                                                                                                                                                                             |
| IAP-2743     |                    | **Issue**: Application delete dialog in "Details" and "Sharing" tabs in edit application page on delete attempt. **Resolution**: Application delete dialog appears on delete attempt.                                                                                                                                                                                                                        |
| IAP-2785     |                    | **Issue**: Access to restricted endpoints in API Manager's API was exposed in API Catalog page. **Resolution**: Query parameter of API Manager ID is filtered before making calls to API Manager.                                                                                                                                                                                                            |
| IAP-2799     | 1106396            | **Issue**: Default 'No authentication' option was available on SOAP APIs, which have 1 authentication method. **Resolution**: 'No authentication' option is available only on SOAP APIs with multiple authentication methods.                                                                                                                                                                                |
| IAP-2806     | 1105894            | **Issue**: Sorting is not case insensitive on API Catalog page. **Resolution**: Sorting is changed to be case insensitive.                                                                                                                                                                                                                                                                                   |
| IAP-2807     | 1105894            | **Issue**: Sorting is not case insensitive on API Filter. **Resolution**: Sorting is changed to be case insensitive.                                                                                                                                                                                                                                                                                         |
| IAP-2820     | 1101412            | **Issue**: Httpd error log is getting polluted with REST request and responses to/ from API Manager. **Resolution**: Curl verbose output option deactivated.                                                                                                                                                                                                                                                 |
| IAP-2874     | 1113649            | **Issue**: Results string in API Catalog couldn't be translated. **Resolution**: Result string is now translatable.                                                                                                                                                                                                                                                                                          |
| IAP-2916     |                    | **Issue**: when there are 2 API Catalogs and APIs are restricted by "Show only APIs with tags" with non-existing tags for only one of catalogs, error 302 appears on API Catalog page. **Resolution**: with 2 API Catalogs and APIs restricted by "Show only APIs with tags" with non existing tags API Catalog lists all APIs                                                                               |
| IAP-2935     |                    | **Issue**: Edit Profile menu item is unpublished on fresh install. **Resolution**: Edit Profile menu item is published on fresh install.                                                                                                                                                                                                                                                                     |

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

### Uploading files in API endpoints with Content-Type application/octet-stream is not possible while using OAS3

The execution of an endpoint with Content-Type application/octet-stream is not possible and the request results in an endless loader in the response section.

Related Issue: IAP-2952

### Imported Swagger 2.0 definitions into API Manager are not translated to OAS3, which results in unexpected behavior in API Portal

When a Swagger definition 2.0 is uploaded into API Manager its visualization and behavior in API Portal are unpredictable. This happens because Swagger 2.0 is not correctly translated into OAS3 definition by API Manager. One of the known problems is that the body parameters of POST endpoints are not displayed.

Related Issue: RDAPI-18389

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
