{
    "title": "About APIs ",
    "linkTitle": "Using APIs",
    "weight": "260"
}Application Programming Interfaces, or APIs, are a set of functions
that use a service. The service is followed by an application program.

Each of these services is described in one of the following sections, which provides an overview
of the service. Applications can address requests to {{< TransferCFT/axwayvariablesComponentShortName  >}} via
service call functions:

- [REST API](api_intro)
- [Web services](../../cft_intro_install/about_this_document_ibmi/using_apis/about_web_services)
- [Using JPI](../../cft_intro_install/about_this_document_ibmi/using_apis/java_api)
- Transfer
    services
- Synchronous
    communication services
- Catalog
    query services

The programming interfaces and programming language are related. The
services can be called by programs in COBOL or in C language as described in this section.

<span id="About_CFT_Services"></span>

About {{< TransferCFT/axwayvariablesComponentShortName  >}} services
-------------------------------------------------------------------------

You can query information related to correctly completed transfers.
This information can be accessed in the communication structure.

A description
of this structure sub-assembly is provided in the cftapi.h
for the C language, and cftapi.cop
for COBOL.

#### Restrictions

- Visual Basic APIs are available on PCs
    only. See [Windows
    operations](../../cft_intro_install/windows_install_start_here).
- The security APIs are not covered in this section. See [Managing
    security]().
