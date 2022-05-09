{
    "title": "Install Transfer CFT ",
    "linkTitle": "Install Transfer CFT ",
    "weight": "190"
}The {{< TransferCFT/axwayvariablesComponentShortName  >}} product comprises the {{< TransferCFT/axwayvariablesComponentShortName  >}} and the utilities supporting communications between {{< TransferCFT/axwayvariablesComponentShortName  >}} and users.

The installation procedure creates the environment in which {{< TransferCFT/axwayvariablesComponentShortName  >}} is to be executed. The system manager is responsible for assigning users the appropriate quotas and privileges to access the tools used to dialog with {{< TransferCFT/axwayvariablesComponentShortName  >}}.

There are two types of installation:

- Initial product installation: This procedure generates a full environment, comprising a standard directory structure. When the installation terminates correctly, a procedure is used to generate configuration files that enable you to perform a test loop-back transfer.

<!-- -->

- Upgrade: This installation procedure renames the previous {{< TransferCFT/axwayvariablesComponentShortName  >}} subdirectories and generates a new standard installation directory structure.

See the [Product license key](../../preinstallation#Product) section for information on the license key and the end user license.

Pre-installation
----------------

To prepare for the installation procedure:

1. Log into a privileged account and access the SYS$UPDATE directory. Enter the user name and password.

    `Username`: `SYSTEM`

    `Password`:

1. Access the directory where you uploaded the zipped installation kit files. The `Transfer_CFT_{{< TransferCFT/axwayvariablesReleaseNumber >}}_Install_vms-ia64_<BN>.zip` file contains the following archive files:
    -   cft036.a
    -   cft036.b
    -   cft036.c
    -   cft036.d
1. Unzip the file: `unzip Transfer_CFT_{{< TransferCFT/axwayvariablesReleaseNumber >}}_Install_vms-ia64_<BN>.zip`

Start the installation
----------------------

1. Enter the command: `@sys$update:vmsinstal cft036`

    The OpenVMS Software Product Installation Procedure screen is displayed.

1. Follow the screen instructions as shown in the following example installation, where 3.x represents the current version.
    ```
    \*Do you want to continue anyway [NO]? y \* Are you satisfied with the backup of your system disk [YES]?
     Where will the distribution volumes be mounted: AXP2$DKB400: [CFTPATH.ia64] \* Enter installation options you wish to use (none):
     The following products will be processed: CFT V3.x
     Beginning installation of CFT V3.x at 14:34
    ```
1. Define the installation options.
    ```
    First Transfer CFT 3x installation                  1 \* Option selected [1]:
    *Select 1 to install Transfer CFT 3x.*
    *NOTE
    : Options 2 and 3 are not available in the RA version of Transfer CFT 3.x VMS.*
    ```
1. Define the groups that can connect to Transfer CFT.  

    ****WARNING**** The following restrictions apply when using Transfer CFT in a multi-node architecture. The Transfer CFT user must be available on all machines where you are installing Transfer CFT, and have the same group number and user ID.

    ```
    For each user group that connects to Transfer CFT via CFTUTIL, COPILOT or
    Transfer CFT APIs, Transfer CFT uses 4 global sections for an approximate
    total of 10500 global pages, with each global section being shared by
    all users that are in the same group. \* Number of groups using Transfer CFT [1]:
    ```

1. Define the Transfer CFT user account. The user name entered during installation is displayed, otherwise the default value ****CFTVMS**** is used.
    ```
     \* Name of Transfer CFT account [CFTVMS]:
    Do you want to use CFTVMS as the profile? \* Do you want to update the profile [Yes]:
    ```
1. Select the transfer options.
    ```
     \* Maximum of simultaneous transfers (all networks) [32]: \* Maximum of file sub-processes [8]: \* Maximum of transfers per file sub-process [8]:
     You can select the DEC Network: \* Will CFT use the DECnet network [Yes]: \* Name of the batch queue used by CFT [SYS$BATCH]:
     This feature requires the CMKRNL privilege. \* Will procedures be executed for the transfer owner user [No]:
    ```

> **Note**
>
> Tip  
> See also Installing Transfer CFT on a single host, or Installing Transfer CFT on multiple hosts.
