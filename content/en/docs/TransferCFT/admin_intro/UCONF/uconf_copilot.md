---
title: "UCONF: Copilot server"
linkTitle: "Transfer CFT UI server"
weight: 280
--- ****UNIX****

Refer to the [UCONF parameters](../uconf_directory) table for information on `copilot.*.unix `parameters.

****Alias management****

You can access customized file system directories via the {{< TransferCFT/axwayvariablesComponentShortName  >}} user interface HTTP server using aliases.

To add a new alias, access the Unified Configuration (uconf) and configure the following:

| ID  | Description  |
| - - - | - - - |
| copilot.http.aliases  | List of enabled alias- id  |
| copilot.http.aliases.(alias- id) alias  | Name of the alias  |
| copilot.http.aliases.(alias- id).path  | Path that replaces the alias in the URL  |

****Security for Cop**i**lot u****

| Parameter  | Description  |
| - - - | - - - |
| copilot.http.onlyssl  | Enter Yes to restrict the access of the {{< TransferCFT/axwayvariablesComponentShortName  >}} user interface with https.  |

****View available drives****

To view available drives from the ****Edit a file**** icon in the graphical user interface, define the following:

| Parameter  | Options  | Description  |
| - - - | - - - | - - - |
| copilot.nt.rootdrives  | @REMOVABLE_DRIVES  | To view removable drives such as a USB key, CD, and so on.  |
| - " - | @LOCAL_DRIVES  | To view hard drives.  |
| - " - | @NET_DRIVES  | To view network drives.  |

****Client keep- alive****

Use this parameter to define the keep- alive interval in seconds for a client session. By default this occurs every 60 seconds.

| Parameter  | Value  |
| - - - | - - - |
| copilot.misc.client_keep_alive_delay  | Enter an integer for the delay in seconds.<br/> 60 = default<br/> 0 = no keep- alive |

****Client timeout****

Use this parameter to define the client timeout in minutes. The default value is 30 minutes.

| Parameter  | Value  |
| - - - | - - - |
| copilot.misc.ClientTimeout  | Enter an integer for the timeout in minutes.<br/> 30 = default<br/> 0 = no timeout |

****Web services****

Use this parameter to define the {{< TransferCFT/axwayvariablesComponentShortName  >}} Web Services. See also [Setting up Web Services](../../../cft_intro_install/about_this_document_ibmi/using_apis/about_web_services).

| Parameter  | Value  | Former value  |
| - - - | - - - | - - - |
| copilot.webservices.wsicomplience  | (bool) No  | [WEBSERVICES] WsiComplience  |
| copilot.webservices.upload_directory  | (dir) $(cft.runtime_dir)/conf/ws_upload  | NA  |

****Configure Copilot with HTTPS****

This section describes how to install the certificates that are required to enable HTTPS connections for Copilot. The basic steps are:

- Install a certificate on the server
- Install a certificate on the client

### Install a certificate on the server

The following tables describe the UCONF parameters that determine the certificates used by the Transfer CFT Copilot server to authenticate itself.

You can use the following certificate and private key formats, where the format of the certificate may differ from that of the key.

The certificate type is dictated by the file name extension (.p12, .pkcs12, .der, .pem, for example `my_certificate.pem`).

*For native files in a z/OS or IBM i environment*, Transfer CFT first tries to decode the certificate in PEM format. If the format cannot be determined, the last letters of the file name are used as the suffix. IBM i also handles the certificates stored on the IFS partition.

| Supported format  | Type  | Extension  |
| - - - | - - - | - - - |
| Certificate  | PKCS#12  | p12, pfx, pkcs12  |
| Certificate  | PEM  | pem  |
| Certificate  | DER  | der  |
| Private key  | PEM  | pem  |
| Private key  | DER  | der  |
| Private key  | PKCS#8  | key, pem  |

****How to define a PKCS#12 certificate****

This example uses a single PKCS#12 certificate where you only require the file name and password.

| Parameter | Value |
| - - - | - - - |
| copilot.ssl.SslCertFile | conf/pki/&lt;my_certificate&gt;.p12 |
| copilot.ssl.SslCertPassword | Certificate password |
| copilot.ssl.SslKeyFile | Not used |
| copilot.ssl.SslKeyPassword | Not used |

****How to define a DER or PEM certificate****

This example uses a DER(or PEM) certificate with the private key in a separate DER file, where you define the key as well as the certificate.

| Parameter | Value |
| - - - | - - - |
| copilot.ssl.SslCertFile | conf/pki /&lt;my_certificate&gt;.der *or* .pem |
| copilot.ssl.SslCertPassword | Not used |
| copilot.ssl.SslKeyFile | conf/pki /&lt;my_key&gt;.der *or* .pem |
| copilot.ssl.SslKeyPassword | Key password, which is mandatory if the key file is encrypted PKCS#8 |

#### Additional HTTPS parameters

There are two additional UCONF parameters to use for HTTPS connections:

| Parameter | Value |
| - - - | - - - |
| copilot.http.onlyssl |  • No: Default value.<br/> • Yes: Restricts access to the Transfer CFT Copilot server to HTTPS secured connections only. |
| <span id="copilot.ssl.SslCipherSuites"></span>copilot.ssl.SslCipherSuites<br/>  | A comma separated list of cipher suites accepted by the Transfer CFT Copilot server.<br/> • “47, 10, 9, 2”: Default value.<br/> <br/> List of supported cipher suites:<br/> • 1 = RSA_WITH_NULL_MD5<br/> • 2 = RSA_WITH_NULL_SHA<br/> • 4 = RSA_WITH_RC4_MD5<br/> • 5 = RSA_WITH_RC4_SHA<br/> • 9 = RSA_WITH_DES_CBC_SHA1<br/> • 10 = RSA_WITH_3DES_EDE_CBC_SHA<br/> • 47 = RSA_WITH_AES_128_CBC_SHA<br/> • 53 = RSA_WITH_AES_256_CBC_SHA<br/> • 59 = RSA_WITH_NULL_SHA256<br/> • 60 = RSA_WITH_AES_128_CBC_SHA256<br/> • 61 = RSA_WITH_AES_256_CBC_SHA256 |
| copilot.ssl.version_min  | Indicates the minimum version of SSL that the Copilot and the REST API server accept.<br/> • Default: tls_1.0<br/> • Possible values are: ssl_3.0 (not recommended), tls_1.0, tls_1.0, tls_1.2 |

