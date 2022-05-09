{
    "title": "Implementing IPv6",
    "linkTitle": "Networks",
    "weight": "140"
}The code in Transfer CFT that manages Internet addresses and name resolution is based on the POSIX Protocol Independent API, namely the getaddrinfo() service and related functions.

The use of the POSIX API by application software preserves IPv4 addressing support and allows transparent support for IPv6 addressing at the software level, provided the OS supports this and is configured for IPv6.

Enabling IPv6 support for applications may have adverse effects on the behavior of these applications due to improper system configurations, or incomplete IPv6 support by the OS. For this reason, hostname resolution for IPv6 addresses is disabled by default on Transfer CFT. This means that by default the hostnames defined in Transfer CFT configuration files are resolved only for IPv4 addresses.

<span id="Configuration"></span>

Configuration
-------------

<span id="Unified configuration settings"></span>

### Unified configuration settings

To enable IPv6 name resolution for Transfer CFT, set the following unified configuration parameters to ****NO**** using either the `CFTUTIL uconfset` command or the UI [Unified configuration](../../admin_intro/uconf) option.


| Parameter  | Value  | Description  |
| --- | --- | --- |
| ipv6.disable_connect  | <span >YES</span> &#124; NO  | The value NO enables IPV6 resolution for hostnames used by Transfer CFT to connect to remote servers. |
| ipv6.disable_listen  | <span >YES</span> &#124; NO  | The value NO enables IPV6 resolution for hostnames used by Transfer CFT to listen for incoming connections.  |


You can also configure IPv6 addresses instead of names in the Transfer CFT configuration files. As the configuration options only apply to hostname resolution, Transfer CFT will use the IPv6 addresses, regardless of the IPv6 option settings.

<span id="Name resolution"></span>

### About name resolution

When `ipv6.disable_connect` is set to ****No****, a hostname used by Transfer CFT to connect to a host may refer to either an IPv4 or IPv6 address, or a list of addresses of any type.

When a name resolution request returns a list of several entries, Transfer CFT tries all entries successively until either the connection succeeds or the list is exhausted. This behavior allows Transfer CFT, in a situation where the name might refer to both IPv4 and IPv6 addresses, to successfully connect to a remote service that is listening for either IPv6 TCP connections or IPv4 TCP connections, but not listening for both address types.

When `ipv6.disable_listen` is set to ****No****, the hostname used by Transfer CFT to listen for incoming connections can refer to an IPv4 address, an IPv6 address, or  a list of addresses of any type. If the name resolution request returns a list with several results, Transfer CFT listens only to the first entry in the list.

