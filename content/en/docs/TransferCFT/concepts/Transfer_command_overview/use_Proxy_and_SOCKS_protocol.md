{
    "title": "Use  a proxy with SOCKS protocol",
    "linkTitle": "Transfers via proxy and SOCKS",
    "weight": "320"
}A proxy is a program
that acts as an intermediary between a client and server. It is often
used to interconnect two networks via a single point.

SOCKS is a protocol that is independent of the application protocol,
and is used to relay a TCP session via a proxy. This independence means
that it can relay both the PeSIT and ODETTE file transfer protocols the PeSIT protocol with
or without SSL.

This topic describes how to use a proxy and SOCKS, and includes:

- [SOCKS protocol architecture](#Connection)
    -   [About the connection](#Connection)
- [Using a proxy with Transfer CFT](#Application_in_CFT)
    -   [Configuring a connection](#Configuration)
    -   [Error
        codes](#Error_codes)
    -   SOCKS references
- [Setting up a proxy for Copilot](#Setting)

<span id="SOCKS"></span>

SOCKS protocol architecture
---------------------------

{{< TransferCFT/axwayvariablesComponentShortName  >}} supports versions 4 and 5 of the SOCKS protocol. A brief
explanation of the SOCKS protocol is provided in this topic.

![Proxy server is between the client and server where client initiates the Connect Request to proxy](/Images/TransferCFT/proxy2_new.png)

<span id="Connection"></span>

### About the connection

#### SOCKS4 exchanges

When using SOCKS4, the exchange between client and server consists of:

1. The client logs into the proxy and sends
    it a connection request specifying the server IP address (version 4)
    or host name, the listening port number of the server, and
    a user name.
1. The proxy checks the connection request and either executes or rejects
    it.
1. A response is sent to the client, indicating the status of the request
    (the server is connected, the connection with the server has failed, the
    user is not authorized, and so on). Once the connection is correctly established
    between the proxy and server, the client and the server are connected
    via the proxy and can perform transparent data inter exchanges (PeSIT,
    and so on).

#### SOCKS5 exchanges

When using SOCKS5, the exchange between client and server consists of:

1. The client and server negotiate an authentication method for the client. The client sends a list of methods it supports and the server responds to indicate which method it has chosen. The only method supported by Transfer CFT is username/password.
1. The client authenticates itself using its username/password. The sever response code indicates if the authentication was successful or not. If authentication is refused, the server immediately closes the connection.
1. The client sends a connection request to the proxy, specifying the remote server IP address or host name and its port number.
1. The proxy checks the connection request and either executes or rejects it. A response is sent to the client, indicating the request was successful or not. Once the connection is correctly established between the proxy and server, the client and the server are connected via the proxy and can perform transparent data inter exchanges (PeSIT, and so on). If the connection if refused, the server immediately closes the connection.

<span id="Application_in_CFT"></span>

Using a proxy in {{< TransferCFT/axwayvariablesComponentShortName  >}}
---------------------------------------------------------------------------

The CFTNET command for a proxy is static. To apply modifications, Transfer
CFT must be shutdown, the new parameters interpreted (CFTUTIL) and Transfer
CFT restarted.

Additionally, at least one file transfer protocol (CFTPROT command) must be declared
for the CFTNET command (describing an access via a proxy for a
protocol).

> **Note**
>
> Note: Transfer CFT only accepts outgoing calls.

The protocols (CFTPROT) and the networks (CFTNET)
must be declared in the CFTPARM command to be applied by Transfer CFT.

#### File transfers via a proxy

![Placement of proxy between Transfer CFT in Intranet and client in Internet](/Images/TransferCFT/proxy_new.png)

<span id="Error_codes"></span>

### Error codes

Transfer CFT responds to error codes from a proxy with:

- Error code 91 for SOCKS4
- Error code 5 for SOCKS5

Transfer CFT repeats the transfer. The
transfer timeout and number of attempts will depend on the CFTTCP command RETRYW, RETRYN
and RETRYM parameters defining the network characteristics of the remote
partner.

- 92 and 93
    for SOCKS4
- Error code != 5 and &gt; 0 for SOCKS5

Transfer CFT puts the transfer on hold (K status). The operator must manually restart (START command) the transfer.

### SOCKS parameter definitions

The following tables lists common parameters for either SOCKS 4 or SOCKS 5. The only difference between the parameters used for SOCKS 4 and 5 is the PASSWORD parameter.


| Parameters  | Value  | Description  |
| --- | --- | --- |
| **ID** | STRING max_length=32  | Network resource identifier. |
| CALL | STRING max_length=0  | Call direction possible through this network resource. ('<span >INOUT</span>','OUT','IN') |
| CLASS | Number &lt;1&gt; min=0 max=64  | Logical class for the physical link. |
| MAXCNX  | Number &lt;32&gt; min=1 max=2000  | Maximum number of simultaneous connections that Transfer CFT accepts to establish on this network resource. |
| TYPE  | TCP  | Defines the type of network resource. |
| BUFLEN  | Number &lt;0&gt; min=32 max=32766  | Size of buffers.  |
| COMMENT  | STRING max_length=80  | Free comment.  |
| ORIGIN  | STRING max_length=0  | Values include 'CFTUTIL','<span >C</span>','DESIGNER','D','COPILOT','O'.  |
| **INET**  | STRING max_length=32  | Identifier of the CFTNET command defining access to the first network  |
| **HOST**  | String max_length=64  | Resource address  |
| **PORT**  | Number &lt;0&gt; min=1 max=65535  | Listening port of the proxy/proxies in the first network  |
| USER  | String max_length=32  | User name transmitted in the connection request addressed to the proxy  |
| PASSWORD  | String max_length=32  | *SOCKS 5 only*<br/> User password transmitted in the connection request addressed to the proxy. |


****Example****

```
CFTNET ID = 'NET0',
TYPE = 'TCP',
....
PROTOCOL = 'SOCKS5',
USER = 'john',
PASSWORD = 'foo'
CFTPROT ID = 'PESITANY_SOCKS5',
...
```
<span id="Configuration"></span>

### Configuring a connection

To configure an outgoing connection through a proxy, user must define the following objects. The numbers referenced here are taken from the example below.

- A network resource with the class 1 (NET0 in the example).
- A network resource with the class 2 (NSOCKS5 in the example) and inet parameter = NET0. This net also contains SOCKS proxy host, port, username and password.
- A transfer protocol with the parameter net = NSOCKS5 (value used in the example).
- A partner referencing the protocol and class = 2 in its CFTTCP definition.
- The default port for proxy servers is 1080.

****Example****

```
CFTNET ID = 'NET0',
TYPE = 'TCP',
CALL = 'INOUT',
MAXCNX = '128',
CLASS = '1',
HOST = 'localhost'
CFTNET ID = 'NSOCKS5',
TYPE = 'TCP',
CALL = 'INOUT',
MAXCNX = '32',
CLASS = '2',
HOST = proxy_address,
PORT = proxy_port,
INET = 'NET0',
PROTOCOL = 'SOCKS5',
USER = 'john',
PASSWORD = 'foo'
CFTPROT ID = 'PESITANY_SOCKS5',
NET = 'NSOCKS5'
CFTPART ID = 'PARIS_NSOCKS5',
CFTTCP ID = 'PARIS_ NSOCKS5',
CLASS = '2',
```

### SOCKS references

- You can find the SOCKS4 specification at: [http://ftp.icm.edu.pl/packages/socks/socks4/SOCKS4.protocol.](http://ftp.icm.edu.pl/packages/socks/socks4/SOCKS4.protocol)
- The SOCKS5 specification is described in RFC 1928 at: <http://tools.ietf.org/html/rfc1928>.

<span id="Setting"></span>

Setting up a proxy for Copilot
------------------------------

The proxy implementation for {{< TransferCFT/axwayvariablesComponentShortName  >}} Copilot is handled directly by the Java Socket class, and uses either the SOCKS V4 or V5 protocol. Note that the HTTP proxy that is used to connect to the Transfer CFT Copilot server for downloading is different from the one used for SOCKS 4 data exchange between Copilot client and server. Therefore, you require 2 proxies:

- An HTTP proxy that you set in internet browser
- A SOCKS 4 proxy that you set in Copilot

Since the Copilot log in screen in a browser window does not provide a way to enter proxy settings, you must force the **Connect to a product** dialog box to display by entering faulty credentials in the Copilot log in screen. Once this connection dialog box displays, you can then set the proxy address and port.

The step is only required for your first log in through a proxy. Copilot retains the proxy address and port settings for subsequent connections.

To remove proxy and revert to the standard log in, simply remove the proxy address and port settings in the connection dialog box.

********Connect to a product dialog box********

![Copilot Connect to a product login screen ](/Images/TransferCFT/copilot_connection_box.png)

****Related topics****

- Network resources - CFTNET (UI)
- [CFTNET (CFTUTIL)](../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftnet)
