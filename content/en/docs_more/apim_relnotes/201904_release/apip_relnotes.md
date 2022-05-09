{
"title": "API Portal 7.7 April 2019 Release Notes",
"linkTitle": "API Portal April 2019",
"weight":"110",
"date": "2019-09-07"
}

## Summary

API Portal provides an API consumer-facing interface that you can customize to match your corporate brand. API Portal is a layered product linked to API Manager, and requires both API Manager and API Gateway. For more information, see the API Gateway and API Manager documentation.

API Portal in Docker containers is available for production environment from [API Portal 7.7 November 2020](/docs/apim_relnotes/20201130_apip_relnotes/) update. For more information, see [Production-ready Docker container](/docs/apim_relnotes/20201130_apip_relnotes/#production-ready-docker-container).

*This section was updated to amend the information about API Portal in Docker containers.*

## New features and enhancements

The following new features and enhancements are available in this release.

### GDPR compliance and privacy management

This release includes the following capabilities for improved GDPR compliance and privacy management.

#### Privacy policy support

Enables you to define and manage privacy policies that your users must accept when signing up to your API Portal. This includes support for adding and configuring privacy policies, auditing user acceptances, and privacy notice version control and management.

#### Improved application sharing

Enhanced privacy controls provide you with a configurable way to restrict the amount of user data (user names and emails) that is exposed when users share applications in your API Portal.

#### New password expiration policies

Enables you to apply improved governance controls on user passwords. For example:

* Enforce password changes at first login to API Portal
* Set a password expiry interval to enforce how often users must change their password

API Portal applies the password policies configured in API Manager.

### API Catalog and Application views

The following improvements have been made to the API Portal API Catalog and Application views to make it easier for your application developers to browse and consume your APIs.

* New Application tab on API details page to enable app developers to create new applications quickly and directly without navigating away from the API details page.
* API statuses (deprecated or unpublished) are now shown in the Application view to give app developers better information and help prevent accidental subscriptions to deprecated APIs.
* New option to show summaries instead of descriptions for APIs to give the app developer a quicker summary view of what the API is about instead of a long description.
* API versions are now shown in the Application view to give app developers better information when subscribing to APIs.
* Monitoring information is not shown for applications or APIs from a connected API Manager with metrics disabled.

### Customization options

To increase flexibility when customizing API Portal, the following new customization options are available.

* View and update custom properties that are set and configured in API Manager, directly in API Portal.
* Map users to roles after login, to control which parts of the Joomla! Administrator Interface (JAI) they can access. User role mapping enables you to expose different capabilities of the (JAI) to users, based on whether they are a standard user or an organization administrator, the organization they belong to, and even their email address.
* Wildcard support for tags, so you can more easily control which APIs are displayed or hidden in your API catalogs.
* Customize the default API Portal 404 error page.
* Configure a consent message to be shown to users when logging in to API Portal.
* Option to enforce users to supply an employer name when signing up.

Using these standard customization options ensures a smooth upgrade process and will enable you to preserve your customizations when you upgrade to future API Portal releases.

## Limitations of this release

This release has the following limitations:

* API Portal 7.7 is compatible with API Gateway and API Manager 7.7 only.
* Upgrade to API Portal 7.7 is supported from API Portal 7.6.2 only. To upgrade from earlier versions, you must first upgrade to 7.6.2.
* This release is not available as a virtual appliance, or as a managed service on Axway Cloud.

*This section was updated to remove the information about API Portal in Docker containers.*

## Fixed issues

### Fixed security vulnerabilities

| Internal ID | Case ID | Description    |
| ----------- | ------- | -------------- |
| IAP-1300    | 00987204 | **Issue**: Documentation did not include best practices for securing Joomla! **Resolution**: API Management Security Guide is being updated to include best practices for securing Joomla!                                                              | 2018-6376 |
| IAP-1830    | 01030372 | **Issue**: The description of the API was not escaped when using an external URL. **Resolution**: External URL description is escaped now.                                                                                                              |           |
| IAP-1846    | 01029705 | **Issue**: Sensitive information (for example, session headers, csrf token headers, and so on) is stored in curl.log file. **Resolution**: Sensitive information is masked (the real value is replaced with a fake value) when stored in curl.log file. |           |
| IAP-1905    |          | **Issue**: When testing an API, malicious code inserted in some query parameters was executed, making API Portal vulnerable to cross-site scripting (XSS). **Resolution**: All query parameters are now escaped and malicious code is not executed.     |           |

### Other fixed issues

| Internal ID | Case ID | Description     |
| ----------- | ------- | --------------- |
| IAP-911     | 00955815, 00940446 | **Issue**: Documentation stated that PHP 5.4 and later are supported on RHEL7 software installation, but API Portal installation failed with PHP 7 due to missing dependencies. **Resolution**: API Portal 7.7 RHEL7 software installation includes improved dependency checking, which resolves this issue.|
| IAP-1421    |                    | **Issue**: When an anonymous user accesses API Portal, a new API Manager session is created. **Resolution**: All anonymous users share a single API Manager session.|
| IAP-1480    | 00997076           | **Issue**: Enumerations are not displayed for method parameters. **Resolution**: Enumeration values are displayed.|
| IAP-1482    | 00997076           | **Issue**: AMPLIFY Swagger UI does not display API method tags. **Resolution**: AMPLIFY Swagger UI now displays API method tags.|
| IAP-1483    | 00995497           | **Issue**: API Portal users cannot log in to Joomla! if granted Administrator or Super User privileges in JAI. **Resolution**: API Portal 7.7 includes a new user role mapping feature which resolves this issue.|
| IAP-1611    | 01010562           | **Issue**: Unofficial best practice documentation describing how to move API Portal configuration and customizations from one environment to another, leveraging Akeeba Backup, was incorrect and outdated. **Resolution**: The documentation was updated.|
| IAP-1652    |                    | **Issue**: When configuring connections to multiple API Managers in JAI, the value of the 'Public label for API Managers' field is not saved. **Resolution**: 'Public label for API Managers' field is saved as expected.|
| IAP-1669    | 01007453           | **Issue**: Customizing styles for some API Portal elements using ThemeMagic has no effect. **Resolution**: It is not possible to customize all elements shown in ThemeMagic. The elements that cannot be customized have been removed from the ThemeMagic customization page:- Basic Colors/Footer Text Color (@footer-text-color)- Footer Color (Advanced)/Footer Text Color (@t3-footer-text-color)- Footer Color (Advanced)/Footer Module Background (@t3-footer-module-background)- Footer Color (Advanced)/Footer Module Text Color (@t3-footer-module-text-color)- Footer Color (Advanced)/Footer Module Title Color (@t3-footer-module-title-color)- Drop Down Color (Advanced)/Disabled background color (@aap-dropdown-disabled-bg-color)- Drop Down Color (Advanced)/Disabled text color (@aap-dropdown-disabled-text-color)- Table Color (Advanced)/Row disabled background color (@aap-tr-disabled-bg-color)- Spotlight Color (Advanced)/Spotlight Background Color (@t3-spotlight-background)- Spotlight Color (Advanced)/Spotlight Text Color (@t3-spotlight-text-color) |
| IAP-1699    |                    | **Issue**: When Public API mode is enabled and an anonymous user tries to open the APIs or Applications pages, the user is redirected to the sign in page. **Resolution**: The Public API mode now displays the APIs and Applications pages for anonymous users.|
| IAP-1701    |                    | **Issue**: When Public API mode is enabled, anonymous users can access internal pages. **Resolution**: An error is shown when an anonymous user tries to access internal pages, and the user is redirected to the sign in page.|
| IAP-1742    |                    | **Issue**: When the Public API mode is enabled, an anonymous user can open the user profile page. **Resolution**: A 404 error page is shown when an anonymous user tries to open the user profile page.|
| IAP-1752    | 01018549           | **Issue**: A very long range for the date parameter returns an extensive number of results, allowing an attacker to consume numerous back-end resources. **Resolution**: A back-end restriction was added to limit users to retrieving results for a custom defined time period of two months only.|
| IAP-1760    | 01019664           | **Issue**: In some cases the API key was not sent to API Manager when trying a method secured with an API key. **Resolution**: API key is sent to API Manager in all cases when API key security is selected.|
| IAP-1767    |                    | **Issue**: API details page does not show the Authentication section if there are no SDKs available for download. **Resolution**: The Authentication section is shown even when there is no SDK available for download.|
| IAP-1770    |                    | **Issue**: API Portal checked for HTTP Headers using Pascal case with hyphens. So, when the headers were all in lowercase the request could not be sent to API Manager. **Resolution**: All headers are now modified to the expected format before API Portal reads them.|
| IAP-1789    | 01026967           | **Issue**: Clicking the "Save application" button multiple times, almost simultaneously, in the application creation form creates several applications. **Resolution**: All buttons in the form are locked while the submission is being processed, so the button is not clickable again until the application is created.|
| IAP-1795    |                    | **Issue**: Error message (API does not belong to a catalog) is shown when trying to open Application page for application with linked APIs and the Application page is not displayed. **Resolution**: Application page is displayed as expected.|
| IAP-1807    | 01015588           | **Issue**: When locked out after consecutive failed login attempts (bad password), the user can reset the password, however, attempts to log in again with the new password fail until the lock time expires. **Resolution**: Passwords cannot be reset until the lock time expires.|
| IAP-1823    |                    | **Issue**: Something went wrong page is shown when generating OAuth credentials with an empty redirect URL field. **Resolution**: A meaningful error message is shown when generating OAuth credentials with an empty redirect URL field.|
| IAP-1852    | 01022998           | **Issue**: Joomla! administrator user gets an invalid certificate error when uploading certificate chain PEM file. **Resolution**: You can now upload a certificate chain file successfully, but you must first install and configure the OpenSSL PHP extension.|
| IAP-1857    | 01024680           | **Issue**: API Details page (Swagger UI) is not rendering under Internet Explorer 11. **Resolution**: API Details page is fully functional under Internet Explorer 11.|
| IAP-1894    |                    | **Issue**: If you enable public API user password encryption, you cannot save the public API user account password when the Linux security module (SELinux) is enabled, because SELinux prevents writing to the encryption key file. **Resolution**: Public API user password can be encrypted and saved as expected when SELinux is enabled.|
| IAP-1898    | 01019882           | **Issue**: A broken hidden error message (small yellow triangle) is shown below the main menu after a new user logs in. **Resolution**: The issue was fixed after the correct PHP module was installed.|
| IAP-1901    | 01024460           | **Issue**: API Portal overwrites Content-Type header with multipart-formdata. **Resolution**: API Portal does not change the value of the Content-Type header anymore.|
| IAP-1937    | 01028187           | **Issue**: When multiple API managers are configured, only metrics from the master API Manager are shown under Monitoring in API Portal. **Resolution**: Metrics data is now shown for master and slave API Managers.|
| IAP-1966    | 01042321           | **Issue**: When using SSO to log in, user is redirected to API Portal home page instead of the page specified in the 'Redirect after login' field. **Resolution**: User is redirected to the specified page as expected.|
| IAP-1977    | 01044495           | **Issue**: When using SSO, the SSO flow is not executed correctly when the IdP uses the 'Location' header for the redirect. **Resolution**: The SSO flow executes correctly as expected.|
| IAP-1981    | 01004660           | **Issue**: When there are two languages installed, and the default one is not English, trying an API method using an expired session reloads the page instead of redirecting to the sign-in page as expected. **Resolution**: The user is redirected to the sign-in page correctly.|
| IAP-2052    | 01022066           | **Issue**: API Portal shows application sharing option for a Community organization which was renamed in Policy Studio. Application sharing should not be enabled for Community organizations. **Resolution**: API Portal does not show application sharing for any Community organization.|

## Known issues

The following are known issues in this version of API Portal.

### Page layout and alignment for Arabic language

If you change the API Portal language to Arabic (or any other right to left language) there are issues with page layout and alignment on the API Portal Home and Pricing pages, and some buttons are not visible. As a workaround, you can turn on development mode in JAI. Follow these steps:

1. Log in to Joomla! Admin Interface (JAI).
2. In the JAI top navigation bar, click **Extensions > Templates**.
3. Click your template style (for example, `purity_III - Default`) to open it.
4. Click the **General** tab.
5. Change **Development Mode** to `ON`.
6. Click **Save** and click **Close** to close the template style.

Related **Issue**: | IAP-308

### Related documentation

To find all available documents for this product version:

1. Go to <https://docs.axway.com/bundle>.
2. In the left pane Filters list, select your product or product version.

{{< alert title="Note" color="primary" >}}Customers with active support contracts need to log in to access restricted content.{{< /alert >}}

The AMPLIFY API Management solution enables you to create, publish, promote, and manage Application Programming Interfaces (APIs) in a secure and scalable environment.

The following reference documents are also available:

* [Supported Platforms](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en) - Lists the different operating systems, databases, browsers, and thick client platforms supported by each Axway product.

* [Interoperability Matrix](https://docs.axway.com/bundle/Axway_Products_InteroperabilityMatrix_allOS_en) - Provides product version and interoperability information for Axway products.

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email <support@axway.com> or visit [Axway Support](https://support.axway.com).

See for the information that you should be prepared to provide when you contact Axway Support.
