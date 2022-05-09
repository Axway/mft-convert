{
"title": "Configure virtual hosts",
  "linkTitle": "Configure virtual hosts",
  "weight": 2,
  "date": "2019-10-17",
  "description": "Configure virtual hosts for HTTP services or REST APIs."
}
A virtual host is a server, or pool of servers, that can host multiple domain names (for example, `company1.api.example.com` and `company2.api.example.com`). This enables you to run more than one website, or set of REST APIs, on a single host machine (for example, `192.0.2.11`). Each domain name can have its own host name, paths, APIs, and so on. For example:

```
https://company1.api.example.com:8080/api/v1/test
https://company2.api.example.com:8080/api/v2/test
https://company3.api.example.com:8080/api/v2/test
```

The API Gateway implements *name-based* virtual hosting, in which the client HTTP `Host` header is used as the routing criteria during path resolution (for example, `Host company1.api.example.com`). This means that you can have multiple domains running on the same hardware (IP address), while this is not apparent to the client or end user.

For example, the following URL invokes the `company2.api.example.com` virtual host:

```
https:/company2.api.example.com/api/v1/test
```

This results in the following message at runtime:

```
POST /api/v1/test HTTP/1.1
Host:company2.api.example.com
Content-Type:application/x-www-form-urlencoded
client_id=SampleConfidentialApp&client_secret=......
```

{{< alert title="Note" color="primary" >}}To support name-based virtual hosts, you must first ensure that your Domain Name System (DNS) server has been updated to map each host name to the correct IP address (for example, `*.example.com`
is mapped to `192.0.2.11`). For more details on configuring a DNS service with wildcards for virtual hosting, see the
[API Gateway Administrator Guide](/docs/apim_administration/apigtw_admin/). When your DNS server has been updated to map each host name to the correct IP address, you can then configure the API Gateway for virtual hosting.{{< /alert >}}

## Configure virtual hosts for HTTP services

You can configure virtual hosts at the HTTP service level. This means that these settings are applied to the HTTP service and to any child resolvers. To configure a virtual host at the HTTP service level, perform the following steps:

1. In the Policy Studio tree, select an HTTP service (for example, **Environment Configuration** > **Listeners** > **API Gateway** > **Default Services** > **Virtual Hosts**).
2. Right-click, and select **Add a Virtual Host**.
3. Configure the following settings in the **Virtual Host** dialog:

   * **Name**: Enter a unique name of the virtual host.
   * **Enabled**: Select whether the virtual host processing is enabled. This is enabled by default.
   * **Hosts**: Specify the list of domains that you wish to host under this HTTP service. To add a host, click **Add** at the bottom right, and enter the domain name (for example `company1.api.example.com`). You can also specify domain names using wildcards already configured in your DNS (for example `*.example.com:/8080` or `company3.api.example.com.*`).

### Configure child resolvers

When you have configured a virtual host at the HTTP service level, you can also configure the following child resolvers:

* Relative path
* Static content provider
* Static file provider
* Servlet application

To configure a child resolver, perform the following steps:

1. In the Policy Studio tree, select the **Paths** node under the virtual host, and right-click to add a resolver (for example, **Add relative path**).
2. In the **Resolve path to policies** dialog, click the browse button beside the **Path Specific Policy** field, and select a policy to run on this path.

For example, if an inbound request to `/Healthcheck` matches on `company1.api.*`, the **Company1 Health Check** policy is executed. Otherwise, the global **Health Check** policy for the HTTP service is executed (in this case, **Default Services**).

![Configuring a Virtual Host Resolver](/Images/docbook/images/virtual_hosts/v_host_child_resolver.png)

## Configure virtual hosts for REST APIs

When API Manager has been installed, you can also configure virtual hosts for specific REST APIs. By default, the HTTP service-level profile is used, but you can override the virtual host at the REST API level.

For example, when adding a new REST API in the **New Rest API** dialog, you can specify virtual hosts on the **Exposure** tab. In the **Virtual Host** field, click the browse button to select a virtual host in the dialog.

{{< alert title="Tip" color="primary" >}}The virtual hosts listed are automatically filtered based on the HTTP service selected on the **Exposure** window (for example, **Default Services** ). {{< /alert >}}
If no virtual host has already been configured, right-click the HTTP service node (for example, **Default Services**), and select **Add a Virtual Host**.

![Virtual Hosts for REST API](/Images/docbook/images/virtual_hosts/v_host_rest_api.png)

In addition, when editing an existing REST API, you can also specify a virtual host in the **Edit Rest API** dialog.

Finally, when the API has been created, you can view it in the **Resolver** screen for its virtual host. The following shows a simple Flickr API example:

![Virtual Hosts Resolver for REST API](/Images/docbook/images/virtual_hosts/v_host_rest_api_path.png)