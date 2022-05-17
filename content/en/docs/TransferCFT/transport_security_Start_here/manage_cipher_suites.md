---
    title: "Manage cipher suites"
    linkTitle: "Manage cipher suites"
    weight: 180
---This section describes how to define the cipher suites that can be used for secure file transfers, governance exchanges ({{< TransferCFT/PrimaryCGorUM  >}}, Sentinel, etc.), and when Transfer CFT is server (for example, when acting as an API server).

## Supported cipher suites

{{< TransferCFT/axwayvariablesComponentLongName  >}} supports the cipher suites listed below, and prioritizes them as displayed in the **Order used** column (the {{< TransferCFT/axwayvariablesComponentLongName  >}} order overrides your cipher suite order). The order, between two approximate levels of security, favors the cipher suite that provides a better level of performance.


| Suite  | Order used | Authentication  | Confidentiality  | Integrity  |
| --- | --- | --- | --- | --- |
| 49199 **  | 1  | ECDHE + RSA authentication  | AES-128 GCM  | SHA-256  |
| 49200 **  | 2  | ECDHE + RSA authentication  | AES-256 GCM  | SHA-384  |
| 49191 **  | 3  | ECDHE + RSA authentication | AES-128  | SHA-256  |
| 49192**  | 4  | ECDHE + RSA authentication  | AES-256  | SHA-384  |
| 156 **  | 5  | RSA authentication  | AES 128 GCM  | SHA-256  |
| 157 **  | 6  | RSA authentication  | AES 256 GCM  | SHA-384  |
| 60*  | 7  | RSA authentication (512, 1024, 2048, or 4096)  | AES-128  | SHA-256  |
| 61*  | 8  | RSA authentication (512, 1024, 2048, or 4096)  | AES-256  | SHA-256  |
| 47 &lt;/td&gt;  | 9  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | AES-128 &lt;/td&gt;  | SHA-1 &lt;/td&gt;  |
| 53  | 10  | RSA authentication (512, 1024, 2048, or 4096)  | AES-256  | SHA-1  |
| 10 &lt;/td&gt;  | 11  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | Triple DES &lt;/td&gt;  | SHA-1 &lt;/td&gt;  |
| 5 &lt;/td&gt;  | 12  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | RC4 &lt;/td&gt;  | SHA-1 &lt;/td&gt;  |
| 4 &lt;/td&gt;  | 13  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | RC4 &lt;/td&gt;  | MD5 &lt;/td&gt;  |
| 59*  | 14  | RSA authentication (512, 1024, 2048, or 4096)  | None  | SHA-256  |
| 2 &lt;/td&gt;  | 15  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | None &lt;/td&gt;  | SHA-1 &lt;/td&gt;  |
| 1 &lt;/td&gt;  | 16  | RSA authentication (512, 1024, 2048, or 4096) &lt;/td&gt;  | None &lt;/td&gt;  | MD5 &lt;/td&gt;  |


> **Note**
>
> \* To comply with security standards, as of Transfer CFT version 3.2.0 the use of the cipher suites 59, 60, and 61 is restricted to TLS 1.2 exclusively. This means that you cannot negotiate a session with another partner (monitor) that is using a TLS version lower than 1.2 with these cipher suites.

> **Note**
>
> \*\* These cipher suites are only available for Transfer CFT 3.2.2 and higher and are restricted to use with TLS 1.2.

## Set the cipher suites for file transfers

Refer to *Transport security in CFTSSL* for details on how to specify the cipher suites for file transfers.

## Set the cipher suites for governance

This section describes how to specify the cipher suites for governance or when Transfer CFT is acting as an API server:

- ssl.ciphersuites
- copilot.ssl.sslciphersuites

**Procedure**

<span id="cipher_suites"></span>You can use these UCONF parameters to define the cipher suites:


| Parameter  | Description  | Type  | Default value  |
| --- | --- | --- | --- |
| ssl.ciphersuites  | Defines the cipher suite to be used between the Transfer CFT connectors.  | int_list  | 49200, 49199, 156, 157, 60, 61, 47, 53  |
| copilot.ssl.sslciphersuites | Defines the cipher suite to be used between the Transfer CFT UI client and UI server. | int_list  | $(ssl.ciphersuites)  |


<span id="Perfect"></span>

## Perfect Forward Secrecy support

Transfer CFT supports Perfect Forward Secrecy (PFS) with ECDHE_RSA cipher suites. PFS ensures that sessions using long-term public and private keys will not be compromised if one of the private keys is compromised in the future. With PFS, systems can negotiate new keys for every communication, and if a key is compromised only that specific session is vulnerable.

For more information, refer to [www.perfectforwardsecrecy.com](https://www.perfectforwardsecrecy.com/)
