---
    "title": "Manage the Transfer CFT Copilot server",
    "linkTitle": "Manage the Transfer CFT UI server",
    "weight": "180"
---
The Transfer CFT Copilot server provides a number of functions that support the following services: the Transfer CFT UI, REST API services, web services, multi-host/multi-node architectures, and Central Governance.

This page describes how to start/stop and check the status of the Transfer CFT Copilot server.

Start and stop the Copilot server
---------------------------------

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
> Note: For details on starting in your specific operating system, refer to the corresponding Transfer CFT 3.10 Installation Guide.

<span id="Check__status"></span>

### Check the Copilot server status

To check the Copilot status, enter:

```
copstatus
```

The `copstatus `return code is 0 when Copilot is running, and 1 when Copilot is stopped.

Additional `copstatus `commands include:

```
copstatus [-p] [-v] [-h&#124;--help]
-p print copsmng pid if copilot is started
-v print status message
-
h&#124;--help copstatus help
```

Windows menus
-------------

You can also use the Windows ****Start**** menu to start Axway software. From the ****Start**** menu, select ****Programs**** (or All Programs), ****Axway Software**** and then the product.

For example, to start the Copilot server click ****Start &gt; All Programs &gt; Axway Software &gt; {{< TransferCFT/axwayvariablesComponentShortName  >}} &gt; Start {{< TransferCFT/axwayvariablesComponentShortName  >}} UI Server.****

Operating system specific tasks
-------------------------------

### z/OS

#### Start

{{% TransferCFT/snippets/zos_copilot_start%}}

#### Stop

{{% TransferCFT/snippets/zos_copilot_stop%}}

### IBM i

#### Start

{{% TransferCFT/snippets/ibm_i_copilot_start%}}

#### Stop

{{% TransferCFT/snippets/ibm_i_copilot_stop%}}
<span id="Copilot"></span>

Transfer CFT Copilot server configuration
-----------------------------------------

### General Copilot server parameters

The following table lists the UCONF identifiers
and the default values for the {{< TransferCFT/axwayvariablesComponentShortName  >}} UI (Copilot) server.


| ID  | Default  | Description  |
| --- | --- | --- |
| copilot.general.serverport | 1766  | Copilot server listening port.  |
| copilot.general.serverhost  | 0.0.0.0  | TCP Transfer CFT Copilot server address, where 0.0.0.0 indicates that you want the Transfer CFT UI to listen on all network interfaces if your machine has more than one.  |


****UNIX****

Refer to the [UCONF parameters](../uconf/uconf_directory) table for information on `copilot.*.unix `parameters.

#### Alias management

You can access customized file system directories via the {{< TransferCFT/axwayvariablesComponentShortName  >}} user interface HTTP server using aliases.

To add a new alias, access the Unified Configuration and configure the following:


| ID  | Description  |
| --- | --- |
| copilot.http.aliases  | List of enabled alias-id  |
| copilot.http.aliases.(alias-id) alias  | Name of the alias  |
| copilot.http.aliases.(alias-id).path  | Path that replaces the alias in the URL  |


#### View available drives

To view available drives from the ****Edit a file**** icon in the graphical user interface, define the following:


| Parameter  | Options  | Description  |
| --- | --- | --- |
| copilot.nt.rootdrives  | @REMOVABLE_DRIVES  | To view removable drives such as a USB key, CD, and so on.  |
| copilot.nt.rootdrives  | @LOCAL_DRIVES  | To view hard drives.  |
| copilot.nt.rootdrives  | @NET_DRIVES  | To view network drives.  |


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
| copilot.restapi.authentication_method  | string  | system (Windows)<br/> xfbadm (UNIX) | Defines authentication method.<br/> <br/> See also, [xfbadmusr utilitiy.](../../cft_intro_install/unix_install_start_here/run_first_time_ux/use_cft_utilities#xfbadmusr1) |
| copilot.restapi.nb_workers  | int  | 4  | Number of activated workers that process the REST API requests.  |
| copilot.restapi.maxclient  | int  | 256  | Number of client connections handled per REST worker.  |
| copilot.restapi.coms_id  | string  | coms  | The TCPIP CFTCOM object identifier used by the REST API server to communicate with the Transfer CFT server.<br/> Leave empty to use the COM file instead. |
| copilot.restapi.catalog.retry_delay  | int  | 5  |  • The delay between retries in seconds. The Transfer CFT Copilot server checks the request status in catalog every retry_delay seconds.<br/> • The delay between retries in seconds. The Transfer CFT Copilot server checks the request status in catalog every retry_delay seconds. |
| copilot.restapi.catalog.retry_timeout  | int  | 30  | The default value of the apiTimeout parameter as defined in the request URL.<br/> Available exclusively for POST requests. |


<span id="Configur"></span>

Configure Copilot with SSL security
-----------------------------------

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

The Transfer CFT Copilot server uses the certificate that was created by Central Governance during the product registration. This certificate is stored in Transfer CFT PKI database and the certificate id is the value of the UCONF `cft.instance_id `parameter. This means that there is no action required to install a certificate.

However, to override the default behavior use the procedure described below in *When using Transfer CFT without governance*.

#### When using Transfer CFT without governance

{{% TransferCFT/snippets/install_certificate%}}

#### Additional HTTPS parameters

There are two additional UCONF parameters to use for HTTPS connections:


| Parameter | Value |
| --- | --- |
| copilot.http.onlyssl |  • No: Default value.<br/> • Yes: Restricts access to the Transfer CFT Copilot server to HTTPS secured connections only. |
| <span id="copilot.ssl.SslCipherSuites"></span>copilot.ssl.SslCipherSuites<br/>  | A comma separated list of cipher suites accepted by the Transfer CFT Copilot server, for example: “47, 10, 9, 2”.<br/> See the [Supported cipher suites](../../transport_security_start_here/manage_cipher_suites#cipher_suites) for details. |


### Install a certificate on the client side

****Windows****

On Windows, there are two ways to install a certificate on the client side - using a Windows certificate or the Java keystore.

****UNIX****

On Linux, using the Java keystore is the only option.

#### Install a certificate in the Windows keystore

1. In Windows Explorer, navigate to the certificate `<my_root_certificate>.der` and right-click (for example, at &lt;CFTDIRRUNTIME&gt;/conf/pki/&lt;my_root_certificate&gt;.der).
1. Select the ****Install certificate**** option.
1. Follow the screen instructions. Windows automatically imports the certificate to its keystore in the `Intermediate certificate authorities` folder.

****Alternative method****

1. In Internet Explorer, select ****Tools &gt; Internet Options.****
1. In the ****Content**** tab select the ****Certificate**** button.
1. Select ****Import,**** which starts the ****Certificate Import Wizard****.
1. Click ****Next****, and ****Browse**** to the` <my_root_certificate>.der`.
1. Follow the screen instructions. Windows imports the certificate to its keystore.

#### Install a certificate in the Java keystore

The Java keystore is a file located at` ~/jre/lib/security/cacerts`. The default password for this keystore is “changeit”.

Use the keytool command as follows to import the` <my_root_certificate>.der `certificate into the Java keystore:

```
keytool
–
importcert
   -trustcacerts
   -alias AXWMFTCA
   -file <my_root_certificate>
.der
   -storepass changeit -keystore
<keystore>
```
