---

    title: Manage the Transfer CFT Copilot server
    linkTitle: Manage the Transfer CFT UI server
    weight: 190

---
The Transfer CFT Copilot server provides a number of functions that support the following services: the Transfer CFT UI, REST API services, web services, multi-host/multi-node architectures, and Central Governance.

This page describes how to start/stop and check the status of the Transfer CFT Copilot server.

<span id="Start_/_stop_Copilot"></span>

## Start and stop the Copilot server

To start the Copilot server, run the command:

```
copstart
```

To stop the Copilot server, run the command:

```
copstop
```

To stop the Copilot server and force connections to close, run:

```
copstop -f
```

To kill all Copilot server processes, run:

```
copstop -kill
```

> **Note**
>
> For details on starting in your specific operating system, refer to the corresponding Transfer CFT 3.9 Installation Guide.

<span id="Check__status"></span>

### Check the Copilot server status

To check the Copilot status, enter:

```
<span class="code">`copstatus`</span>
```

The <span class="code">`copstatus `</span>return code is 0 when Copilot is running, and 1 when Copilot is stopped.

Additional <span class="code">`copstatus `</span>commands include:

```
<span class="code">```` copstatus [-p] [-v] [-h|--help]         </span>   -p           print copsmng pid if copilot is started          -v           print status message         </span>   -h|--help    copstatus help     ``` ````</span>

## Windows menus

You can also use the Windows <span class="bold_in_para">****Start**** </span>menu to start Axway software. From the <span class="bold_in_para">****Start**** </span>menu, select <span class="bold_in_para">****Programs**** </span>(or All Programs), <span class="bold_in_para">****Axway Software****</span> and then the product.

For example, to start the Copilot server click <span class="bold_in_para">****Start &gt; All Programs &gt; Axway Software &gt; {{< TransferCFT/axwayvariablesComponentShortName  >}} &gt; Start {{< TransferCFT/axwayvariablesComponentShortName  >}} UI Server.****</span>

## Operating system specific tasks

### z/OS

#### Start

COPRUN is an example of a JCL statement that starts the Transfer CFT Copilot server. The server can be started as a Start Task. The Transfer CFT Copilot server STEPLIB, and then JOBLIB should be defined as an APF. If it is not defined as an APF, no RACF check can be performed. This results in no log-on check being available and all requests are done with the user associated with the server JOB.

When the <span class="code">`copilot.misc.CreateProcessAsUser`</span> variable is set, STEPLIB or JOBLIB can be non-APF. Only a {{< TransferCFT/suitevariablesCentralGovernanceName  >}}/PassPort user can sign on to Copilot user interface.

> **Note**
>
> When the ‘cft.mvs.copilot.check\_apf’ uconf variable is set to ‘Yes’, CFTCOPL must be APF authorized to start.

LOG message: <span class="code">`+CFTI42E Copilot must be APF-authorized.`</span>

> **Note**
>
> CFTCOPL must be APF authorized to start if the UCONF cft.mvs.copilot.check\_apf variable is set to Yes. Otherwise, the Transfer CFT log displays CFTI42E Copilot must be APF-authorized.

#### Stop

COPSTOP is an example of the JCL stop statement for the Transfer CFT UI server. You can also stop the Transfer CFT UI server using the operator command pause (/P jobname) for the server-associated task.

### IBM i

#### Start

****Menu****

1. Access the *Transfer CFT* <span class="italic_in_para" style="font-style: italic;">**Main Menu**</span>.  
    In the Main Menu enter the command <span class="code">`cft`</span> and press <span class="bold_in_para">****Enter****</span> to open the Transfer CFT menu.
1. Enter **1** to access **Common CFT commands**.
1. Select option <span class="span_5" style="font-weight: bold;">****1 Start Copilot****</span>. The *Copilot server* menu is displayed.  

****Command****

Execute: <span class="code">`COPSTART `</span>

#### Stop

****Menu****

1. Access the *Transfer CFT* <span class="italic_in_para" style="font-style: italic;">**Main Menu**</span>.  
    In the Main Menu enter the command <span class="code">`cft`</span> and press <span class="bold_in_para">****Enter****</span> to open the Transfer CFT menu.
1. Enter **1** to access **Common CFT commands**.
1. Select option <span class="span_5" style="font-weight: bold;">****2**** </span><span class="span_5" style="font-weight: bold;">****Stop Copilot****</span>.  
    Only the server waiting for a connection is stopped. Other servers that users have logged onto are shut down when the user logs off, or after a network timeout.

****Command****

Execute: <span class="code">`COPSTOP `</span>

<span id="Copilot"></span>

## Transfer CFT Copilot server configuration

### General Copilot server parameters

The following table lists the UCONF identifiers
and the default values for the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI (Copilot) server.


| ID  | Default  | Description  |
| --- | --- | --- |
| copilot.general.serverport | 1766  | Copilot server listening port.  |
| copilot.general.serverhost  | 0.0.0.0  | TCP Transfer CFT Copilot server address, where 0.0.0.0 indicates that you want the Transfer CFT UI to listen on all network interfaces if your machine has more than one.  |


