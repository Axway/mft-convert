{
    "title": "Installing Transfer CFT on a single host",
    "linkTitle": "Install Transfer CFT on a single host",
    "weight": "210"
}This section describes how to install Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} on a single host. As described in this section, you may opt to run multiple instances of Transfer CFT on a single host to optimize the hardware architecture, for example.

To begin the procedure:

1. Select the single host option to perform a classic Transfer CFT installation:
    ```
    Installation type: Single host or multiple hosts. \* Single host(1) - Multiple hosts (2)........... [ 1
    ]:
    ```
1. Specify the root and shared directories for components to install. The default directory is the login directory for the Transfer CFT account.
    ```
    Single Host.
    Specify the root and shared directories for components to install. \* Installation Directory [AXP2$DKB400:[MYDIRPATH]]: AXP2$DKB400:[ MYDIRPATH]
    ```
1. Set the multi-node option. If you want to install Transfer CFT on a single host but run multiple instances of Transfer CFT, set the multi-node option to (Y)es. If you activate multi-node, you are prompted to enter the number of Transfer CFT nodes, from 2 to 4 nodes.
    ```
     \* Enable the multi-node architecture Y/N [N]
    :
    --> Type M to modify, ENTER to continue:
    ```
1. Enter the product license key. Alternatively, you can set the Transfer CFT key in the CFT_SCEN:CFT.KEY file after installation.
    ```
    Product license key. \* Enter the license key. Alternatively, you can set the Transfer CFT key in CFT_SCEN:CFT.KEY:
    <ENTER your LICENSE KEY>
    --> Type M to modify, ENTER to continue:
    ```
1. Enter the local Transfer CFT instance details.
    -   Group name: Use this field to assign your Transfer CFT to a Transfer CFT group.
    -   Local name: This value is your CFTNAME, as defined by LOCALPART in CFTPARM by default. See also the UCONF values.

    ```
    Local Transfer CFT instance details. \* Group Name .......... [Production.VMS]: \* Local Name .......... [I64OV1_MYCFTNAME]: \* Hostname [I64OV1.LAB1.LAB.PTX.AXWAY.INT]:
    --> Type M to modify, ENTER to continue:
    ```
1. For the sample default TCP/IP `cft-tcp.conf `file, enter the ports as needed.
    ```
     \* The synchronous communication [1765]: \* The PeSITany protocol ......... [1761]: \* The PeSIT protocol using SSL .. [1762]:
    --> Type M to modify, ENTER to continue:
    ```
1. Define the default database size for the catalog and communication files.
    ```
     \* Catalog file - nb of records.. [10000]: \* Comm. file - nb of records .... [1000]:
    --> Type M to modify, ENTER to continue:
    ```
1. Enter the listening port for the Transfer CFT User Interface (UI).
    ```
     \* Listening port ................ [1766]:
    --> Type M to modify, ENTER to continue:
    ```
1. Configure the Transfer CFT connectors according to your system setup.
    ```
     \* Sentinel Y/N? .................... [N]: \* Access Management - PassPort Y/N? [N]:
    --> Type M to modify, ENTER to continue:
    ```

After completing the installation process, go to [Starting Transfer CFT]().
