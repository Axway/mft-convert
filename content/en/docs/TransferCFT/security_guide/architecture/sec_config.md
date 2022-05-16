---

    title: Configuration
    linkTitle: Configuration
    weight: 90

---
<span id="__RefHeading___Toc473905785"></span>

## Global SSL/TLS parameters

For all used SSL/TLS, you can use the following uconf parameters:

- `ssl.version_min`: Specify the minimum SSL/TLS version allowed by Transfer CFT connectors.
- `ssl.ciphersuites`: Specify the SSL/TLS cipher suites negotiated by Transfer CFT connectors.
- `cft.ssl.version_min`: Specify the minimum SSL/TLS version allowed by Transfer CFT for file transfers.
- `cft.ssl.version_max`: Specify the maximum/TLS SSL version allowed by Transfer CFT for file transfers.

Notably and by default, the uconf `copilot.ssl.version_min` value is set to point to the `ssl.version_min` value. However, this can be overridden.

### How to secure exchanges with Central Governance / Flow Manager

Connection between Transfer CFT and Central Governance are secured by default using SSL/TLS. At the registration, Transfer CFT generates a CSR (Certificate Sign Request), which is signed by Central Governance with a dedicated CA certificate. In return Transfer CFT gets a certificate chain that is used to authenticate itself for all connections to Central Governance, PassPort and Sentinel. The connections are SSL/TLS secured with mutual authentication.

Set the CA certificate used to authenticate Central Governance:

- Import the CA certificate of Central Governance in the internal PKI database using the PKIUTIL pkicer command (refer to the Transfer CFT 3.9{{< TransferCFT/axwayvariablesReleaseNumber >}} User Guide).
- Reference the imported certificate Identifier using the following parameter: uconf:cg.ca_cert_id  
    ****Example****  
    CFTUTIL uconfset id=cg.ca_cert_id, value=CGCA

### How to SSL/TLS secure the UI

Connections to the Transfer CFT Copilot server are secured by default. To enable HTTP for this connection, set the uconf parameter` copilot.http.onlyssl` to `No`. For more information, please refer to the [Transfer CFT User Guide &gt; Administration &gt; Manage the servers &gt; Manage the Copilot server](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/administration/manage_copilot.htm) &gt; *Configure Copilot with SSL security* section.

<span id="__RefHeading___Toc473905788"></span>

## Secure partner connections for file transfers with Central Governance / Flow Manager

If you are managing your flows using Central Governance, please read the associated documentation about securing the flows in the Central Governance User Interface. This section describes how to secure the flows while not using Central Governance.

## Secure partner connections without Central Governance / Flow Manager

Flows can be secured with SSL/TLS. To secure your connection, on each side you must define a security profile using the CFTSSL object, which describes how to secure the connection. The SSL object references to CA certificate to trust, and certificate-key pairs. Depending on the uconf `pki.type` parameter, the certificate and private data is fetched using the IDs in the internal PKI database (pki.type=cft), the PassPort PS (pki.type=passport), system PKI database on ZOS (pki.type=system), custom user exit (pki.type=exit).  
Please refer to the *Transfer CFT Users Guide* ****Security &gt; Configuring transport security &gt; CFTSSL configuration**** for details.

The CFTSSL object ID must be referenced as the SSL parameter of the CFTPROT object for the server configuration, or the CFTPART object for the client configuration. Additionally, on the server additional partner-related control can be performed after SSL/TLS handshake by setting the ID of a CFTSSL object in the SSL parameter of the associated CFTPART object whose DIRECT parameter is set to SERVER. Please refer to the *Transfer CFT Users Guide* ****Security &gt; Configuring transport security &gt; Start here**** for more information.

The command used to insert keys and trusted certificate in internal the PKI database is using the PKIUTIL command. Please refer to the *Transfer CFT Users Guide* ****Security &gt; Managing certificates &gt; Command line operations &gt; Start here**** for more information on how to manage certificates and private keys in the internal PKI database.

<span id="__RefHeading___Toc473905789"></span>

## Secure by default configuration

Connections to Axway products are secured by default in a Central Governance or Flow Manager context. Connections with the UI are secured by default, though connections with the partners are not secured by default. Please refer to the previous section for information on implementing SSL/TLS for these connections.

<span id="__RefHeading___Toc473905790"></span>

## Product certificates

To authenticate Central Governance and other Axway products, the certificate authority CN=PassPortCA is used by default. This certificate expires the November 28, 2019.

In addition, this certificate authority also signs the PassPort Product CA which generates certificates used by Transfer CFT to authenticate itself for governance connections (connections to other Axway products) and for file transfers with partners.

Best practices are to change the following:

- The certificate authority PassPort Product CA on Central Governance; refer to the *Central Governance* or *Flow Manager Security Guide* > ****Product certificate**** to change this certificate.
- The certificate authority PassPort CA on Transfer CFT:
    -   Import in the internal PKI database the certificate authority to use using the PKIUTIL pkicer command.
    -   Reference the imported certificate Identifier using the following parameter: uconf:cg.ca_cert_id

      
    CFTUTIL uconfset id=cg.ca_cert_id, value=CGCA
