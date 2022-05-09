{
    "title": "Deployment packages",
    "linkTitle": "Create a deployment package",
    "weight": "220"
}This section describes how to create a {{< TransferCFT/axwayvariablesComponentLongName  >}} deployment package for an OpenVMS environment. It details how to create a reusable and distributable Transfer CFT package to ease the task of installing and configuring Transfer CFTs on multiple servers having the same architecture.

The procedure consists of:

- Installing and generating a deployment package that is based on the configured template
    &lt;/li&gt;
- Deploying and installing the generated package

<span id="Install"></span>

Install and generate a deployment package
-----------------------------------------

To create the package:
&lt;/p&gt;

1. Install a Transfer CFT instance. This configured Transfer CFT serves as the template for the package you are about to create.
1. Create a deployment package from the template Transfer CFT.
    &lt;ol style="list-style-type: lower-alpha;"&gt;&lt;li value="1"&gt;Generate the &lt;code&gt;product_name.ANS&lt;/code&gt; file, for example &lt;code class="span_code_1"&gt;sys$update:CFT035.ANS&lt;/code&gt;.This file consists of a collection of questions and responses that display as one question per line. &lt;ul&gt;&lt;li&gt;Use the VMSINSTALL command procedure's &lt;code&gt;OPTION A&lt;/code&gt; (the auto-answer option) to start the procedure:
    ```
    $@sys$update:vmsinstal cft031 DKB0:[TRANSFER_CFT_V3_2_0_VMS-ALPHA.INSTALL] OPTIONS A
    ```
1. If the file exists in the directory, the VMSINSTALL procedure performs a new Transfer CFT installation (silent mode). If the file does not exist, it is created.
1. The `product_name.ANS` file is then created in the `sys$update` directory.
1. Reply to the `product_name.ANS` file questions. For example:

<!-- -->

