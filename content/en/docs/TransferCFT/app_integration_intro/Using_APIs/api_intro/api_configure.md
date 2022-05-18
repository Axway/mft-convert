---
title: "REST API server configuration"
linkTitle: "REST API server configuration"
weight: 300
--- Before you can start using REST API operations with {{< TransferCFT/suitevariablesTransferCFTName  >}}, you need to set a few parameters in the {{< TransferCFT/suitevariablesTransferCFTName  >}} configuration.

## Before you start

The REST server is a Copilot service. To start the REST server, use the `copstart `command to [start Copilot](../../../../admin_intro/manage_copilot).

## Procedure

{{< TransferCFT/suitevariablesTransferCFTName  >}} requires the following configuration settings before you can use REST API.

{{% TransferCFT/snippets/part_1_config_api%}}

1. Enable the Copilot REST API if you did not do so during installation.  
    ```
    CFTUTIL uconfset id=copilot.restapi.enable, value=yes
    ```

1. Optionally, you can change the REST API server port as follows (default 1768):  
    ```
    CFTUTIL uconfset id=copilot.restapi.serverport, value=<new port>
    ```

1. You require a secure SSL/TLS communication between the client (REST or browser) and the REST server. When using Central Governance, the REST API server automatically uses the SSL business certificate generated during the [registration](../../../../governance_services_intro/cg_register_overview); there is no need to perform this step. This certificate is stored in the internal PKI base and is identified by the Transfer CFT instance ID (uconf:cft.instance_id).  
    Otherwise, use UCONF to set the following Copilot parameters to configure the SSL certificate.  
    ```
    CFTUTIL uconfset id=copilot.ssl.SslCertFile, value=<ssl pkcs12 certificate for copilot>
    CFTUTIL uconfset id=copilot.ssl.SslCertPassword, value=<ssl pkcs12 certificate password>
    ```

    These parameter settings are described in [Install a certificate on the server side](../../../../admin_intro/manage_copilot#Install).  

1. Specify the authentication method, as the client must provide credentials (user/password) to the REST server. Set the UCONF the `copilot.restapi.authentication_method` parameter.  
    ****Example****  
    ```
    CFTUTIL uconfset id=copilot.restapi.authentication_method, value=system
    ```

    See the [Authentication methods](../../../../c_intro_userinterfaces/web_copilot_ui#Authentication_methods) for option details.

The supported authentication methods are:

| Authentication method  | copilot.restapi.authentication_method  | Details  |
| --- | --- | --- |
| Operating System  | system  | The user/password is checked against the operating system.<br/> <blockquote> **Note**<br/> We strongly recommend that you set copilot.misc.createprocessasuser=yes when using the system option.<br/> </blockquote> **Unix**<br/> You must use <code>cftsu </code>to create users as a superuser is required (sudo or root privilege) to create a group and assign a user to a group. Refer to [Using system users - UNIX](../../../../cft_intro_install/unix_install_start_here/run_first_time_ux/run_first_time_ux/t_adding_system_user_unix) for details.<br/> • Create a group "group1": groupadd group1<br/> • Add user "user1" to group "group1": usermod - a - G group1 user1<br/> **Windows**<br/> You require a superuser (administrative user account) to create a group and assign a user to a group.<br/> • Create a group "group1": net localgroup group1 /add<br/> • Add user "user1" to group "group1": net localgroup group1 user1 /add<br/> <blockquote> **Note**<br/> For a user belonging to a domain, use: domain\user1 instead of user1<br/> </blockquote>  |
| Access Management  | am  | This methods uses an indirection towards the Access Management system. The user/password is checked by the configured access management system: {{< TransferCFT/suitevariablesFlowManager  >}}, PassPort AM, or internal AM. |
| xfbadm database<br/> (UNIX and HP NonStop exclusively) | xfbadm  | The user/password is checked using the xfbadm base (see the [xfbadmusr and xfbadmgrp utilities](../../../../cft_intro_install/unix_install_start_here/run_first_time_ux/use_cft_utilities)).<br/> A user that can execute xfbadmusr/xfbadmgrp utilities can create users and groups after executing the <code>profile </code>from the runtime directory.<br/> • Create a group "group1" with gid=200: xfbadmgrp add - G group1 - p group1_pw - g 200<br/> • From the user prompt, to add a user "user1" to group "group1"enter: xfbadmusr add - l user1 - p user1_pw - u AUTO - g 200 |

********<span id="REST"></span>REST API server authentication method********

********![](/Images/TransferCFT/authentication_copilot_server.png)********

> **Note**
>
> 1\. If copilot.restapi.authentication_method = system, then your access management type must be set to either am.type= none, or both am.type=internal and am.internal.group_database = system.

> **Note**
>
> 2\. If copilot.restapi.authentication_method = xbfadm, then your access management type must be set to either am.type= none, or both am.type=internal and am.internal.group_database = xbfadm.

| Parameter  | Type  | Default  | Description  |
| --- | --- | --- | --- |
| copilot.restapi.enable  | bool  | No  | Enable/disable the REST API service:<br/> • Yes: enable<br/> • No: disable |
| copilot.restapi.serverport  | int  | 1768  | REST API server port. |
| copilot.restapi.authentication_method  | string  | system (Windows)<br/> xfbadm (UNIX) | Defines authentication method.<br/> <br/> See also, [xfbadmusr utilitiy.](../../../../cft_intro_install/unix_install_start_here/run_first_time_ux/use_cft_utilities#xfbadmusr1) |
| copilot.restapi.nb_workers  | int  | 4  | Number of activated workers that process the REST API requests.  |
| copilot.restapi.maxclient  | int  | 256  | Number of client connections handled per REST worker.  |
| copilot.restapi.coms_id  | string  | coms  | The TCPIP CFTCOM object identifier used by the REST API server to communicate with the Transfer CFT server.<br/> Leave empty to use the COM file instead. |
| copilot.restapi.catalog.retry_delay  | int  | 5  |  • The delay between retries in seconds. The Copilot server checks the request status in catalog every retry_delay seconds.<br/> • The delay between retries in seconds. The Copilot server checks the request status in catalog every retry_delay seconds. |
| copilot.restapi.catalog.retry_timeout  | int  | 30  | The default value of the apiTimeout parameter as defined in the request URL.<br/> Available exclusively for POST requests. |

