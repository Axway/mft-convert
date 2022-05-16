---

    title: Customize the initialize.properties file
    linkTitle: 3. Customize the initialize.properties file
    weight: 150

---
A common practice is to create a copy of the <span class="code">`initialize.properties `</span> file, which is located in the downloaded installation package. This gives you an initial intact version should you later need it.

Customize the <span class="code">`initialize.properties `</span> file. Use the table below to help you with parameter settings; note that the <span class="code">`CryptoKey_Password`</span> is mandatory. Be sure that if you want to use special characters in a configuration file field, you protect the value by enclosing it in double quotation marks ("").

> **Note**
>
> If you are installing Transfer CFT but have another Transfer CFT profile loaded, you cannot have environment variables in the initialize.properties file for the new installation.

****Example****

To use the # character in a value, for example, protect the entire string using quotation marks "" as follows:

`CryptoKey_Password = "Aedft#439"`

If you do not enclose this value in "", the string is interpreted as: <span class="code">`CryptoKey_Password = Aedft`</span>

> **Note**
>
> Some parameters can be calculated during the installation (flagged by @automatic); you can leave these fields blank.

> **Note**
>
> Parameters that have default values are flagged by @default.

QQQ\_QQQ\_QQQ split BIG table

## Parameters by type

### Silent installation parameters of initialize.properties


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| Installation architecture  | @default = single  | Defines single or cluster mode installation.<br/> Values: single, first_host, additional_host | N/A  |
| installdir  |   | Transfer CFT installation directory.  | cft.install_dir  |


### Basic installation parameters of initialize.properties


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| runtimeDir  | ./runtime  | Transfer CFT runtime directory.<br/> Specify the directory where you want to install the Transfer CFT runtime directory.<br/> By default, the runtime directory is installed in a sub-directory of the Transfer CFT installation directory. Use the default directory, or specify a new directory. A runtime directory will be created if it does not already exist.<br/> Directory paths cannot contain spaces. | cft.runtime_dir  |
| Full_Hostname | @automatic  | Host Address of the local server: FQDN (Fully Qualified Domain Name) or IP Address. See **Note*** | cft.full_hostname  |
| multinode_hostname  | @automatic  | When not defined, this field is filled with the hostname of the machine where you are installing {{< TransferCFT/suitevariablesTransferCFTName  >}}, whether it is the first host or an additional host.<br/> If the hostname contains a "." period, the value used consists of the name of the host preceding the first period. For example, "myhost.fqdn.net" would be shortened to "myhost". | cft.multi_node.hostnames  |
| multinode_host_address  | @automatic  | If you do not specify a value, the machine's FQDN address is used.<br/> Note that if there is an error in the machine's configuration, the value taken could be incorrect. Be sure to check that you can ping the address, and that it is the value for the cluster network. | cft.multi_node.hostnames.&lt;hostname&gt;.host  |
| Instance_ID | @default  | The maximum length is 24.  | cft.instance_id  |
| Instance_Group  |   | Transfer CFT instance group.<br/> The maximum length is 1000. | cft.instance_group  |


> **Note**
>
> \*This host address defines:

- The unconf <span class="code">`sentinel.trkproductipaddr `</span>parameter, which is the host address that identifies this host
- The host address used to connect this Transfer CFT Copilot server

### Security configuration parameters of initialize.properties


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| CryptoKey_Password | @mandatory  | Seed password to generate the encryption key.<br/> The password must contains at least 8 characters, contain upper and<br/> lower case characters as well as numeric and special characters (*$!?+-@). | N/A  |
| CryptoKey_Key_File | @default = $CFTDIRRUNTIME/data/crypto/crypkey  | Location that stores the generated key file.  | crypto.key_fname  |
| CryptoKey_Salt_File | @default = $CFTDIRRUNTIME/data/crypto/crypsalt  | Location that stores the generated salt file.  | crypto.salt_fname  |


### Runtime configuration parameters of initialize.properties


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| Key |   | Key for Transfer CFT. The key is stored in the $CFTDIRRUNTIME/conf/cft.key.<br/> **Without the key you can install, but not start the product.** |   |
| Catalog_File_Size  | @default= 10000  | Sets the default catalog size. | cft.cftcat.default_size  |
| Communication_File_Size  | @default = 1000  | Sets the default communication file size.  | cft.cftcom.default_size  |
| PESIT_Port  | @default = 1761  | The port number of the PeSiT protocol.<br/>  | samples.pesitany_sap.value  |
| PESIT_SSL_Port  | @default = 1762  | The port number of the PeSIT protocol using SSL.  | samples.pesitssl_sap.value  |
| COMS_Port  | @default = 1765  | The port number of the COMS synchronous communication media.<br/>  | samples.coms_port.value  |
| Copilot_Port  | @default = 1766  | Sets the port number for the Transfer CFT Copilot server that listens for<br/> incoming unsecured and secured (SSL) connections.<br/> The same port number is used for the Graphical User Interface and Webservices<br/> with or without SSL. | copilot.general.serverport  |
| Copilot_OnlySSL  | @default = Yes  | When you set to <span >****Yes****</span>, Copilot only allows SSL connections, and disables use of the HTTP server.<br/> This affects the old Transfer CFT UI, Webservices, and JPI.<br/>  | copilot.http.onlyssl  |
| Copilot_SSL_Port  | @default = 1767  | Sets the port number for the Transfer CFT Copilot server that listens for incoming secured (SSL with mutual authentication) connections. Only used by the Governance.  | copilot.general.ssl_serverport  |
| UI_DefaultUser_Username<br/> UI_DefaultUser_Password |   | *UNIX systems only*<br/> The default Transfer CFT UI user/password. | N/A  |
| RESTAPI_Enable<br/> <br/>  | @default = Yes  | Activates the Transfer CFT REST API Server.<br/> The port number used to connect to the REST API server. | copilot.restapi.enable  |
| RESTAPI_Port  | 1768  | The port number used to connect to the REST API server. | copilot.restapi.serverport  |
| RESTAPI_Certificate_Path<br/> RESTAPI_Cert_Pass |   | Sets the certificate and its password to be used by the Transfer<br/> CFT REST API Server and for HTTPS connections with Copilot (old<br/> Transfer CFT UI, Web services, and JPI).<br/> • When REST API is enabled, these cannot be empty<br/> • When Copilot_OnlySSL is activated, these cannot be empty.<br/> When using Central Governance, the REST API server automatically uses the SSL business certificate generated during the registration; there is no need to pre-define this value if you are going to register Transfer CFT with Central Governance. | copilot.ssl.SslCertFile  |


