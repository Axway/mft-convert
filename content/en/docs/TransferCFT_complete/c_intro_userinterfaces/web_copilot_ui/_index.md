{
    "title": "Transfer CFT user interface",
    "linkTitle": "Transfer CFT UI ",
    "weight": "100"
}{{< TransferCFT/suitevariablesTransferCFTName  >}} features a web browser user interface that you can use to configure, track and manage transfers, and consult the  log. The following sections describe the steps you must perform before you can start using this user interface.

The Transfer CFT user interface requires secure SSL/TLS communication between the browser and the REST server, and for the REST API option to be enabled. If you performed a custom installation without these options, you must perform the following steps before connecting to the user interface.

1. [Import the Certificate Authority on the client for the browser connection.](#Connect)
1. [Configure the REST API server.](#Configur2)
1. [Connect to the user interface.](#Connect2)

Supported browsers
------------------

The {{< TransferCFT/axwayvariablesComponentLongName  >}} user interface requires a web browser. You will best experience browser-based applications such as the UI when using the latest version of one of the following browsers:

- Chrome
- Microsoft Edge
- Firefox (keyboard shortcuts are not enabled)

Prerequisites
-------------

For security purposes, you require a user certificate to secure the user interface connection. If you do not have a certificate, please see the instructions in [How to generate a chain certificate.](certificate_ui_connection)

> **Note**
>
> Note: You cannot use a self-signed certificate to secure the connection.

<span id="Connect"></span>

Import the Certificate Authority on the client
----------------------------------------------

You must import the CA that corresponds with the server certificate on the browser used for the user interface connection.

****Standalone Transfer CFT****

You must import the Certificate Authority that corresponds with the certificate to your web browser certificate store.

****If you are using Central Governance or Flow Manager****

The REST API server automatically uses the SSL business certificate generated during the registration. This certificate is stored in the internal PKI base and is identified by the Transfer CFT instance ID (UCONF `cft.instance_id` parameter). You must import the matching Certificate Authority to your web browser certificate store.

You are now ready to [connect to the user interface](#Connect2). If you encounter errors, please see the [Troubleshooting](#Troubles) section.

<span id="Configur2"></span>

Configure the REST API server
-----------------------------

{{% TransferCFT/snippets/part_1_config_api%}}
<span id="Connect2"></span>

Connect to the user interface
-----------------------------

Start the Copilot server before connecting as the REST API server.

The user that starts the Copilot server must have write permission for the Transfer CFT CFTCOM, CFTPART, CFTPARM, and CFTPKI data files.

1. Start the Copilot server: `copstart`
1. In your web browser, enter the URL using the following format: `https://<UI_server_host>:<RestApi_port>/cft/ui/`  
    Example  
    https://10.128.14.139:1768/cft/ui/
1. Enter your username and password (depending on the authentication method set on the server) where the username is limited to 32 characters.  
    The Transfer CFT interface displays. To use shortcuts, see [Keyboard shortcuts](#Keyboard). You are now ready to [configure](conf_intro) and [create flows](flow_def_intro).

<span id="Authentication_methods"></span>

Authentication methods
----------------------

{{% TransferCFT/snippets/authentication%}}

User privileges
---------------

If you or your administrator have configured access management for {{< TransferCFT/axwayvariablesComponentLongName  >}}. depending on your privileges the display may vary slightly. User rights may control items that you can view, modify, or delete.

{{% TransferCFT/snippets/bruteforce%}}

<span id="Troubles"></span>If you encounter issues when trying to connect to the user interface, please refer to [Troubleshooting the user interface](troubleshoot_ui).

<span id="Keyboard"></span>

Keyboard shortcuts
------------------

Keyboard shortcuts provide a way to navigate the user interface from the keyboard. Begin by using **ALT + H** to display the shortcuts. You can then move through the UI, using ALT + the object abbreviation key (e.g. **Alt + T** for transfers) as displayed in the UI.

![](/Images/TransferCFT/new_ui_shortcuts.png)

- **ALT** + **H**: Displays available shortcuts
- From the **New** [Transfer] page, for example, press Enter to begin a new request.
- To navigate across the page using keyboard shortcuts, use Tab to move to the next field, or Shift + Tab to go back.

> **Note**
>
> Note: Keyboard shortcuts on Firefox are largely nonfunctional.

Browse system to find a file
----------------------------

From the user interface, you can browse the remote {{< TransferCFT/axwayvariablesComponentLongName  >}} file system to select a file to send or to execute as a processing script. Use the folder icon located next to the field name, for example `fname`, to locate and select a file.

Additionally, you can add or modify a root directory to the directories available for authorized users to browse.

1. In the user interface, access the **Unified Configuration** page.
1. Search for the `copilot.rootdir` parameter and in the **Value** column enter the new directory name. You can use a logical name appended to the default runtime directory, being sure to enter a space to separate the runtime directory from the logical folder name. For example, set `copilot.rootdirs` to `runtime My_New_Directory`.

    ![](/Images/TransferCFT/rootdirs.png)

1. Click **OK** to confirm. This creates a new UCONF root directory parameter having that name.
1. In the newly created root directory, click `##INVALID## `in the **Value** field and enter the full path to the new directory.  
      
    ![](/Images/TransferCFT/rootdirs3.png)  
    For example, enter `C:\MyNewDirectory`.
1. Click **OK** to confirm. The resulting directory displays as follows:  
    ![](/Images/TransferCFT/rootdirs2.png)  

Limitations
-----------

### UNIX LDAP user

You cannot use an LDAP user to connect to the web-based user interface.

### Keyboard shortcut limitation

When tabbing through the UI using keyboard shortcuts, if you tab to a field in a collapsed section that has a drop-down box and you use the space bar, the field's drop-down menu displays even though the section remains collapsed.

![](/Images/TransferCFT/UI_LIMITATION.png)