****UNIX****

Refer to the [UCONF parameters](../uconf/uconf_directory) table for information on <span class="code">`copilot.*.unix `</span>parameters.

#### Alias management

You can access customized file system directories via the {{< TransferCFT/axwayvariablesComponentShortName  >}} user interface HTTP server using aliases.

To add a new alias, access the Unified Configuration and configure the following:


| ID  | Description  |
| --- | --- |
| copilot.http.aliases  | List of enabled alias-id  |
| copilot.http.aliases.(alias-id) alias  | Name of the alias  |
| copilot.http.aliases.(alias-id).path  | Path that replaces the alias in the URL  |


#### View available drives

To view available drives from the <span class="bold_in_para">****Edit a file****</span> icon in the graphical user interface, define the following:


| Parameter  | Options  | Description  |
| --- | --- | --- |
| copilot.nt.rootdrives  | @REMOVABLE_DRIVES  | To view removable drives such as a USB key, CD, and so on.  |
|   | @LOCAL_DRIVES  | To view hard drives.  |
|   | @NET_DRIVES  | To view network drives.  |


#### Client keep-alive

Use this parameter to define the keep-alive interval in seconds for a client session. By default, this occurs every 60 seconds.


| Parameter  | Value  |
| --- | --- |
| copilot.misc.client_keep_alive_delay  | Enter an integer for the delay in seconds.<br/> 60 = default<br/> 0 = no keep-alive |


#### Client timeout

Use this parameter to define the client timeout in minutes. By default, the timeout is 30 minutes.


| Parameter  | Value  |
| --- | --- |
| copilot.misc.ClientTimeout  | Enter an integer for the timeout in minutes.<br/> • 30 = default timeout of 30 minutes<br/> • 0 = 60 minute timeout for the Transfer CFT UI (token)<br/> • 0 = no timeout for the deprecated Copilot UI |


#### Web services

Use these parameter to define the {{< TransferCFT/axwayvariablesComponentShortName  >}} Web Services. See also [Setting up Web Services](../../cft_intro_install/about_this_document_ibmi/using_apis/about_web_services).


| Parameter  | Value  | Former value  |
| --- | --- | --- |
| copilot.webservices.wsicomplience  | (bool) No  | [WEBSERVICES] WsiComplience  |
| copilot.webservices.upload_directory  | (dir) $(cft.runtime_dir)/conf/ws_upload  | NA  |


#### REST API server

Use these parameter to configure the REST API server. See also [Transfer CFT REST API concepts](../../app_integration_intro/using_apis/api_intro).


| Parameter  | Type  | Default  | Description  |
| --- | --- | --- | --- |
| copilot.restapi.enable  | bool  | No  | Enable/disable the REST API service:<br/> • Yes: enable<br/> • No: disable |
| copilot.restapi.serverport  | int  | 1768  | REST API server port. |
| copilot.restapi.authentication_method  | string  | system (Windows)<br/> xfbadm (UNIX) | Defines authentication method.<br/> <br/> See also, <a href="../../cft_intro_install/unix_install_start_here/run_first_time_ux/use_cft_utilities#xfbadmusr1">xfbadmusr utilitiy.</a> |
| copilot.restapi.nb_workers  | int  | 4  | Number of activated workers that process the REST API requests.  |
| copilot.restapi.maxclient  | int  | 256  | Number of client connections handled per REST worker.  |
| copilot.restapi.coms_id  | string  | coms  | The TCPIP CFTCOM object identifier used by the REST API server to communicate with the Transfer CFT server.<br/> Leave empty to use the COM file instead. |
| copilot.restapi.catalog.retry_delay  | int  | 5  |  • The delay between retries in seconds. The Transfer CFT Copilot server checks the request status in catalog every retry_delay seconds.<br/> • The delay between retries in seconds. The Transfer CFT Copilot server checks the request status in catalog every retry_delay seconds. |
| copilot.restapi.catalog.retry_timeout  | int  | 30  | The default value of the <a href="../../app_integration_intro/using_apis/api_intro/api_commands#Manage">apiTimeout</a> parameter as defined in the request URL.<br/> Available exclusively for POST requests. |


<span id="Configur"></span>

## Configure Copilot with SSL security

This section describes how to install the certificates that are required to enable:

- Transfer CFT UI (web browser-based)
- REST API  

The basic steps are:

- Install a certificate on the server side
- Install a certificate on the client side
- Connect to Copilot using an SSL connection

### Operating system specific limitations

- For z/OS, certificates can be USS or sequential files.
- For IBM i (OS/400), certificates must be native files if you have enabled the REST API interface. If only Copilot is enabled, IFS files are supported (as in the previous version).

<span id="Install"></span>

### Install a certificate on the server side

#### When using Central Governance

The Transfer CFT Copilot server uses the certificate that was created by Central Governance during the product registration. This certificate is stored in Transfer CFT PKI database and the certificate id is the value of the UCONF <span class="code">`cft.instance_id `</span>parameter. This means that there is no action required to install a certificate.

