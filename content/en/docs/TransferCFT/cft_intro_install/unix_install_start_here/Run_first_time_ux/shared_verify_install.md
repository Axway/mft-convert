---
title: "Post installation"
linkTitle: "Post installation"
weight: 170
---## Verify your installation

You can check the installation log in the &lt;span class="code">&lt;code>&lt;installation directory>/install.log&lt;/code>&lt;/span> file. See the installation&lt;madcap:conditionaltext data-mc-conditions="axway_conditions.ScreenOnly"> &lt;a href="../../troubleshoot_registration">troubleshooting section&lt;/a>&lt;/madcap:conditionaltext> if you encounter problems with starting &lt;span class="mc-variable axway_variables.Component_Long_Name variable">Transfer CFT&lt;/span> or registering with &lt;span class="mc-variable suite_variables.Central_GovernanceName variable">Central Governance&lt;/span>.
&lt;/p>

### Installed directories

When you install Transfer CFT, the `home` directory is created and populated under the `Transfer_CFT` installation directory. This `home `directory contains installation libraries, binaries, and templates. Do not store any personal files in the `home `directory, as they are erased during updates.

### Installer-generated files

During the installation, the Transfer CFT installer creates two files in the installation directory that are working files for the installer. Do not modify these files unless instructed to do so by Axway. While they are not used for Transfer CFT operations, they are necessary for installer functions such as upgrades.

- .rundir
- .transfer_cft.properties

## Standalone installations

If you are not using {{< TransferCFT/suitevariablesTransferCFTName  >}} with {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, you must provide a certificate to be able to use the {{< TransferCFT/suitevariablesTransferCFTName  >}} UI. . See [Using the web-based browser UI](../../../../c_intro_userinterfaces/web_copilot_ui#Connect2) page for details.

## Register with {{< TransferCFT/suitevariablesCentralGovernanceName  >}}

Begin your registration with {{< TransferCFT/suitevariablesCentralGovernanceName  >}} by starting the Copilot server, which launches the registration process.

### Start the Transfer CFT Copilot server

To set the environment variables:

1. Navigate  to the `runtime `directory.
1. From  the runtime directory prompt, enter:  
    ```
    . ./profile
    ```
1. To start the Copilot server, run the command:
    ```
    copstart
    ```

<span id="Verify"></span>

## Verify the Transfer CFT registration with {{< TransferCFT/suitevariablesCentralGovernanceName  >}}

### Log on {{< TransferCFT/suitevariablesCentralGovernanceName  >}}

If you have not already done so, log on {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.

In {{< TransferCFT/suitevariablesCentralGovernanceName  >}} from the **Product** page, check the Product List for your installed {{< TransferCFT/axwayvariablesComponentLongName  >}}.

See the Troubleshooting installation section in the {{< TransferCFT/axwayvariablesComponentLongName  >}} {{< TransferCFT/suitevariablesDocTypeUser  >}} for tips in case of an error.

### Start the Transfer CFT server

To start Transfer CFT from the {{< TransferCFT/suitevariablesCentralGovernanceName  >}} interface, use the following procedure.

1. Click **Products** on the top toolbar to open the page.
1. Select the product (Transfer CFT) to start.
1. Click Start. When started successfully, the Status column displays **Started**.

### View the log using {{< TransferCFT/PrimaryCGorUM  >}}

1\. Click the name of the Transfer CFT system on the **Product List** page to open its details page.

2\. Click Logs on the right side of the page.

The log page is displayed where you can:

- Click Refresh anytime to update the log entries.
- Sort the entries by newest or oldest.
- Filter the entries, saving filters for future use.

> **Note**
>
> For details on starting, stopping and viewing the Transfer CFT refer to the Central Governance User Guide.

## Register with {{< TransferCFT/PrimaryCGorUM  >}}

If you intend to implement {{< TransferCFT/PrimaryCGorUM  >}}, please refer to the {{< TransferCFT/axwayvariablesComponentLongName  >}} *User's Guide &gt; [*Register with* {{< TransferCFT/PrimaryCGorUM  >}}](https://docs.axway.com/bundle/TransferCFT_36_UsersGuide_allOS_en_HTML5/page/Content/cft_installation/migrate/register_CG.htm)* page for registration details.
