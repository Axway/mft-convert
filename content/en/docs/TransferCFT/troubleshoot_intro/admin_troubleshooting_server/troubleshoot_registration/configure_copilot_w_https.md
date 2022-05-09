{
    "title": "Configuring Copilot with HTTPS",
    "linkTitle": "Install a certificate on the server",
    "weight": "310"
}The certificates used by the Transfer CFT Copilot server to authenticate itself are defined in UCONF using the parameters described in the following tables.

The examples provided in this section use sample certificates that are supplied in product. Do not use these certificates in production; you should instead use your own certificates.

******Example 1******

This example uses a single PKCS\#12 certificate.


| Parameter | Value |
| --- | --- |
| copilot.ssl.SslCertFile<br/>  | conf/pki/MFT_Demonstration_User_Certificate.p12 |
| copilot.ssl.SslCertPassword<br/>  | Certificate password (“user” for the sample above)<br/>  |
| copilot.ssl.SslKeyFile<br/>  | Not used |
| copilot.ssl.SslKeyPassword<br/>  | Not used |


******Example 2******

This example uses a DER certificate with the private key in a separate DER file.


| Parameter | Value |
| --- | --- |
| copilot.ssl.SslCertFile<br/>  | conf/pki /MFT_Demonstration_User_Certificate.der<br/>  |
| copilot.ssl.SslCertPassword<br/>  | Not used |
| copilot.ssl.SslKeyFile<br/>  | conf/pki /MFT_Demonstration_User_Certificatek.der |
| copilot.ssl.SslKeyPassword<br/>  | Key file password (no password with sample file above) |


### Additional HTTPS parameters

There are two additional UCONF parameters to use for https connections:


| Parameter | Value |
| --- | --- |
| copilot.ssl.SslCipherSuites<br/>  | A comma separated list of cipher suites accepted by the Copilot server.<br/> • “47, 10, 9, 2”: Default value.<br/> <br/> List of supported cipher suites:<br/> • 1 = RSA_WITH_NULL_MD5<br/> • 2 = RSA_WITH_NULL_SHA<br/> • 4 = RSA_WITH_RC4_MD5<br/> • 5 = RSA_WITH_RC4_SHA<br/> • 9 = RSA_WITH_DES_CBC_SHA1<br/> • 10 = RSA_WITH_3DES_EDE_CBC_SHA<br/> • 47 = RSA_WITH_AES_128_CBC_SHA<br/> • 53 = RSA_WITH_AES_256_CBC_SHA<br/> • 59 = RSA_WITH_NULL_SHA256<br/> • 60 = RSA_WITH_AES_128_CBC_SHA256<br/> • 61 = RSA_WITH_AES_256_CBC_SHA256 |

