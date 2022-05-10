---
    "title": "About PassPort AM",
    "linkTitle": "PassPort type access management",
    "weight": "160"
---
{{% TransferCFT/snippets/access_management%}}

Concepts
--------

{{< TransferCFT/axwayvariablesCompanyName  >}} PassPort Access Management centralizes {{< TransferCFT/axwayvariablesComponentShortName  >}} access management. PassPort AM provides:

- Identity and access
    control for {{< TransferCFT/axwayvariablesCompanyName  >}} products
- Authentication
    via a user/password login
- Authorization enabling
    user access to {{< TransferCFT/axwayvariablesCompanyName  >}} products resources
- Role-based access
    that defines privileges for users

### User roles

The default roles are as follows:

- **Administrator** provides full user access
- **Helpdesk** enables you to view the Catalog and Log
- **Designer** allows you to manage application flows
- **Application** allows applications to request and manage transfers, and view the Catalog
- **PartnerManager** allows you to manage partners

Please refer to the [*Transfer CFT *{{< TransferCFT/axwayvariablesReleaseNumber  >}} *Security Guide*](https://docs.axway.com/bundle/TransferCFT_38_SecurityGuide_allOS_en_HTML5/page/Content/security_guide/predefined_privileges.htm) for a complete list of privileges and roles. (Requires login.)

### Tools

To configure the PassPort AM Connector in {{< TransferCFT/axwayvariablesComponentShortName  >}}, set the uconf
parameters described in [Configuring PassPort AM](configure_passport_am). You can use one of the following tools to set these parameters:

- CFTUTIL: CFTUTIL
    UCONFSET id=, value=
- Transfer CFT user interface:
    the [Unified Configuration](../../admin_intro/uconf/uconf_userinterface) window

<span id="CSD file"></span>

### CSD file

Access information for PassPort AM integration is stored in a configuration
file called Component Security Descriptor (CSD). This CSD file is delivered in an XML
format. A sample CSD file is provided with
the product.

A component identifies itself to PassPort via values for four properties: component name, component version, component group and instance.

PassPort knows three of these values (name, version, group) when it imports the CSD file. Upon importing the file, PassPort assigns an instance value of **default**. It is important to note that all four values must match on the PassPort and the Transfer CFT side. If not, your Transfer CFT cannot communicate with PassPort.

For more information, refer to [PassPort AM CSD](passport_am_csd).
