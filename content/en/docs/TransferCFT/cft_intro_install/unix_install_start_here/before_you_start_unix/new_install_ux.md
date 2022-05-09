{
    "title": "Customize the initialize.properties file",
    "linkTitle": "3. Customize the initialize.properties file",
    "weight": "150"
}A common practice is to create a copy of the `initialize.properties ` file, which is located in the downloaded installation package. This gives you an initial intact version should you later need it.

Customize the `initialize.properties ` file. Use the table below to help you with parameter settings; note that the `CryptoKey_Password` is mandatory. Be sure that if you want to use special characters in a configuration file field, you protect the value by enclosing it in double quotation marks ("").

{{% TransferCFT/snippets/loaded_profile_initialize%}}

****Example****

To use the \# character in a value, for example, protect the entire string using quotation marks "" as follows:

`CryptoKey_Password = "Aedft#439"`

If you do not enclose this value in "", the string is interpreted as: `CryptoKey_Password = Aedft`

> **Note**
>
> Note: Some parameters can be calculated during the installation (flagged by @automatic); you can leave these fields blank.

> **Note**
>
> Note: Parameters that have default values are flagged by @default.

****Silent installation parameters****


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| Installation architecture  | @default = single  | Defines single or cluster mode installation.<br/> Values: single, first_host, additional_host | N/A  |
| installdir  |   | Transfer CFT installation directory.  | cft.install_dir  |
| accept_general_conditions  | NO  | Set to YES to accept the General Terms and Conditions in the product [license](https://www.axway.com/en/legal/contract-documents) when performing a silent installation.  |   |


****Basic installation parameters****


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| runtimeDir  | ./runtime  | Transfer CFT runtime directory.<br/> Specify the directory where you want to install the Transfer CFT runtime directory.<br/> By default, the runtime directory is installed in a sub-directory of the Transfer CFT installation directory. Use the default directory, or specify a new directory. A runtime directory will be created if it does not already exist. | cft.runtime_dir  |
| Full_Hostname | @automatic  | Host Address of the local server: FQDN (Fully Qualified Domain Name) or IP Address. See **Note*** | cft.full_hostname  |
| multinode_hostname  | @automatic  | When not defined, this field is filled with the hostname of the machine where you are installing {{< TransferCFT/suitevariablesTransferCFTName  >}}, whether it is the first host or an additional host.<br/> If the hostname contains a &quot;.&quot; period, the value used consists of the name of the host preceding the first period. For example, &quot;myhost.fqdn.net&quot; would be shortened to &quot;myhost&quot;. | cft.multi_node.hostnames  |
| multinode_host_address  | @automatic  | If you do not specify a value, the machine's FQDN address is used.<br/> Note that if there is an error in the machine's configuration, the value taken could be incorrect. Be sure to check that you can ping the address, and that it is the value for the cluster network. | cft.multi_node.hostnames.&lt;hostname&gt;.host  |
| Instance_ID | @default  | The maximum length is 24.  | cft.instance_id  |
| Instance_Group  |   | Transfer CFT instance group.<br/> The maximum length is 1000. | cft.instance_group  |


> **Note**
>
> Note: \*This host address defines:

- The unconf `sentinel.trkproductipaddr `parameter, which is the host address that identifies this host
- The host address used to connect this Transfer CFT Copilot server

****Security configuration parameters****


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| CryptoKey_Password | @mandatory  | Seed password to generate the encryption key.<br/> The password must contains at least 8 characters, contain upper and<br/> lower case characters as well as numeric and special characters (*$!?+-@). | N/A  |
| CryptoKey_Key_File | @default = $CFTDIRRUNTIME/data/crypto/crypkey  | Location that stores the generated key file.  | crypto.key_fname  |
| CryptoKey_Salt_File | @default = $CFTDIRRUNTIME/data/crypto/crypsalt  | Location that stores the generated salt file.  | crypto.salt_fname  |


****Runtime configuration parameters****


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| Key |   | Enter the license key for the Transfer CFT product.<br/> The key is stored in the $CFTDIRRUNTIME/conf/cft.key.<br/> *Without the key you can install, but not start the product.* |   |
| Catalog_File_Size  | @default= 10000  | Sets the default catalog size. | cft.cftcat.default_size  |
| Communication_File_Size  | @default = 1000  | Sets the default communication file size.  | cft.cftcom.default_size  |
| PESIT_Port  | @default = 1761  | The port number of the PeSiT protocol.<br/>  | samples.pesitany_sap.value  |
| PESIT_SSL_Port  | @default = 1762  | The port number of the PeSIT protocol using SSL.  | samples.pesitssl_sap.value  |
| COMS_Port  | @default = 1765  | The port number of the COMS synchronous communication media.<br/>  | samples.coms_port.value  |
| Copilot_Port  | @default = 1766  | Sets the port number for the Transfer CFT Copilot server that listens for<br/> incoming unsecured and secured (SSL) connections.<br/> The same port number is used for the Graphical User Interface and Web Services<br/> with or without SSL. | copilot.general.serverport  |
| Copilot_OnlySSL  | @default = Yes  | Copilot only allows SSL connection.<br/> This value applies to the old Transfer CFT UI, Webservices and JPI.<br/> If you set this to Yes, it disables the use of the HTTP server when an SSL context is defined (HTTPS only). | copilot.http.onlyssl  |
| Copilot_SSL_Port  | @default = 1767  | Sets the port number for the Transfer CFT Copilot server that listens for incoming secured (SSL with mutual authentication) connections. Only used by the Governance.  | copilot.general.ssl_serverport  |
| UI_DefaultUser_Username<br/> UI_DefaultUser_Password |   | *UNIX systems only*<br/> The default Transfer CFT UI user/password. | N/A  |
| RESTAPI_Enable<br/> <br/>  | @default = Yes  | Setting this to Yes activates the Transfer CFT REST API Server. | copilot.restapi.enable  |
| RESTAPI_Port  | 1768  | The port number used to connect to the REST API server. | copilot.restapi.serverport  |
| RESTAPI_Certificate_Path<br/> RESTAPI_Cert_Pass |   | Sets the certificate and the corresponding password to be used by the Transfer<br/> CFT REST API Server and for HTTPS connections with Copilot (the old Transfer CFT UI, Web services, and JPI).<br/> • When REST API is enabled, you must complete these fields.<br/> • When Copilot_OnlySSL is activated, you must complete these fields.<br/> When using Central Governance, the REST API server automatically uses the SSL business certificate generated during the registration; there is no need to pre-define this value if you are going to register Transfer CFT with Central Governance. | copilot.ssl.SslCertFile  |


****Multi-node and Cluster parameters****


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| Multinode_Enable  | @default = No  | Enable the multi-node architecture.<br/> To use a multi-node architecture, you must define the multi-node option in the initialize.properties file. | cft.multi_node.enable  |
| Multinode_Number  | @default = 2  | Enter the number of nodes.  | cft.multi_node.nodes  |
| LoadBalancer_Host  |   | Specify the host address of the load balancer.<br/> When using an ACTIVE/ACTIVE or ACTIVE/PASSIVE deployment, you require a load balancer to connect to the Transfer CFT Copilot server. | cft.multi_node.load_balancer.host  |
| LoadBalancer_Port  |   | Specify the load balancer port, which is redirected to the Central Governance dedicated port of the Transfer CFT UI Server. When using ACTIVE/ACTIVE or ACTIVE/PASSIVE deployment, you require a load balancer to connect to the Transfer CFT Copilot server. | cft.multi_node.load_balancer.port  |


****Central Governance parameters****


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| CG_Enable | @default = No | Enter Yes to enable Central Governance connectivity.  | cg.enable  |
| CG_Host  |   | The Central Governance host address.<br/> **If you enabled {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, you must complete this field.** | cg.host  |
| CG_Port | @default = 12553  | The Central Governance port.<br/> **If you enabled {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, you must complete this field.** | cg.port  |
| CG_Mutual_Port  | @default = 12554  | The Central Governance port for Mutual Authentication.<br/> **If you enabled {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, you must complete this field.** | cg.mutual_auth_port  |
| CG_SharedSecret  |   | Specify the shared secret, which is needed to register with the Central Governance server.<br/> **If you enabled {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, you must complete this field.** | cg.shared_secret  |
| CG_ConfigurationPolicy  |   | Specify Central Governance configuration policy to apply to the Transfer CFT instance.  | cg.configuration_policy  |
| CG_Certificate_Path  | @default = $CFTDIRRUNTIME/conf/pki/passportCA.pem  | Specify the Custom Certificate to authenticate Central Governance.  | N/A  |


****Sentinel Connector parameters****


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| Sentinel_Enable  | @default = No  | Set to Yes to enable Sentinel.<br/> **Do not enable this if you have enabled {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.** | sentinel.xfb.enable  |
| Sentinel_Host  |   | Enter the Sentinel host address.  | sentinel.trkipaddr  |
| Sentinel_Port  | @default= 1305  | Enter the Sentinel port.<br/> You do not need to define this field if you are registering Transfer CFT with {{< TransferCFT/suitevariablesCentralGovernanceName  >}}. | sentinel.trkipport  |
| Sentinel_Log_Filter  | @default = EWF  | Sentinel Log Filter: (I)nformation, (W)arning, (E)rror, (F)atal Authorized characters are only I, W, E, F<br/> You can only use each letter once. | sentinel.xfb.log  |
| Sentinel_Transfer_Filter  | @default = ALL  | Sentinel Transfer Filter<br/> Possible values are: ALL, SUMMARY, NO, ERROR | sentinel.xfb.transfer  |
| Sentinel_Use_SSL  | @default = Yes  | Enables an SSL connection with Sentinel.  | sentinel.xfb.use_ssl  |
| Sentinel_Certificate_Path  | @default=$CFTDIRRUNTIME/conf/pki/passportCA.pem  | Sentinel root certificate.  | N/A  |


**Windows Services parameters**


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| CFT_StartAsService  | @default = No  | Start Transfer CFT Server using service mode.  | cft.nt.service_mode  |
| CFT_ServiceName<br/> CFT_ServiceDisplayName | @default= Transfer_CFT<br/> @default= AMPLIFY Transfer CFT | Transfer CFT Server Service name.<br/> You cannot have spaces in the CFT_ServiceName. |  cft.nt.service_name  |
| CFT_ServiceSpecificUser  | @default= No  | Use a specific account to start the Transfer CFT Server Service.  | N/A  |
| CFT_ServiceUsername<br/> CFT_ServicePassword |   | The Username (Domain\User) of the user who will start the Transfer CFT Server.  | N/A  |
| CFT_StartType  | @default= auto  | Transfer CFT Server service start type: auto, manual, disabled  | N/A  |
| CFTUI_StartAsService  | @default= No  | Start Transfer CFT UI Server using service mode.  | copilot.nt.service_mode  |
| CFTUI_ServiceName CFTUI_ServiceDisplayName  | @default= Transfer_CFT_UI<br/> @default= AMPLIFY Transfer CFT UI | Transfer CFT UI Server Service name.<br/> You cannot have spaces in the CFTUI_ServiceName. | copilot.nt.service_name  |
| CFTUI_ServiceSpecificUser  | @default= No  | Use a specific account to start the Transfer CFT UI Server Service.  | N/A  |
| CFTUI_ServiceUsername CFTUI_ServicePassword  |   | The Username (Domain\User) of the user who will start the Transfer CFT UI Server.  | N/A  |
| CFTUI_StartType  | @default= auto  | The Transfer CFT UI Server service start type: auto, manual, disabled  | N/A  |


****Miscellaneous parameters****


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| JAVA_BINARY_PATH  |   | Java binary path used to start Transfer CFT jar files.  | cft.jre.java_binary_path  |
| IntegrityControl_Enable  | @default = Yes  | Activate integrity control for the Transfer CFT databases.  |   |
| Parameter  | Automatic or default  | Description  | UCONF  |


**SAML parameters**


| Parameter  | Automatic or default  | UCONF  |
| --- | --- | --- |
| Enable SAML  | @default = No  | am.type= 'saml'  |


Password management
-------------------

The passwords used in the `initialize.properties` file are encrypted in the original file when you run the installation builder. You can then use the original file as a template for future installations. Impacted passwords are prefaced by &lt;CFT_PASSWORD&gt;, and include the following:

- CryptoKey_Password
- UI_DefaultUser_Password
- CFT_ServicePassword
- CFTUI_ServicePassword
- RESTAPI_Cert_Pass
- CG_SharedSecret
