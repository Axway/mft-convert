---

    title: Transfer CFT user interface
    linkTitle: Transfer CFT UI 
    weight: 110

---
{{< TransferCFT/suitevariablesTransferCFTName  >}} features a web browser user interface that you can use to configure, track and manage transfers, and consult the  log. The following sections describe the steps you must perform before you can start using this user interface.

The Transfer CFT user interface requires secure SSL/TLS communication between the browser and the REST server, and for the REST API option to be enabled. If you performed a custom installation without these options, you must perform the following steps before connecting to the user interface.

1. [Configure the REST API server.](#Configur2)
1. [Set the Certificate Authority on the client side.](#Connect)

## Supported browsers

The user interface requires a web browser such as:

- Google Chrome
- Microsoft Edge version 41
- Firefox (without keyboard shortcuts)

If there is a specified version, it is the minimum version for that browser.

<span id="Connect2"></span>

## Connect to the user interface

Start the Copilot server before connecting as the REST API server.

The user that starts the Copilot server must have write permission for the Transfer CFT CFTCOM, CFTPART, CFTPARM, and CFTPKI data files.

1. Start the Copilot server: <span class="code">`copstart`</span>
1. In your web browser, enter the URL using the following format: <span class="code">`https://<UI_server_host>:<RestApi_port>/cft/ui/`</span>  
    Example  
    https://10.128.14.139:1768/cft/ui/
1. Enter your username and password (depending on the authentication method set on the server) where the username is limited to 32 characters.  
    The Transfer CFT interface displays. To use shortcuts, see [Keyboard shortcuts](#Keyboard). You are now ready to [configure](conf_intro) and [create flows](flow_def_intro).

<span id="Configur2"></span>

## Configure the REST API server

1. Enable the Copilot REST API if you did not do so during installation.  
    ```
    CFTUTIL uconfset id=copilot.restapi.enable, value=yes
    ```
1. Optionally, you can change the REST API server port as follows (default 1768):  
    ```
    CFTUTIL uconfset id=copilot.restapi.serverport, value=<new port>
    ```
1. You require a secure SSL/TLS communication between the client (REST or browser) and the REST server. When using Central Governance, the REST API server automatically uses the SSL business certificate generated during the [registration](../../governance_services_intro/cg_register_overview); there is no need to perform this step. This certificate is stored in the internal PKI base and is identified by the Transfer CFT instance ID (uconf:cft.instance\_id).  
    Otherwise, use UCONF to set the following Copilot parameters to configure the SSL certificate.  
    ```
    CFTUTIL uconfset id=copilot.ssl.SslCertFile, value=<ssl pkcs12 certificate for copilot>
    CFTUTIL uconfset id=copilot.ssl.SslCertPassword, value=<ssl pkcs12 certificate password>
    ```  
    These parameter settings are described in [Install a certificate on the server side](../../admin_intro/manage_copilot#Install).  
1. Specify the authentication method, as the client must provide credentials (user/password) to the REST server. Set the UCONF the <span class="code">`copilot.restapi.authentication_method`</span> parameter.  
    Example  
    ```
    CFTUTIL uconfset id=copilot.restapi.authentication_method, value=system
    ```

<span class="bold_in_para">****<span id="Authentication_methods"></span>Authentication methods****</span>

The supported authentication methods are:


| Authentication method  | copilot.restapi.authentication_method  | Details  |
| --- | --- | --- |
| Operating System  | system  | The user/password is checked against the operating system.<br/> <blockquote> **Note**<br/> We strongly recommend that you set copilot.misc.createprocessasuser=yes when using the system option.<br/> </blockquote> **Unix**<br/> You must use <span ><code>cftsu </code></span>to create users as a superuser is required (sudo or root privilege) to create a group and assign a user to a group. Refer to <a href="" >Using system users - UNIX</a> for details.<br/> • Create a group "group1": <span >groupadd group1</span><br/> • Add user "user1" to group "group1": <span >usermod -a -G group1 user1</span><br/> **Windows**<br/> You require a superuser (administrative user account) to create a group and assign a user to a group.<br/> • Create a group "group1": <span >net localgroup group1 /add</span><br/> • Add user "user1" to group "group1": <span >net localgroup group1 user1 /add</span><br/> <blockquote> **Note**<br/> For a user belonging to a domain, use: domain\user1 instead of user1<br/> </blockquote>  |
| Access Management  | am  | This methods uses an indirection towards the Access Management system. The user/password is checked by the configured access management system: {{< TransferCFT/suitevariablesFlowManager  >}}, PassPort AM, or internal AM. |
| xfbadm database<br/> (UNIX and HP NonStop exclusively) | xfbadm  | The user/password is checked using the xfbadm base (see the <a href="../../cft_intro_install/unix_install_start_here/run_first_time_ux/use_cft_utilities">xfbadmusr and xfbadmgrp utilities</a>).<br/> A user that can execute xfbadmusr/xfbadmgrp utilities can create users and groups after executing the <span ><code>profile </code></span>from the runtime directory.<br/> • Create a group "group1" with gid=200:<span > xfbadmgrp add -G group1 -p group1_pw -g 200</span><br/> • From the user prompt, to add a user "user1" to group "group1"enter: <span >xfbadmusr add -l user1 -p user1_pw -u AUTO -g 200</span> |


********<span class="autonumber"></span><span id="REST"></span>REST API server authentication method********

********<span class="autonumber"></span>
![](/Images/TransferCFT/authentication_copilot_server.png)********

> **Note**
>
> 1\. If copilot.restapi.authentication\_method = system, then your access management type must be set to either am.type= none, or both am.type=internal and am.internal.group\_database = system.

> **Note**
>
> 2\. If copilot.restapi.authentication\_method = xbfadm, then your access management type must be set to either am.type= none, or both am.type=internal and am.internal.group\_database = xbfadm.

 

<span id="Connect"></span>

## Set the Certificate Authority on the client side

For security purposes, you must import the CA that corresponds with the server side certificate.

****When using Central Governance****

The REST API server automatically uses the SSL business certificate generated during the registration. This certificate is stored in the internal PKI base and is identified by the Transfer CFT instance ID (UCONF <span class="code">`cft.instance_id`</span> parameter). You must import the matching Certificate Authority to your web browser certificate store.

****When you are not using {{< TransferCFT/PrimaryCGorUM  >}}****

You must import the Certificate Authority that corresponds with the certificate that you defined previously (Step 3) to your web browser certificate store.

You are now ready to [connect to the user interface](#Connect2). If you encounter errors, please see the [Troubleshooting](#Troubles) section.

## Limit the number of failed login attempts

Transfer CFT provides brute force protection for logging on the {{< TransferCFT/suitevariablesTransferCFTName  >}} UI, REST API, or Web Services when using either the *system* mode or *xfbadm* mode (UNIX and HP NonStop only) authentication. That is, it limits the number of login failure attempts, where both the user and the password are checked to avoid brute force attacks.

For other authentication methods, such as PassPort and LDAP, no check is made. You must manage that in the Password Policy of those external tools.

You can use the following UCONF parameters to manage this option:

- `copilot.general.login_failures_fname`: A file that stores data shared between Transfer CFT and Copilot.
- `copilot.general.max_login_failures`: An integer that sets the maximum number of login failures for a user (default is 3, and 0 disables this option).

> **Note**
>
> In a multi-host environment, an attacker may have up to the copilot.general.max\_login\_failures \* &lt;number of host> tries before the user is locked if the file is not in a directory shared by all hosts.

When the maximum number of login failures is reached, the user account is locked for 30 seconds.

****Platform specifics****

- On IBM i systems, there is no action if the password is incorrect as the system offers methods that you can rely on to avoid brute force attacks (the system value is [QMAXSIGN](https://www.ibm.com/support/knowledgecenter/ssw_ibm_i_74/rzarl/rzarlmaxsgn.htm)).
- On z/OS systems, only the inherent system protection is available (refer to the RACF suboperand [REVOKE](https://www.ibm.com/support/knowledgecenter/SSLTBW_2.3.0/com.ibm.zos.v2r3.icha700/setrpw.htm) for the PASSWORD option).

<span id="Troubles"></span>If you encounter issues when trying to connect to the user interface, please refer to [Troubleshooting the user interface](troubleshoot_ui).

<span id="Keyboard"></span>

## Keyboard shortcuts

Keyboard shortcuts provide a way to navigate the user interface from the keyboard. Begin by using **ALT + H** to display the shortcuts. You can then move through the UI, using ALT + the object abbreviation key (e.g. **Alt + T** for transfers) as displayed in the UI.

![](/Images/TransferCFT/new_ui_shortcuts.png)

- **ALT** + **H**: Displays available shortcuts
- From the **New** \[Transfer\] page, for example, press Enter to begin a new request.
- To navigate across the page using keyboard shortcuts, use Tab to move to the next field, or Shift + Tab to go back.

> **Note**
>
> Keyboard shortcuts on Firefox are largely nonfunctional.

## Limitations

### Keyboard shortcut limitation

When tabbing through the UI using keyboard shortcuts, if you tab to a field in a collapsed section that has a drop-down box and you use the space bar, the field's drop-down menu displays even though the section remains collapsed.

![](/Images/TransferCFT/UI_LIMITATION.png)
