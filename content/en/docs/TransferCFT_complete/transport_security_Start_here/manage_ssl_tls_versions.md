{
    "title": "Managing SSL and TLS versions",
    "linkTitle": "Manage SSL and TLS versions",
    "weight": "170"
}This section describes how to manage the SSL and TLS versions, and enable compatibility mode if needed. Additionally, it provides details on TLS use with {{< TransferCFT/axwayvariablesComponentLongName  >}}.

SSL/TLS parameters
------------------

The following parameters manage SSL and TLS versions in Transfer CFT:

VERSION: This CFTSSL object parameter sets the SSL version, where:

- TLSV1 and TLSV1COMP values correspond to TLS version 1.2, 1.1, 1.0.
- SSLV3 and SSLV3COMP values correspond to SSL version 3.0.

cft.ssl_version_min: This UCONF parameter sets the minimum SSL version that Transfer CFT will accept when communicating with another partner (as either client or server).

cft.ssl_version_max: This UCONF parameter sets the maximum SSL version that Transfer CFT will accept when communicating with another partner, as either client or server. The default value for this parameter is TLS 1.2.

### About {{< TransferCFT/headerfootervariableshflongproductname  >}} SSL connections &lt;/h3&gt;

In Transfer CFT, the SSL/TLS version is proposed by the client and negotiated with the server.

**Client mode**

`DIRECT=CLIENT`

In client mode, TLSV1COMP or SSLV3COMP sets the header length in the NSDU to enable compatibility with other products.

**Server mode**

`DIRECT=SERVER`

In server mode, the header length is automatically detected for all SSL versions (SSLV3, TLSV1, SSLV3COMP, TLSV1COMP).

> **Note**
>
> Note: It is recommended that you set the server to use TLSV1COMP.

Enable TLS 1.1 and 1.2
----------------------

The Transfer CFT SSL version is by default set to TLS 1.0 when transferring files. To use a higher version of TLS, set the cft.ssl.version_max parameter to the appropriate version. For example, to use TLS version 1.2 set the UCONF value to cft.ssl.version_max=tls_1.2.

However, by default Copilot and {{< TransferCFT/PrimaryCGorUM  >}} use TLS 1.2 for all connections.

Interoperability with Transfer CFT versions lower than 3.2.1
------------------------------------------------------------

In order to comply with security standards, as of version 3.2.1 Transfer CFT restricts the use of the cipher suites 59, 60, and 61 only to TLS 1.2. Interoperability with Transfer CFTs that have a version lower than 3.2.1 is not possible using these cipher suites.

In both the current and previous version, downgrade to the same cipher suite if you were using the ciphers suites 59, 60 and 61.

Set the minimum SSL/TLS version
-------------------------------

This section describes how to specify the minimum SSL/TLS protocol version using the following parameters:

- ssl.version_min
- cft.ssl.version_min
- copilot.ssl.version_min

It is important to note that the Transfer CFT SSL/TLS implementation allows a fallback to SSL 3.0. This means that even if you configure TLS 1.x in the security profile (the `version `parameter in CFTSSL), if the remote partner only supports SSL 3.0 your exchanges with this partner will occur using the SSL 3.0 protocol. This behavior offers backward compatibility with SSL 3.0 to allow for interoperability with legacy systems.

**Procedure**

You can use these UCONF parameters to define the minimum SSL/TLS protocol version (setting these parameters to tls_1.0, for example, disables SSL 3.0 connections):


| Parameter  | Description  | Type  | Possible values  | Default value  |
| --- | --- | --- | --- | --- |
| ssl.version_min  | Minimum SSL version allowed by the access management connector.  | enum  | ssl_3.0, tls_1.0, tls_1.1, tls_1.2  | tls_1.0  |
| cft.ssl.version_min | Minimum SSL version allowed by the Transfer CFT server. Note that if default value for this parameter is not set, it uses the ssl.version_min value. | enum | ssl_3.0, tls_1.0, tls_1.1, tls_1.2  | $(ssl.version_min)  |
| copilot.ssl.version_min | Minimum SSL version allowed by the Transfer CFT Copilot server. Note that if default value for this parameter is not set, it uses the ssl.version_min value. | enum  | ssl_3.0, tls_1.0, tls_1.1, tls_1.2  | $(ssl.version_min)  |


#### Related information

For *SSL 3.0 Protocol Vulnerability and POODLE attack* (CVE-2014-3566) details, refer to: <https://www.us-cert.gov/ncas/alerts/TA14-290A>

TLS Server Name Indication (SNI)
--------------------------------

Server Name Indication (SNI) is an extension of the TLS protocol, and is used to indicate which hostname the client attempts to connect to at the beginning of the handshake process. In Transfer CFT, set the UCONF parameter `ssl.extension.enable_sni `to `yes `to enable SNI for all TLS connections.

TLS limitation
--------------

Transfer CFT 3.2.1 supports TLS 1.2, which is enabled by default. However, TLS renegotiation is not supported.
