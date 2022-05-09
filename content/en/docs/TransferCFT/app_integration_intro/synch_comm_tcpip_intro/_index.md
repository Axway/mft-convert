{
    "title": "Using synchronous communication commands",
    "linkTitle": "Using synchronous communication commands",
    "weight": "250"
}About synchronous communication (TCP/IP)
----------------------------------------

Synchronous communication provides a real-time response when sending data from a client to the Transfer CFT server. This response indicates that the client command was acknowledged by Transfer CFT and is listed in the catalog. For a transfer request, for example, the response would indicate the date/time stamp and unique identifier associated with the transfer.

An additional benefit of synchronous communication is that you can use it for monitoring purposes, as it returns IDTU information about a transfer.

The alternative to synchronous mode is an asynchronous mode, which places the client data in a communication file where it waits to be picked up by the Transfer CFT server, at which point it is then listed in the catalog.

Another difference between modes is that unlike synchronous mode, asynchronous mode does not require that {{< TransferCFT/axwayvariablesComponentLongName  >}} be started for the client to send data.

 

![](/Images/TransferCFT/new_synch_comm.png)

Configuring synchronous communication
-------------------------------------

There are two steps to set up synchronous communication in Transfer CFT, configure the server and the client.

### Configure the server

Use the CFTCOM command to define the synchronous communication settings. In the {{< TransferCFT/suitevariablesCentralGovernanceName  >}} {{< TransferCFT/suitevariablesDocTypeUser  >}}, refer to **Transfer request mode &gt; synchronous**.

- Port: Use the Transfer CFT server port that receives the client commands
- Maximum connection: Set the number of incoming connections on the Transfer CFT server
- Session timeout: Set the amount of time after which Transfer CFT disconnects the client connection

**Example**

```
CFTUTIL CFTCOM ID=COMS, TYPE=TCPIP, HOST=localhost, PORT=1765, PROTOCOL=XHTTP, DISCTS=60
CFTUTIL uconfset id=cft.server.cftcoms.max_connection, value=256
```

If you manually configure synchronous communication in Transfer CFT, remember to add the COMS identifier in the CFTPARM COM parameter.

Resulting LOG message after a Transfer CFT restart

```
CFTI18I+ TYPE : XHTTP HOST : 127.0.0.1 PORT : 1765
```

### Configure the client

On the client side perform one of the following configuration procedures, depending on the type of client you are using.

**Example**

#### Simple client configuration using CFTUTIL

Start a CFTUTIL session and perform the following commands.

```
CONFIG TYPE=COM, MEDIACOM=TCPIP, FNAME=XHTTP://localhost:1765
<execute requests in same session>
```

Use a configuration file to define the synchronous communication, where the file should contain at least the following elements:

- \# TCP/IP COMMUNICATION
- TYPE =TCP
- NAME =xhttp://localhost:1765
- TIMEOUT = 600

Start a CFTUTIL session and perform the following commands.

```
 CONFIG TYPE=COM, FNAME=<path><config_file>
<execute requests in same session>
```

#### API

The API configuration consists of using a configuration file (the same elements as in a simple client configuration).

```
cftau ("COM",C=<path><config_file>);
```

#### JPI

JPI client configuration consists of using a configuration file (the same elements as in a simple client configuration).

```
CFTUTIL uconfset id=copilot.cft.com, value=’C=<path><config_file>’
```

#### Web services

When using web services the default media identifier used is the first once declared in the general CFTPARM object. Additionally, you can override this in the web services XML file by adding the desired COM using the format `<axw:CFTCOM_ID>COM0</axw:CFTCOM_ID>` in the SOAP request.

#### Transfer CFT UI

```
CFTUTIL uconfset id=copilot.cft.com, value=’C=<path><config_file>’
```

When using the Transfer CFT UI the first media identifier listed in the server's parameter file (CFTPARM) is used. So for example, if CFTPARM ID = IDPARM0, where COM = (COM0, COM1), the COM0 media is used.

Commands to use with synchronous communication
----------------------------------------------

You can user either the [SWAITCAT](../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/swaitcat_concepts) command or the SEND/RECV command with the [WSTATES, WPHASES, and WPHASESTEPS](sync_transfer_request_tasks) parameters to manage Transfer CFT transfer wait states.

Securing synchronous communication
----------------------------------

You can use password management to control remote users that can perform CFTUTIL commands using synchronous communication media, see [Password authentication](../../c_intro_userinterfaces/about_cftutil/control_remote_users_synch_com).

Troubleshooting
---------------

For more information on errors and corrective actions, see [Synchronous communication return codes](../../troubleshoot_intro/about_error_codes/synch_comm_return_codes).
