{
"title": "Configure HTTP Strict Transport Security",
  "linkTitle": "Configure HTTP Strict Transport Security",
  "weight": 150,
  "date": "2021-02-26",
  "description": "Create an HSTP profile and configure it for HTTP services."
}
HTTP Strict Transport Security (HSTS) is a browser security mechanism, implemented by way of an HTTP `Strict-Transport-Security` response header, which allows the connection with a website through secure connections (HTTPS) only.

HSTS enforces security on applications by mitigating attacks such as man-in-the-middle, insecure link referencing, and invalid certificates.

## Add an HSTS profile

To create an HSTS to your domain, you must first add an HSTS profile in Policy Studio.

1. In Policy Studio, click **Environment Configuration > Libraries > HSTS Profiles**.
2. Right-click **HSTS Profiles**, and select **Add an HSTS Profile**.
3. Configure the following settings:

   * **Name**: Unique name of the HSTS profile. Defaults to HTTP Strict Transport Security.
   * **Enable HSTS**: Specifies whether HSTS processing is enabled for the profile. Defaults to enabled.
   * **HSTS Parameters:**
     * **max-age (seconds)**: Specifies the time, in seconds, that the browser should remember that a site is only to be accessed using HTTPS.
     * **includeSubDomains**: Applies the new profile to all of the site's subdomains. Defaults to enabled.
4. Click **OK**.

## Configure an HSTS profile

The HSTS profile is not configured in SSL interfaces by default. To enable it, for example, for API Gateway, perform the following steps:

1. In Policy Studio, click **Environment Configuration > Listeners > API Gateway > Default Services > Ports**.
2. Right-click the HTTPS interface you wish to update, and click **Edit**.
3. Click the **Advanced** tab, and under **HSTS Settings**, select the HSTS profile you wish to use to protect all endpoints serviced by this interface.
4. Click **OK**.

![To enable HSTS profile](/Images/docbook/images/general/hsts5.png)

After the changes have been deployed, all the responses from the interface will contain the `Strict-Transport-Security` header.

{{% alert title="Note" %}}
When you enable HSTS, it takes effect for the entire domain regardless of any other ports configured on the listeners. Any non-SSL ports that exist in the configuration (listeners) will no longer work from the browser after an HSTS-protected interface has been invoked from the browser.

HSTS is made redundant when self-signed certificates are employed.
{{% /alert %}}

By default, the following ports use self-signed certificates:

* API Manager UI(8075)
* API Manager traffic (8065)
* API Gateway Manager UI (8090)
* API Gateway Analytics (8040)
* Client Application Registry (8089)