When the resolution list contains both IPv4 and IPv6 entries, the first entry generally refers to an IPv6 address, although all operating systems are not guaranteed to respond this way. When Transfer CFT listens on an IPv6 address of a network interface, it may or may not also receive IPv4 incoming connections targeting this interface. See [Hybrid dual-stack implementation](#Hybrid%20dual-stack%20implementation).

While you can configure the two IPv6 UCONF parameters independently, it is recommended that you **not** set` ipv6.disable_listen` to `No`, and `ipv6.disable_connect` to `Yes`.

<span id="host attributes in CFTNET"></span>

### Setting the host attributes in CFTNET

Independently of the configuration that applies to all TCP related services in Transfer CFT, two new names can be used for the CFTNET [host](../../c_intro_userinterfaces/command_summary/parameter_intro/host) attribute. These host names, IN6ADDR_ANY and IN4ADDR_ANY can be used in the place of INADDR_ANY. These names indicate the listening point for all protocols for the defined CFTNET as:

- IN6ADDR_ANY: listens for any IPv6 address
- IN4ADDR_ANY: listens for any IPV4 address

Depending on the OS, using IN6ADDR_ANY can allow listening for both IPv4 and IPv6 connections, or for only IPv6 connections. See the hybrid dual-stack section below for details.

<span id="Hybrid dual-stack implementation"></span>

Hybrid dual-stack implementation
--------------------------------

Modern hybrid dual-stack IPv6/IPv4 implementations allow application programs to use sockets created for the IPv6 protocol to also handle IPv4 connections. In such situations, IPv4 addresses are represented in a special IPv6 format called IPv4 mapped IPv6 address.

For IPv6 sockets that are created for:

- Outgoing connections (client side): the IPv4 address must be supplied in this special format
- Accepting connections (server side): IPv4 addresses returned by the system are returned in this special format

This type of IPv6 socket, which can be used for either IPv6 or IPv4 connections, is called a hybrid socket.

The IEEE-POSIX-1003.1-2004 specifications assume that hybrid sockets are available to software applications, but in fact not all operating systems provide this feature. In order for an IPv6 socket to be hybrid, the option IPv6_V6ONLY must be available and set to 0.

In the current version of Transfer CFT, this option is not changed by the software. As a result, IPv6 listening sockets operate with the default value assigned by the system. This difference leads to two different behaviors for Transfer CFT server programs using an IPv6 socket when listening for incoming connections:

- If the OS implements a hybrid dual-stack and provides the IPv6_V6ONLY socket option, and if the default system value for the IPv6_V6ONLY option is zero, then the Transfer CFT server programs listening on an IPv6 socket will receive both IPv6 and IPv4 incoming connections.
- Otherwise, Transfer CFT server programs listening on an IPv6 socket will only receive IPv6 connections.

<span id="Regulati"></span>

### Tuning IPv6

If IPv6 is not properly configured, performance may be affected. Use the following UCONF parameters to regulate numeric host name and service name resolution, and revert to IPv4 functionality if required.

Tuning parameters


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.ipv6.set_ai_numerichost | Yes  |  • ****Yes****: Use when the host name is numeric to prevent the API system getaddrinfo from performing unnecessary DNS requests for numeric hostnames.<br/> • ****No****: Use DNS requests for all hostnames, including numeric. |
| cft.ipv6.set_ai_numericserv  | Yes  |  • ****Yes****: Use when the service name is numeric (port number) to prevent the API system getaddrinfo from performing an unnecessary service name translation.<br/> • ****No****: The getaddrinfo system API will perform a service name translation even if unnecessary. |
| cft.ipv6.use_ipv4_legacy_resolver  | No  |  • ****Yes****: Use legacy IPv4 only with the host and service names resolution APIs, gethostbyname() and getservbyname(). This detects if a performance issue involves IPv6 specific elements such as configuration settings, system API implementation, etc.<br/> • ****No****: Use IPv6 functionality. |


### IPv6 support

{{< TransferCFT/axwayvariablesComponentShortName  >}} provides support for both IPV4 and IPV6.

Enable IPv6


| Parameter  | Description  |
| --- | --- |
| ipv6.disable_connect | **No** indicates that an address or a name used by {{< TransferCFT/axwayvariablesComponentShortName  >}} to connect to a host may be either an IPV4 or an IPV6 address. When using a name, this parameter can refer to a list of addresses, of any type. |
| ipv6.disable_listen  | **No** indicates that an address or name used by {{< TransferCFT/axwayvariablesComponentShortName  >}} to listen for incoming connections may be either an IPV4 or an IPV6 address.<br/> When using a name, this parameter can refer to a list of addresses, of any type. |


> **Note**
>
> Note: It is recommended that you do not set ipv6.disable_listen to No, and ipv6.disable_connect to Yes.

<span id="Reference materials"></span>

Reference materials
-------------------

Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} code changes are based on the following documentation:

- RFC 3493 *Basic Socket Interface Extensions for IPv6*
- IEEE-1003.1-2004 *System Interfaces*

****Related topics****

- [Command guide (CFTNET)](../../c_intro_userinterfaces/command_summary)
- [Unified configuration (CFTUTIL)](../../admin_intro/uconf/uconf_w_cftutil)
- [Unified configuration (GUI)](../../admin_intro/uconf/uconf_userinterface)
