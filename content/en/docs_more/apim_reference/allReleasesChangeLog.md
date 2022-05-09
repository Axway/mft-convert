{
"title": "API Management release change log",
  "linkTitle": "Release change log",
  "weight": "90",
  "date": "2020-10-20",
  "description": "Functionality change log for all supported versions of API Gateway, API Manager, and API Portal since version 7.5.3."
}

List of the major changes made in each release of API Gateway, API Manager, and API Portal. This is a reference information to help you to decided the best strategy to migrate your product to the latest API Management release.

You can find the release notes for all service packs on [Axway Support](https://support.axway.com).

## Version 7.7 2021

|Update       |New features and enhancements        |Important Changes    |Deprecated features  |Release Date|
|---------    |---               |---                  |---                  |---         |
|[7.7.20210530](/docs/apim_relnotes/20210530_apimgr_relnotes/)|Dependency view and revoke access to front-end APIs|From the May update onwards, the update schedule will change from every two months to every three months.                  |PS Antivirus filters                 |30/05/2021|
||Import APIs over HTTPS through a HTTPS proxy server|Embedded ActiveMQ host name verification|The packet sniffing capability is deprecated from this update                 ||
||YAML configuration store (GA)|INSTALL_DIR/apigateway folder permissions changed|MariaDB 5.5||
||Support for MariaDB 10.5|Replacement of MD5 hashed API Gateway configuration files with SHA256|- ||
||New SHA-256 hash algorithm option on SFTP server fingerprint check|Changes to JWT Verify filter|- ||
||SFTP server fingerprint check (MD5 hash algorithm)|Analytics CSP Header change|- ||
||-         |Authorization records threshold added to API Manager OAuth Screen|-||
|**Update**       |**New features and enhancements**        | **Important Changes**    |**Deprecated features**  |**Release Date**|
|[7.7.20210330](/docs/apim_relnotes/20210330_apimgr_relnotes/)|Updated cipher scheme |Apache Cassandra security advisory|Antivirus filters |30/03/2021 |
||Passphrase policy enforcement|Changes in JWT filters|End of Support for API Management version [7.7 January 2020](/docs/apim_relnotes/20200130_apimgr_relnotes/) |
||Security enhancements to JWT Sign and Verify filters|New Cipher schemes for configuration and KPS|End of Support for the browser Internet Explorer 11|
||Automatic upgrade of projects in Policy Studio|Driver for metrics database|End of Support for MySQL 5.6|
||API Manager request rate limiter|- | -|
||HTTP strict transport security profile|- | -|
||Certification with MySQL 8|- | -|
||YAML configuration store (Technical preview capability)|- |- |
|**Update**       |**New features and enhancements**        | **Important Changes**    |**Deprecated features**  |**Release Date**|
|[7.7.20210130](/docs/apim_relnotes/20210130_apimgr_relnotes/)| Option to disable SSL renegotiation is added for HTTPS listeners and Connection filters |- |-  |30/01/2021 |
| |Backup passphrase parameter to restore operation in kpsadmin|-  | -| |
| |Content Security Policy header to improve security|-  | -| |
| |Apache Cassandra security advisory|-  | -| |
| |Changes in JWT filters|-  | -| |
| |New Cipher schemes for configuration and KPS|-  | -| |
| |Driver for metrics database|-  | -| |
| |YAML configuration store (Technical preview capability)|-  | -| |

## Version 7.7 2020

|Update       |New features and enhancements        | Important Changes   |Deprecated features  |Release Date|
|---------    |---               |---                  |---                  |---         |
|[7.7.20201130](/docs/apim_relnotes/20201130_apimgr_relnotes/)|JVM Flag added to allow orgAdmins to manage API Lifecycles|-|-|30/11/2020
||OAS3 runtime bugs resolved|-|-| |
||Simplified Cassandra backup and restore scripts|-|-| |
|**Update**       |**New features and enhancements**        | **Important Changes**   |**Deprecated features**  |**Release Date**|
|[7.7.20200930](/docs/apim_relnotes/20200930_apimgr_relnotes/)|API Manager Remote Hosts aligned with PolicyStudio functionality |OpenJDK JRE update to v8u265| API Management REST APIs versions 1.1 and 1.2 are deprecated. |30/09/2020|
|                                                                                                |YAML-based configuration store (Technical preview) |Removed broken and risky algorithms from sFTP server |Swagger version 1.1 is deprecated ||
|                                                                                                |Database integration updates |Groovy v2.5.13 |The requirement to run `update-apimanager.py` has been removed from the upgrade steps. | |
|                                                                                                |User membership in multiple organizations. |Filebeat v6.2.2 |- | |
|                                                                                                |New API Management REST API version 1.4. |OpenSSL and FIPS support |- | |
|                                                                                                |OpenJDK JRE update to v8u265. |Increased validation of WSDLs |- | |
|                                                                                                |Changes to the way API Gateway verifies wildcarded certificates. |API Gateway/Manager behavior changes|- | |
|                                                                                                |The Groovy version used by the Scripting Filter is updated to 2.5.13. | Security updates|- | |
|**Update**       |**New features and enhancements**        | **Important Changes**  |**Deprecated features**  |**Release Date**|
|[7.7.20200730](/docs/apim_relnotes/20200730_apimgr_relnotes/)|ICAP filter improvements. |Filebeat v6.2.2 |- |30/07/2020 |
|                                                                                                |Improved upgrade instructions. |OpenJDK JRE |- | |
|                                                                                                |MSSQL Database Support. |OpenSSL and FIPS support |- | |
||-|Increased validation of WSDLs |-||
||-|API Gateway/Manager behavior changes |-||
||-|Security updates |-||
|**Update**       |**New features and enhancements**        | **Important Changes**  |**Deprecated features**  |**Release Date**|
|[7.7.20200530](/docs/apim_relnotes/20200530_apimgr_relnotes/)|Updated OS Support. |TLS v1.3 support| -|30/05/2020 |
|                                                                                                |Improved upgrade experience. |Filebeat v6.2.2 |- | |
|                                                                                                |Log4j file format changed to YAML. |OpenJDK JRE |- | |
|                                                                                                |Swagger parser libs updated. |OpenSSL and FIPS support |- | |
||-|Log4j 2 changes |-||
||-|Swagger parser |-||
|**Update**       |**New features and enhancements**        | **Important Changes**  |**Deprecated features**  |**Release Date**|
|[7.7.20200330](/docs/apim_relnotes/20200330_apimgr_relnotes/)|Added TLS 1.3 support. |Filebeat v6.2.2 |FIPS support was removed. |30/03/2020 |
|                                                                                                |OpenSSL updated to v1.1.1. |OpenJDK JRE |McAfee Sophos Clam AV embedded antivirus scanners were removed.||
||-|OpenSSL and FIPS support |-||
|**Update**       |**New features and enhancements**        | **Important Changes**  |**Deprecated features**  |**Release Date**|
|[7.7.20200130](/docs/apim_relnotes/20200130_apimgr_relnotes/)|Swagger 2.0 enhancements. |Increased validation of WSDL |API Tester was removed from installation.|30/01/2020 |
|                                                                                                |Open API Specification (OAS) 3.0 enhancements. |Filebeat v6.2.2 |RAML support was removed.| |
|                                                                                                |Try it improvements. |Increased validation of /users endpoint |Export of back-end APIs is not supported for OAS3 or WSDL APIs.| |
|                                                                                                |Usage tracking improvements. |OpenJDK JRE |Documentation is no longer provided in PDF format.| |
|                                                                                                |Back-end API improvements. |API Gateway/Manager behavior changes|- | |
|                                                                                                |API documentation enhancements. |Security updates |- | |
|                                                                                                |Multi organization (beta). |- |- | |
|                                                                                                |Increased validation of WSDLs. |- |- | |
|                                                                                                |Filebeat updated to v6.2.2. |- |- | |
|                                                                                                |Increased validation of users endpoint. |- |- | |
|                                                                                                |Undesirable cipher suites disabled by default when using SSL/TLS. |- |- | |
|                                                                                                |Endpoint identification algorithms for LDAPS enabled by default. |- |- | |
|                                                                                                |Fields that contain confidential information are no longer returned in some API calls.|- |- | |

## Version 7.7 2019

|Update       |New features and enhancements | Important Changes |Deprecated features  |Release Date|
|---------    |---                           |---                |---                  |---
|7.7 SP2      |OpenSSL updated to 1.0.2t-fips to fix security vulnerabilities. |- |- |21/12/2019 |
|7.7 SP1|- |- |- |29/08/2019 |
|[7.7](/docs/apim_relnotes/201904_release/)|Traffic monitor enhancements.|- |Support for MySQL Server 2005 has been removed.|12/04/2019 |
|                                          |Filter JMS transactions by custom property in Traffic Monitor.|- |- | |
|                                          |Entity Explorer added to API Gateway client tools installer.|- |- | |
|                                          |Elastic topology container deployment enhancements.|- |- | |

## Version 7.6.2

|Update       |New features and enhancements        |Deprecated features  |Release Date|
|---------    |---               |---                  |---         |
|7.6.2 SP5|The jre directory in Policy Studio/Configuration Studio must be remove before applying the Service Pack|- |17/07/2020|
|7.6.2 SP4|JQuery upadated to 3.4.0 to fix security vulnerabilities.|- |19/07/2019 |
|7.6.2 SP3|jQuery updated to 3.3.1 to fix security vulnerabilities.|- |18/04/2019 |
|                                                                                                                       |JRE updated to 1.8.0_202 to fix security vulnerabilities.|- | |
|7.6.2 SP2| |- | 17/12/2018|
|7.6.2 SP1|Spring framework updated to 4.3.18.RELEASE to fix security vulnerabilities |- | 01/11/2018|
|                                                                                                                |OpenSSL updated to 1.0.2p-fips to fix security vulnerabilities.|- | |
|                                                                                                                |Bouncy Castle library updated to 1.60 to fix security vulnerabilities.|- | |
|[7.6.2](https://docs.axway.com/bundle/APIGateway_762_ReleaseNotes_allOS_en_HTML5/page/Content/ReleaseNotesPortal/APIGateway_ReleaseNotes_allOS_en.htm)|Elastic topology container deployment.|Axway physical appliance deployment option was removed. | 26/07/2018|
|                                                                                                                |Smooth rate limiting.|The ConnectToUrlFilter.removePreviousConnections system property is no longer available. | |
|                                                                           |Support for /tmp directory mounted with noexec- Apache Cassandra has been upgraded to version 2.2.12|- | |

## Version 7.5.3

|Update       |New features and enhancements        |Deprecated features  |Release Date|
|---------    |---               |---                  |---         |
|7.5.3 SP13|Dojo updated to 1.10.10 to fix security vulnerability | - |01/05/2020|
|7.5.3 SP12|Improvements in the XML redaction.| - |26/102019|
|                                                                                                               |Jackson-databind updated to 2.9.10 to fix security vulnerabilities.|-  | |
|7.5.3 SP11|The Gateway now includes Zulu OpenJDK 1.8 JRE instead of Oracle JRE 1.8.|To allow legacy API method matching excluding the method’s defined MIME type, the Java property com.coreapireg.apimethod.contenttype.legacy must be set to true. |25/06/2019|
|                                                                                                               |It is possible to allow inbound API requests with trailing slash to match an API path with no trailing slash.|-  | |
|                                                                                                               |An API method’s Content-Type is now checked against the API method’s defined MIME type when performing path matching.|-  | |
|                                                                                                               |OpenSSL updated to 1.0.2r-fips.|-  | |
|                                                                                                               |Added CSRF token protection for API Gateway Management APIs.|-  | |
|                                                                                                               |JQuery updated to 3.4.0.|-  | |
|7.5.3 SP10|Jython updated to 2.7.1 to fix this security vulnerability.|Internet Explorer versions 8.0 and 9.0 are no longer officially supported by API Gateway. |22/02/2019|
|                                                                                                               |jQuery updated to 3.3.1 to fix this security vulnerability.|-  | |
|                                                                                                               |JRE updated to 1.8.0_202 to fix this security vulnerability.|-  | |
|7.5.3 SP9|IBM MQ updated to 7.5.0.8 to fix this security vulnerability.|- |16/11/2018|
|                                                                                                               |OpenSSL updated to 1.0.2p-fips to fix this security vulnerability.| - | |
|                                                                                                               |JRE updated to 1.8.0_191-b12 to fix this security vulnerability.|-  | |
|                                                                                                               |`apimanager-promote` script requires decryption password or passfile.|-  | |
|7.5.3 SP8|Bouncy Castle library updated to 1.60 to fix this security vulnerability.|- |23/08/2018|
|                                                                                                               |Spring framework updated to 4.3.17.RELEASE to fix this security vulnerability.|- | |
|                                                                                                               |JRE updated to 8u181 to fix this security vulnerability.|- | |
|7.5.3 SP7|Jackson-databind updated to 2.9.5 to fix this security vulnerability.|- |08/06/2018|
|                                                                                                               |JRE updated to 8u172 to fix this security vulnerability.|- | |
|                                                                                                               |OpenSSL updated to 1.0.2o to fix this security vulnerability.|- | |
|7.5.3 SP6|OpenSSL updated to v1.0.2.n to fix security vulnerability.|- |27/04/201|
|                                                                                                               |Apache Log4j updated to 2.8.2 to fix security vulnerability.|- | |
|                                                                                                               |When exporting API collections from API Manager, the generated file is now encrypted and not plainText.|- | |
|7.5.3 SP5|OpenSSL updated to 1.0.2m-fips to fix security vulnerability.|- |23/01/2018|
|                                                                                                               |JQuery updated to 2.2.4, to fix security vulnerability.|- | |
|                                                                                                               |commons-email updated to 1.5 to fix security vulnerability.|- | |
|7.5.3 SP4|Nimbus JOSE+JWT library updated to v4.41.2 to fix security vulnerability.|- |06/11/2017|
|                                                                                                               |RE version updated to v1.8u152 to fix security vulnerability.|- | |
|7.5.3 SP3|- |- |02/10/2017|
|7.5.3 SP2|JSCH update to 0.1.54 to fix security vulnerabilities. |- |31/07/2017|
|7.5.3 SP1|JRE updated to v8u131 to fix security vulnerability. |- |15/06/2017|
|[7.5.3](https://docs.axway.com/bundle/APIGateway_753_ReleaseNotes_allOS_en_HTML5/page/Content/ReleaseNotesPortal/APIGateway_ReleaseNotes_allOS_en.htm)|Apache ActiveMQ updated to 5.14.3 to fix security vulnerabilities.|- | 11/04/2017|
|                                                                                                               |JSCH update to 0.1.54 to fix security vulnerabilities.|Sun Access Manager filters have been deprecated and will be removed in a future release.| |
|                                                                                                               |owasp esapi updated to 2.1.0.1 to fix security vulnerabilities.|API Gateway Analytics is deprecated and will be removed in a future release. | |
|                                                                                                               |JRE updated to 8u121 to fix security vulnerabilities.|The OpenID connect filters in Policy Studio have been deprecated and will be removed in a future release.| |
|                                                                                                               |xmlsec updated to 1.5.8 to fix security vulnerabilities.|Support for Tivoli Access Manager version 6.0 has been removed and replaced by support for Tivoli Access Manager version version 6.1.| |
|                                                                                                               |spring-core updated to v4.3.5 to fix security vulnerabilities.|The ISO format for API Gateway virtual appliance has been replaced by the OVA format.| |
|                                                                                                               |Jsch dependency updated v0.1.54 to fix security vulnerabilities.|- | |
|                                                                                                               |Remove Fluent-hc dependency from API Gateway.|The jabber and restJabber code samples had been removed | |
