---
title: Configure TLS authentication with API Manager
linkTitle: Configure TLS authentication with API Manager
weight: 75
date: 2020-06-22
description: Configure API Portal and API Manager Mutual TLS authentication to increase the level of security of your API Portal.

---
Configure API Portal and API Manager Mutual TLS authentication to increase security in API Portal and API Manager connection. The mutual authentication ensures that traffic is secure between the server (API Manager), which presents its certificate to the client, and the client (API Portal), which presents its own certificate for the server's verification.

Before you start you must have a TLS certificate, which presents the identity of API Portal to API Manager. To configure API Manager to require and verify this certificate, see [Set up the back-end SSL server for mutual authentication](/docs/apim_administration/apimgr_admin/api_mgmt_custom_policies/#set-up-the-back-end-ssl-server-for-mutual-authentication).

## Configure API Portal to present its certificate

1. Log in to the Joomla! Administrator Interface (JAI), and click **Components** > **API Portal** > **API Manager**.
2. On section **Mutual Authentication**, change **Send API Portal Certificate to API Manager** to **Yes**.
    * **API Portal (Client) Certificate** - Click **Choose File** and select the certificate file from your local file system. The supported file extensions for the certificate files are `.cer`, `.crt`, `.pem`, and `.der`.
    * **API Portal (Client) Private key** - Click **Choose File** and select the private key file for the certificate from your local file system. The supported file extensions for the private key is `.key`.
    * **Private Key Passphrase** - If your private key is passphrase protected, type your passphrase in the text field (the text is masked). Leave the field empty if there is no passphrase on your private key.
3. Click **Save**.

If you have multiple API Managers configured, API Portal HTTP client will present the certificate for every one of them. It is up to each API Manager whether to require and verify this certificate or not. No performance impact is expected.

## Expose a new port for connection with API Portal

Once you configure API Manager to require client certificate, every client that is connecting on port `8075` (or whatever default port is configured) will present its certificate. This includes not only API Portal as a client, but also the browser.

To avoid any issues with connecting to API Manager from the browser, we recommend you to:

* Expose a new port for mutual authentication connection with API Portal.
* Configure API Manager to require client certificate for connection on this new port.
* Leave the default port (`8075`) in the same state as it was until now, so the browser continues to connect to API Manager on the default port.

For more details on how to expose a new port for API Manager, see [Add Http or Https Interface](/docs/apim_policydev/apigw_gw_instances/general_services/#http-and-https-interfaces).

Once you expose a new port, you must change it in the API Portal configuration:

1. In JAI, click **Components > API Portal > API Manager > Port**.
2. Update the port, and click **Save**.
