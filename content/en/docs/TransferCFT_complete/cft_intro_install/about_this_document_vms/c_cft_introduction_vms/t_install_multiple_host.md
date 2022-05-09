{
    "title": "Installing Transfer CFT on multiple hosts",
    "linkTitle": "Install Transfer CFT on multiple hosts",
    "weight": "210"
}Installing Transfer CFT on multiple hosts
-----------------------------------------

Installing a multi-host Transfer CFT architecture requires additional prerequisites and installation steps beyond those described in the single host installation procedure.

### Step overview

1. Install the first machine using the [****First host**** installation](#host_installation_step) option.
    -   For the first host installation, you should enter all hosts for the installation architecture.

    <!-- -->

    -   You must at this point enter the shared directory that will be used by all of the hosts and machines in the setup.

    <!-- -->

    -   This first installation saves all of the initial information for the additional machine installations.

1. Install all additional machines using the [****Additional host**** installation](#host_installation_step) option, for example the second and third machine.
    -   Do ****not**** select ****First host**** for an additional host installation.

    <!-- -->

    -   You are prompted to enter the shared directory location, as defined in the previous step.

### Prerequisites

To install Transfer CFT OpenVMS in a cluster you require:

- Multiple machines on which each of these the Transfer CFT binaries can be installed in the installation directory path.
- A shared directory that is available for all of the machines where the Transfer CFT runtime will be installed and shared.

### Procedure

To begin the procedure:

1. Select ****Option 2**** to install Transfer CFT on multiple hosts.
    ```
    Installation type: Single host or Multiple hosts. \* Single host(1) – Multiple Hosts(2)........... [1]: 2
    ```
1. Select the option that corresponds with your installation. <span id="host_installation_step"></span>You can define either the first host in this screen, or additional host installations according to the planned architecture.
    ```
    First host: Installs Transfer CFT on the first Host............ (1)
    Additional host: Installs Transfer CFT on an additional Host (2) \* Selected option [1]:
    ```
1. Specify the root and shared directories for components to install. The default directory is the login directory for the Transfer CFT account.
    &lt;ul&gt;&lt;li&gt;Installation directory: Path of the first host&lt;/li&gt;&lt;li&gt;Shared Directory: Shared Disk path, the shared disk is shared by all installed hosts&lt;/li&gt;&lt;li&gt;Runtime Directory: The Transfer CFT runtime will be installed in the shared folder&lt;/li&gt;&lt;/ul&gt;
    ```
    First host. \* Installation Directory [AXP2$DKB400:[CFTPATH]]: \* Shared Directory [AXP2$DKB400:[ CFTPATH.SHARED]]: \* Runtime Directory AXP2$DKB400:[ CFTPATH.SHARED.cft.runtime]
    --> Type M to modify, ENTER to continue:
    ```
1. Enter the local Transfer CFT instance details.
    -   Group name: Use this field to assign your Transfer CFT to a Transfer CFT group. This value is used for Transfer CFT Navigator and Composer, if your setup includes these additional components.
    -   Local name: This value is your CFTNAME, as defined by LOCALPART in CFTPARM by default. See also the UCONF values.

    ```

     \* Group Name .......... [Production.VMS]: \* Local Name .......... [I64OV1_MYCFTNAME]: \* Hostname [I64OV1.LAB1.LAB.PTX.AXWAY.INT]:
    --> Type M to modify, ENTER to continue:
    ```
1. Enable the multi-nodes architecture. Then enter the number of nodes in the setup and the hostnames.
    ```
     \* Number of nodes ..... [2]: (1 to 4)
    Specify hosts that take part of the multi-node architecture. \* Number of hosts ..... [2]: (1 to 4) \* Hostname 0 _______________ HOSTNANE_N°1 \* Host address 0 ___________ HOSTNAME_N°1.LONG.ADDRESS.COM \* Hostname 1 ..............: HOSTNANE_N°2 \* Host address 1 ..........: HOSTNAME_N°2.LONG.ADDRESS.COM
    --> Type M to modify, ENTER to continue:
    ```
1. Enter the product license keys, where each node requires a key per host. Alternatively, you can set the Transfer CFT key in the CFT_SCEN:CFT.KEY file after installation.
    ```
    Product license key.
    Enter the product license keys, where each node requires a key per host. Alternatively, you can set the Transfer CFT key in CFT_SCEN:CFT.KEY: \* License Key for host1:
    <Host1 LicenseKey> \* License Key for host2:
    <Host2 LicenseKey> \* License Key for host3:
    <Host3 LicenseKey> \* License Key for node1 in host4:
    <Host4 LicenseKey>
    --> Type M to modify, ENTER to continue:
    ```
1. For the sample default TCP/IP `cft-tcp.conf `file, enter the ports as needed.
    ```
     \* The synchronous communication [1765]: \* The PeSITany protocol ......... [1761]: \* The PeSIT protocol using SSL .. [1762]:
    --> Type M to modify, ENTER to continue:
    ```
1. Define the default internal datafile size for the catalog and communication files.
    ```
     \* Catalog file - nb of records.. [10000]: \* Comm. file - nb of records .... [1000]:
    --> Type M to modify, ENTER to continue:
    ```
1. Enter the listening port for the Transfer CFT User Interface (UI).
    ```
     \* Listening port ................ [1766]:
    --> Type M to modify, ENTER to continue:
    ```
10. Enter the listening port for the Transfer CFT User Interface (UI).
    ```
     \* Listening port ................ [1766]:
    --> Type M to modify, ENTER to continue:
    ```
11. Configure the Transfer CFT connectors according to your system setup.
    ```
     \* Sentinel Y/N? .................... [N]: \* Access Management - PassPort Y/N? [N]:
    --> Type M to modify, ENTER to continue:
    ```

After completing the installation process, go to [Starting Transfer CFT]().
