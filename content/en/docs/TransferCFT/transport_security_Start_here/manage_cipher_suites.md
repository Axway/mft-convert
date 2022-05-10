---
    "title": "Manage cipher suites",
    "linkTitle": "Manage cipher suites",
    "weight": "180"
---
This section describes how to define the cipher suites that can be used for secure file transfers, governance exchanges ({{< TransferCFT/PrimaryCGorUM  >}}, Sentinel, etc.), and when Transfer CFT is server (for example, when acting as an API server).

Supported cipher suites
-----------------------

{{< TransferCFT/axwayvariablesComponentLongName  >}} supports the cipher suites listed below, and prioritizes them as displayed in the **Order used** column (the {{< TransferCFT/axwayvariablesComponentLongName  >}} order overrides your cipher suite order). The order, between two approximate levels of security, favors the cipher suite that provides a better level of performance.

{{% TransferCFT/snippets/cipher_suites%}}

Set the cipher suites for file transfers
----------------------------------------

Refer to *Transport security in CFTSSL* for details on how to specify the cipher suites for file transfers.

Set the cipher suites for governance
------------------------------------

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

Perfect Forward Secrecy support
-------------------------------

Transfer CFT supports Perfect Forward Secrecy (PFS) with ECDHE_RSA cipher suites. PFS ensures that sessions using long-term public and private keys will not be compromised if one of the private keys is compromised in the future. With PFS, systems can negotiate new keys for every communication, and if a key is compromised only that specific session is vulnerable.

For more information, refer to [www.perfectforwardsecrecy.com](https://www.perfectforwardsecrecy.com/)