However, to override the default behavior use the procedure described below in *When using Transfer CFT without governance*.

#### When using Transfer CFT without governance

The following tables describe the UCONF parameters that determine the certificates used by the Transfer CFT UI server to authenticate itself.

You can use the following certificate and private key formats, where the format of the certificate may differ from that of the key.

The certificate type is dictated by the file name extension (.p12, .pkcs12, .der, .pem, for example <span class="code">`my_certificate.pem`</span>).

*For native files in a z/OS or IBM i environment*, if the format cannot be determined (the file suffix used as the extension), Transfer CFT derives the value from these uconf settings:

- <span class="code">`copilot.ssl.sslkeyfile=<not set>`</span> and <span class="code">`copilot.ssl.sslcertpassword=<set>`</span>, then the format is PKCS12
- <span class="code">`copilot.ssl.sslkeyfile= <set>`</span> and <span class="code">`copilot.ssl.sslcertpassword=<not set>`</span>, then the format is PEM

QQQ\_QQQ\_QQQ


| Supported format  | Type  | Extension  |
| --- | --- | --- |
| Certificate  | PKCS#12  | p12, pfx, pkcs12  |
|  - " -  | PEM  | pem  |
|  - " -  | DER  | der  |
| Private key  | PEM  | pem  |
|  - " -  | DER  | der  |
|  - " -  | PKCS#8  | key, pem  |


****How to define a PKCS#12 certificate****

This example uses a single PKCS#12 certificate where you only require the file name and password.


| Parameter | Value |
| --- | --- |
| copilot.ssl.SslCertFile | conf/pki/&lt;my_certificate&gt;.p12 |
| copilot.ssl.SslCertPassword | Certificate password |
| copilot.ssl.SslKeyFile | Not used |
| copilot.ssl.SslKeyPassword | Not used |


****How to define a DER or PEM certificate****

This example uses a DER(or PEM) certificate with the private key in a separate DER file, where you define the key as well as the certificate.


| Parameter | Value |
| --- | --- |
| copilot.ssl.SslCertFile | conf/pki /&lt;my_certificate&gt;.der *or* .pem |
| copilot.ssl.SslCertPassword | Not used |
| copilot.ssl.SslKeyFile | conf/pki /&lt;my_key&gt;.der *or* .pem |
| copilot.ssl.SslKeyPassword | Key password, which is mandatory if the key file is encrypted PKCS#8 |


#### Additional HTTPS parameters

There are two additional UCONF parameters to use for HTTPS connections:


| Parameter | Value |
| --- | --- |
| copilot.http.onlyssl |  • No: Default value.<br/> • Yes: Restricts access to the Transfer CFT Copilot server to HTTPS secured connections only. |
| <span id="copilot.ssl.SslCipherSuites"></span>copilot.ssl.SslCipherSuites<br/>  | A comma separated list of cipher suites accepted by the Transfer CFT Copilot server, for example: “47, 10, 9, 2”.<br/> See the <a href="../../transport_security_start_here/manage_cipher_suites#cipher_suites">Supported cipher suites</a> for details. |


### Install a certificate on the client side

****Windows****

On Windows, there are two ways to install a certificate on the client side - using a Windows certificate or the Java keystore.

****UNIX****

On Linux, using the Java keystore is the only option.

#### Install a certificate in the Windows keystore

1. In Windows Explorer, navigate to the certificate <span class="code">`<my_root_certificate>.der`</span> and right-click (for example, at &lt;CFTDIRRUNTIME>/conf/pki/&lt;my\_root\_certificate>.der).
1. Select the <span class="bold_in_para">****Install certificate****</span> option.
1. Follow the screen instructions. Windows automatically imports the certificate to its keystore in the <span class="code">`Intermediate certificate authorities`</span> folder.

****Alternative method****

1. In Internet Explorer, select <span class="bold_in_para">****Tools > Internet Options.**** </span>
1. In the <span class="bold_in_para">****Content**** </span>tab select the <span class="bold_in_para">****Certificate**** </span>button.
1. Select <span class="bold_in_para">****Import,**** </span>which starts the <span class="bold_in_para">****Certificate Import Wizard****</span>.
1. Click <span class="bold_in_para">****Next****</span>, and <span class="bold_in_para">****Browse**** </span>to the<span class="code">` <my_root_certificate>.der`</span>.
1. Follow the screen instructions. Windows imports the certificate to its keystore.

#### Install a certificate in the Java keystore

The Java keystore is a file located at<span class="code">` ~/jre/lib/security/cacerts`</span>. The default password for this keystore is “changeit”.

Use the keytool command as follows to import the<span class="code">` <my_root_certificate>.der `</span>certificate into the Java keystore:

```
<span class="span_2">keytool </span><span class="span_2">–</span><span class="span_2">importcert</span>
   -trustcacerts
   -alias AXWMFTCA
<span class="span_2">   -file <my_root_certificate></span><span class="span_2">.der</span>
   -storepass changeit<span class="span_2">-keystore </span><span class="span_2"><keystore></span>
```