1. ```
    OpenVMS AXP Software Product Installation Procedure V7.3-2
     
    It is 26-MAR-2015 at 11:06.
    Enter a question mark (?) at any time for help.
    %VMSINSTAL-W-ACTIVE, The following processes are still active:
    TCPIP$FTP_1 \* Do you want to continue anyway [NO]? y \* Are you satisfied with the backup of your system disk [YES]?
    The following products will be processed:
    CFT V3.2
    Beginning installation of CFT V3.2 at 11:06
    %VMSINSTAL-I-RECORDANS, An auto-answer file will be recorded.
    %VMSINSTAL-I-RESTORE, Restoring product save set A ...
    --------
    Warning:
    --------
    Logical names have not been detected CFTDIRINSTALL and CFTDIRRUNTIME
    To update an existing account CFT, please load the CFTLOGIN.COM
    \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#
    \#\#\# First installation of Transfer CFT ... \#\#\#
    \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#
    For each user group that connects to Transfer CFT via CFTUTIL, COPILOT, or
    Transfer CFT APIs, Transfer CFT uses 4 global sections for an
    approximate total of 10500 global pages, with each global
    section being shared by all users that are in the same group. \* Number of groups using CFT [1]:
    %CFT-I-IDENTIFIER, Test of existence COPILOT identifier in SYSUAF.
    -CFT-I-IDENTIFIER, Ignore any error messages ...
    %UAF-E-RDBADDERRU, unable to add COPILOT value [000000,000000] to rights database
    -SYSTEM-F-DUPLNAM, duplicate name
    %CFT-I-IDENTIFIER, End of test. \* Name of Transfer CFT account [CFTVMS]: CFTVM1
    Do you want to use CFTVM1 the profile? \* Do you want to update the profile [Yes]:
    Owner Username UIC Account Privs Pri Directory
    CFTVM1 [402,402] All 4 DKB0:[CFTVM1]
    %CFT-W-USEREXISTS, 9
    -CFT-W-USEREXISTS, The CFTVM1 account already exists.
    -CFT-W-USEREXISTS, The procedure will update it. \* Do you wish to continue [Yes]:
    CFT is a multi-task file transfer monitor. It supports
    the TCP/IP networks.
    To optimize file accesses, CFT can create up to eight
    sub-processes to control up to eight transfers each
    and a total of up to 64 transfers at any one time.
    Depending on your requirements, the account running the monitor
    uses specific privileges and the associated system resources. \* Maximum of simultaneous transfers (all networks) [32]: \* Maximum of file sub-processes [8]: \* Maximum of transfers per file sub-process [8]: \* Number of transfers in SNA mode [0]:
    CFT can submit command procedures on detection of specific events
    (log file full, end of transfer, transfer error and so on).
    These procedures are submitted in batch mode in the queue
    of your choice. \* Name of the batch queue used by CFT [SYS$BATCH]:
    End of transfer procedures can be executed for
    the CFT monitor or the owner of the transfer.
    This feature requires the CMKRNL privilege. \* Will procedures be executed \* for the transfer owner user [No]: y
    %CFT-I-IDENTIFIER, Test for the existence of the PSI$DECLNAME identifier
    -CFT-I-IDENTIFIER, in SYSUAF.
    -CFT-I-IDENTIFIER, Ignore any error messages ...
    %CFT-I-IDENTIFIER, End of test.
    %CFT-I-IDENTIFIER, Test for the existence of the PSI$X25_USER identifier
    -CFT-I-IDENTIFIER, in SYSUAF.
    -CFT-I-IDENTIFIER, Ignore any error messages ...
    %CFT-I-IDENTIFIER, End of test.
    %CFT-I-IDENTIFIER, Test for the existence of the NET$EXAMINE identifier
    -CFT-I-IDENTIFIER, in SYSUAF.
    -CFT-I-IDENTIFIER, Ignore any error messages ...
    %CFT-I-IDENTIFIER, End of test.
    %CFT-I-IDENTIFIER, Test for the existence of the X25_OUTGOING_ALL identifier
    -CFT-I-IDENTIFIER, in SYSUAF.
    -CFT-I-IDENTIFIER, Ignore any error messages ...
    %UAF-E-SHOWERR, unable to complete SHOW command
    -SYSTEM-F-NOSUCHID, unknown rights identifier
    %CFT-I-IDENTIFIER, End of test.
    %SEARCH-I-NOMATCHES, no strings matched
    Installation type: Single host or Multiple hosts. \* Single host(1) - Multiple Hosts(2)........... [1]:
    Single Host.
    Specify the directory where you want to install the components.
    (Default directory: login directory for the CFT account). \* Installation Directory [DKB0:[CFTVM1]]: DKB0:[CFTVM1.TEST]
    The installation directory specified is different from
    the login directory for the CFTVM1 account. \* Enable the multi-node architecture Y/N [N]: y \* Number of nodes ...................... [2]: 3
    --> Type M to modify, ENTER to continue:
    Product license key.
    Enter the product license keys, where each node requires a key per host. Alternatively, you can set the Transfer CFT keys in th
    e CFT_SCEN:CFT.KEY. \* License Key for node0 in host0: \* License Key for node1 in host0: \* License Key for node2 in host0:
    --> Type M to modify, ENTER to continue:
     
    (ALL - no longer multi node - start a new window) 
    Enter the local Transfer CFT instance details. \* Group Name .......... [Production.VMS]: \* Local Name .......... [AXP3_CFTVM1]: \* Hostname [AXP3.LAB1.LAB.PTX.AXWAY.INT]:
    --> Type M to modify, ENTER to continue:
     
     
    Sample - Default TCP/IP port (cft-tcp.conf file) \* The synchronous communication [1765]: \* The PeSITany protocol ......... [1761]: \* The PeSIT protocol using SSL .. [1762]:
    --> Type M to modify, ENTER to continue:
    Default database size \* Catalog file - nb of records.. [10000]: 1000 \* Comm. file - nb of records .... [1000]:
    --> Type M to modify, ENTER to continue:
    Transfer CFT User Interface (UI) \* Listening port ................ [1766]:
    --> Type M to modify, ENTER to continue:
    Configure the following connectors \* Sentinel Y/N? .................... [N]: \* Access Management - PassPort Y/N? [N]:
    --> Type M to modify, ENTER to continue:
    \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
    SUMMARY OF SETTINGS
    \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
    Installation type: Single host
    Hostname = AXP3.LAB1.LAB.PTX.AXWAY.INT
    Group Name = Production.VMS
    Local Name = AXP3_CFTVM1
    _________________________________________________
    Protocol PeSITany = 1761
    Protocol PeSITSSL = 1762
    Comm. Synchronous = 1765
    CFT User Interface = 1766
    _________________________________________________
    Connectivity to Composer, Sentinel, Passport or Navigator is not set. \* Modify a parameter ....... Y/N? [N]: \* Ready to install - confirm Y/N? [Y]:
    %CFT-I-COFFEEBREAK, Installation in progress. Please wait...
    %VMSINSTAL-I-RESTORE, Restoring product save set B ...
    %VMSINSTAL-I-RESTORE, Restoring product save set C ...
    %VMSINSTAL-I-RESTORE, Restoring product save set D ...
    %CFT-I-UPDATEACCOUNT, Updating the CFTVM1 account.
    [………]
    \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\* \* CFT INSTALLATION COMPLETED. \* \* You can log on to the CFTVM1 account, \* \* check that the CFTLOGIN.COM procedure was correctly executed. \*
    \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
    %VMSINSTAL-I-MOVEFILES, Files will now be moved to their target directories...
    Installation of CFT V3.6 completed at 11:10
    Adding history entry in VMI$ROOT:[SYSUPD]VMSINSTAL.HISTORY
    Creating installation data file: VMI$ROOT:[SYSUPD]CFT031.VMI_DATA
    VMSINSTAL procedure done at 11:10
     
    ```

    **Example**

    The contents of the generated `SYS$UPDATE:CFT035.ANS` file resemble the following example file.

    ```

     \* Number of groups using CFT [1]: \\ \* Name of Transfer CFT account [CFTVMS]: \\CFTVM1 \* Do you want to update the profile [Yes]: \\ \* Do you wish to continue [Yes]: \\ \* Maximum of simultaneous transfers (all networks) [32]: \\ \* Maximum of file sub-processes [8]: \\ \* Maximum of transfers per file sub-process [8]: \\ \* Number of transfers in SNA mode [0]: \\ \* Name of the batch queue used by CFT [SYS$BATCH]: \\ \* for the transfer owner user [No]: \\Y \* Single host(1) - Multiple Hosts(2)........... [1]: \\ \* Installation Directory [DKB0:[CFTVM1]]: \\DKB0:[CFTVM1.TEST] \* Confirm that you wish to install CFT in DKB0:[CFTVM1.TEST] [No]: \\Y \* Enable the multi-node architecture Y/N [N]: \\Y \* Number of nodes ...................... [2]: \\3 \* Group Name .......... [Production.VMS]: \\ \* Local Name .......... [AXP3_CFTVM1]: \\ \* Hostname [AXP3.LAB1.LAB.PTX.AXWAY.INT]: \\ \* The synchronous communication [1765]: \\ \* The PeSITany protocol ......... [1761]: \\ \* The PeSIT protocol using SSL .. [1762]: \\ \* Catalog file - nb of records.. [10000]: \\1000 \* Comm. file - nb of records .... [1000]: \\ \* Listening port ................ [1766]: \\ \* Transfer CFT Navigator Y/N? ...... [N]: \\ \* Sentinel Y/N? .................... [N]: \\ \* Composer Enabler Y/N? ............ [N]: \\ \* Access Management - PassPort Y/N? [N]: \\ \* Modify a parameter ....... Y/N? [N]: \\
    ```

Deploy and install the deployment package
-----------------------------------------

To deploy and install the package:

1. Create or import the `product_name.ANS` file in the `sys$update` directory. See [Install and generate a deployment package](#Install) for details.
1. Execute the new installation.
1. ```
    $@sys$update:vmsinstal cft035 DKB0:[TRANSFER_CFT_V3_5_VMS-ALPHA.INSTALL] OPTIONS A
    ```
