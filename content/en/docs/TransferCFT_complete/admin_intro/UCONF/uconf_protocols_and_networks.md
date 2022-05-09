{
    "title": "UCONF: IPv6  options",
    "linkTitle": "IPv6  options",
    "weight": "290"
}{{< TransferCFT/axwayvariablesComponentShortName  >}} provides support for both IPV4 and IPV6.

### Enable IPv6


| Parameter  | Description  |
| --- | --- |
| ipv6.disable_connect | **No** indicates that an address or a name used by {{< TransferCFT/axwayvariablesComponentShortName  >}} to connect to a host may be either an IPV4 or an IPV6 address. When using a name, this parameter can refer to a list of addresses, of any type. |
| ipv6.disable_listen  | **No** indicates that an address or name used by {{< TransferCFT/axwayvariablesComponentShortName  >}} to listen for incoming connections may be either an IPV4 or an IPV6 address.<br/> When using a name, this parameter can refer to a list of addresses, of any type. |


> **Note**
>
> Note: It is recommended that you do not set ipv6.disable_listen to No, and ipv6.disable_connect to Yes.

### Advanced IPv6

Refer to the [UCONF parameters](../uconf_directory) table `ipv6.set_ai_xxx`.


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.ipv6.set_ai_numerichost | Yes  |  • ****Yes****: Use when the host name is numeric to prevent the API system getaddrinfo from performing unnecessary DNS requests for numeric hostnames.<br/> • ****No****: Use DNS requests for all hostnames, including numeric. |
| cft.ipv6.set_ai_numericserv  | Yes  |  • ****Yes****: Use when the service name is numeric (port number) to prevent the API system getaddrinfo from performing an unnecessary service name translation.<br/> • ****No****: |
| cft.ipv6.use_ipv4_legacy_resolver  | No  |  • ****Yes****: Use legacy IPv4 only host and service names resolution API, namely gethostbyname() and getservbyname(). This detects if the performance issue involves new IPv6 specific material as configuration items, new system API implementation, etc.<br/> • ****No****: Use IPv6 functionality. |