### Multi-node and Cluster parameters of initialize.properties


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| Multinode_Enable  | @default = No  | Enable the multi-node architecture.<br/> To use a multi-node architecture, you must define the multinode option in the initialize.properties file. | cft.multi_node.enable  |
| Multinode_Number  | @default = 2  | Enter the number of nodes.  | cft.multi_node.nodes  |
| LoadBalancer_Host  |   | Specify the host address of the load balancer. When using an ACTIVE/ACTIVE or ACTIVE/PASSIVE deployment, you require a load balancer to connect to the Transfer CFT Copilot server. | cft.multi_node.load_balancer.host  |
| LoadBalancer_Port  |   | Specify the load balancer port, which is redirected to the Central Governance dedicated port of the Transfer CFT UI Server.<br/> When using ACTIVE/ACTIVE or ACTIVE/PASSIVE deployment, you require a load balancer to connect to the Transfer CFT Copilot server.  | cft.multi_node.load_balancer.port  |


### Central Governance parameters of initialize.properties


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| CG_Enable | @default = No | Enter Yes to enable Central Governance connectivity.  | cg.enable  |
| CG_Host  |   | The Central Governance host address.<br/> **If you enabled {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, you must complete this field.** | cg.host  |
| CG_Port | @default = 12553  | Central Governance port.<br/> **When CG is enabled, this cannot be empty.** | cg.port  |
| CG_Mutual_Port  | @default = 12554  | The Central Governance port for Mutual Authentication.<br/> **If you enabled {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, you must complete this field.** | cg.mutual_auth_port  |
| CG_RestAPI_Port  | @default = 8081  | Specify the port to use to communicate with Central Governance's REST API (this port is only used when am.type=cg).<br/> **If you enabled {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, you must complete this field.** | cg.restapi_port  |
| CG_SharedSecret  |   | Specify the shared secret, which is needed to register with the Central Governance server.<br/> **If you enabled {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, you must complete this field.** | cg.shared_secret  |
| CG_ConfigurationPolicy  |   | Specify Central Governance configuration policy to apply on the Transfer CFT instance.  | cg.configuration_policy  |
| CG_Certificate_Path  | @default = $CFTDIRRUNTIME/conf/pki/passportCA.pem  | Specify Custom Certificate to authenticate Central Governance.  | N/A  |


### Sentinel Connector parameters of initialize.properties


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| Sentinel_Enable  | @default = No  | Set to Yes to enable Sentinel.<br/> **Do not enable this if you have enabled {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.** | sentinel.xfb.enable  |
| Sentinel_Host  |   | Enter the Sentinel host address.  | sentinel.trkipaddr  |
| Sentinel_Port  | @default= 1305  | Enter the Sentinel port.<br/> You do not need to define this field if you are registering Transfer CFT with {{< TransferCFT/suitevariablesCentralGovernanceName  >}}. | sentinel.trkipport  |
| Sentinel_Log_Filter  | @default = EWF  | Sentinel Log Filter: (I)nformation, (W)arning, (E)rror, (F)atal Authorized characters are only I, W, E, F<br/> You can only use each letter once. | sentinel.xfb.log  |
| Sentinel_Transfer_Filter  | @default = ALL  | Sentinel Transfer Filter<br/> Possible values are: ALL, SUMMARY, NO, ERROR | sentinel.xfb.transfer  |
| Sentinel_Use_SSL  | @default = Yes  | Enables an SSL connection with Sentinel.  | sentinel.xfb.use_ssl  |
| Sentinel_Certificate_Path  | @default=$CFTDIRRUNTIME/conf/pki/passportCA.pem  | Sentinel root certificate.  | N/A  |


### Miscellaneous parameters of initialize.properties


| Parameter  | Automatic or default  | Description  | UCONF  |
| --- | --- | --- | --- |
| JAVA_BINARY_PATH  |   | Java binary path used to start Transfer CFT jar files.  | cft.jre.java_binary_path  |


## Password management

The passwords used in the <span class="code">`initialize.properties`</span> file are encrypted in the original file when you run the installation builder. You can then use the original file as a template for future installations. Impacted passwords are prefaced by &lt;CFT\_PASSWORD>, and include the following:

- CryptoKey\_Password
- UI\_DefaultUser\_Password
- CFT\_ServicePassword
- CFTUI\_ServicePassword
- RESTAPI\_Cert\_Pass
- CG\_SharedSecret
