{
    "title": "UNIX: Installation and operation",
    "linkTitle": "Installation and operation UNIX",
    "weight": "90"
}This section introduces prerequisite information as well as installation and information on operating Transfer CFT in UNIX.

The information in this section
may be supplemented, corrected, or even contradicted by the
README.TXT file or the Release Notes supplied with the product. The README.TXT file and Release Notes take priority in this case.

<span id="Product_presentation"></span>

Product presentation
--------------------

{{< TransferCFT/axwayvariablesComponentShortName  >}} can operate both as client and/or as server. The
number of simultaneous transfers that {{< TransferCFT/axwayvariablesComponentShortName  >}} can support
is defined by the license key. It is also limited by the properties of
the networks used. The TCP/IP network is supported.

Installing {{< TransferCFT/axwayvariablesComponentShortName  >}} for Unix
------------------------------------------------------------------------------

The installation section describes prerequisites and how to install, migrate, update and uninstal{{< TransferCFT/axwayvariablesComponentShortName  >}}.

- [Prerequisites](before_you_start_unix/prereqs_overview)
- [Start the installation](../windows_install_start_here/before_you_start_win/install_transfer_cft_1)

UNIX operations
---------------

- [{{< TransferCFT/axwayvariablesComponentShortName  >}}
    UNIX utilities](run_first_time_ux/use_cft_utilities)
- [Running
    {{< TransferCFT/axwayvariablesComponentShortName  >}} for the first time]()
- [Building
    {{< TransferCFT/axwayvariablesComponentShortName  >}} API applications](run_first_time_ux/api_applications_start_here)
- [Activating
    security](run_first_time_ux/run_first_time_ux/user_rights_and_interface_unix)
- [Specific
    configurations](run_first_time_ux/run_first_time_ux/specific_configurations_intro)

UNIX high availability
----------------------

When installing a cluster for high availability, after enabling the cluster option you must set the multi-node option to NO.

- Using
    AIX with IBM
- Solaris
    Sun cluster

> **Note**
>
> Note: The term
> Transfer CFT is used to designate the Transfer
> CFT software package on UNIX platforms.

> **Note**
>
> Note: Transfer CFT supports all POSIX file systems.

Installation schematic overview
-------------------------------

****Standalone installation****

![](/Images/TransferCFT/install01_(2).png)

****Multi-node installation****

****![](/Images/TransferCFT/install_multi.png)****
