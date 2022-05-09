{
"title": "Configure relative paths",
  "linkTitle": "Configure relative paths",
  "weight": 2,
  "date": "2019-10-17",
  "description": "Configure relative path resolvers to invoke a policy when a request arrives on a specific path."
}
A *relative path* binds policies to a specific relative path location (for example `/test/path`). When the API Gateway receives a request on the specified path, it invokes the specified policy or policy chain.

You can use a *Static Content Provider* or *Static File Provider* to serve static HTTP content or files from a directory. In this case, the API Gateway instance acts as a web server. The API Gateway instance can also act as a *Servlet Application* container, which enables you to drop servlets into the HTTP Services configuration. This should only be used by developers with specific requirements under strict advice from Axway Support.

Relative paths can have nested child resolvers of the following type:

* Relative path
* Static content provider
* Static file provider
* Servlet application

This topic explains how to use Policy Studio to configure each of these relative path resolver types.

## Configure a relative path

To configure a relative path for a specific HTTP Service Group (for example, **Default Services**), perform the following steps:

1. In the Policy Studio tree, select **Environment Configuration** > **Listeners** > **API Gateway** > **Default Services** > **Paths**.
2. Right-click **Paths**, and select **Add** > **Relative Path**. You can also click **Add** on the right.
3. In the **Configure Relative Path** dialog, specify whether to enable listening on the specified path using the **Enable this path resolver** setting, which is set by default.

Alternatively, when editing a policy, you can click **Add Relative Path** at the bottom of the policy canvas beside the **Context** drop-down list. The next sections explain how to configure the settings on the **Configure Relative Path** dialog.

### Policies settings

Use the **Policies** tab to specify the relative path and the policies that are called. The API Gateway invokes the selected policies when it receives a request on the specified path. You can specify a single policy or a chain of policies. Policies are called in the order displayed on this tab. Complete the following fields:

**When a request arrives that matches the path**: Enter a relative path (for example, `/test/path`) for the selected HTTP Services Group. Requests received on this relative path are processed by the policies selected on this tab.

**Global Request Policy**: If a global request policy is configured, when you select this setting, the global request policy is called first in the policy chain.

**Path Specific Policy**: To configure a path-specific policy, select this setting, and browse to select a policy from the dialog. You can search for a specific policy by entering its name in the text box, and the policy tree is filtered automatically. The **Path Specific Policy** field is auto-populated with the currently selected policy when the dialog is launched using the **Add Relative Path** button at the bottom of the policy canvas.

**Global Response Policy**: If a global response policy is configured, when you select this setting, the global response policy is called last in the policy chain.

When you select multiple policies to form a policy chain, the behavior is the same as for a policy shortcut chain filter. Policies are only evaluated when selected, and when the policy can be reached. If any reachable policy fails, the chain fails, and no more policies are evaluated.

### Logging settings

The **Logging Settings** tab enables you to configure the logging level for all filters executed on the relative path, and to configure when message payloads are logged.

#### Transaction Audit Logging Level

You can configure the following settings on all filters executed on the specified relative path:

| Logging Level | Description                                                                                    |
| ------------- | ---------------------------------------------------------------------------------------------- |
| **Fatal**     | Logs Fatal log points that occur on all filters executed.                                      |
| **Failure**   | Logs Failure log points that occur on all filters executed. This is the default logging level. |
| **Success**   | Logs Success log points that occur on all filters executed.                                    |

#### Transaction Audit Payload Logging

You can configure the following settings on the specified relative path:

| Payload Logging                            | Description                                                                                                                        |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| **On receive request from client**         | Log the message payload when a request arrives from the client.                                                                    |
| **On send response to client**             | Log the message payload before the response is sent back to the client.                                                            |
| **On send request to remote server**       | Log the message payload before the request is sent using any **Connection** or **Connect to URL** filters deployed in policies.    |
| **On receive response from remote server** | Log the message payload when the response is received using any **Connection** or **Connect to URL** filters deployed in policies. |

#### Transaction Access Logging

Select the **Include in transaction access log** setting to add this relative path to the API Gateway Access Log. This enables the Access Log at the service level. This setting is not selected by default.

You must also enable the Transaction Access Log at the API Gateway level. In the Policy Studio tree, select **Environment Configuration** > **Server Settings** > **Logging** > **Access Log**, and ensure that **Writing to Transaction Access Log** is enabled.

#### Traffic Monitor Logging

The **Include in traffic monitor log** setting is selected by default. It enables traffic monitoring for Relative Path, Static Content Provider, Static File Provider, and Websocket. This setting is disabled in nested paths because it inherits its parent configuration.

{{< alert title="Note" color="primary" >}}If traffic monitoring is disabled at API Gateway level this supersedes the Logging level setting, which will not have any effect even if it is enabled.{{< /alert >}}

### HTTP method settings

The **HTTP Method** tab enables you to configure an accepted HTTP Method (for example, `POST`). The default is `*`, which means that all HTTP Methods are accepted. You can override the default behavior, and select an appropriate HTTP method for the relative path from the list.

You can also configure multiple HTTP methods on paths of the same name. This enables you to call different policies for different HTTP methods, as shown in the following example:

![HTTP Methods on Same Path](/Images/docbook/images/general/http_methods_same_path.gif)

In this example, the `/test` path is configured three times, each using a different HTTP Method as follows:

* If a `GET`   request is sent to the API Gateway on the `/test`   path, the `Test1`   policy is executed.
* If a `POST`   request is sent, the `Test2`   policy is executed.
* If any other type of request is sent (for example, `DELETE`, `PUT`, and so on), the `Test3`   policy is executed.

For details on the `Path/Test1` subpath in this example, see [Nested relative paths](#nested-relative-paths).

### Advanced settings

On the **Advanced** tab, select whether to resolve this relative path using an **Exact path match**. The default is to use a longest path match (explained in the next section). This setting enables you to further restrict the match to an exact path match (for example, `test1/helloService`). This setting is not selected by default.

### CORS settings

On the **CORS** tab, you can configure settings for Cross Origin Resource Sharing (CORS). By default, the CORS profile set at the HTTP service level is used for all child resolvers of the HTTP service. However, you can override this at the relative path level as follows:

1. In the **CORS Usage** field, select **Override CORS using the following profile**. By default, no CORS profile is selected, and the parent settings are used.

   Relative paths can act as HTTP Services, and can accommodate child resolvers. This means that when a Relative Path has children, and has a CORS profile configured, by default, the children use the parent profile (unless a child overrides it).
2. In the **CORS Profile** field, click the button on the right to select a preconfigured CORS Profile.
3. If no CORS Profiles have already been configured, right-click **CORS Profiles**, and select **Add a CORS Profile**. You can also right-click an existing profile, and select **Edit** to update its settings.

## Nested relative paths

When you have a path `/` that has a child subpath (in this case `/test1`), the following occurs when a request arrives at the API Gateway:

1. Incoming request with path `/test1` is received
2. Request is resolved against `/` (using the longest path match algorithm)
3. API Gateway checks if there are any children (in this case `/test1`)
4. API Gateway checks if any children can process the request (`/test1` is a match)
5. Path resolution is successful

When the request is being processed, the first policy associated with a matching path is executed. In the above example, both `/` and `/test1` make up the match. This means that the policy associated with `/` is executed. If that policy uses the **Call Internal Service** filter, which enables child resolvers to be invoked, then the policy associated with `/test1` will be executed.

Nested paths are generally used as a protection mechanism. For example, a system might be configured with `/` as the only root path, with a number of children. `/` could have a Role-Based Access Control (RBAC) policy associated with it protecting all children. If the RBAC policy succeeds, access is granted, and the child policy is executed. If it fails, access is denied. This mechanism is implemented in the Admin Node Manager. You can view this in Policy Studio by opening the Admin Node Manager configuration.

{{< alert title="Note" color="primary" >}} The difference between `/` having a child `/test1`, and just having a root relative path of `/test1` is due to path resolution and policy execution. To protect `/test1` using RBAC, or run a prerequisite policy prior to running the policy associated with `/test1`, you should use subpaths.

You can also achieve this using a **Policy Shortcut** filter in the policy associated with `/test1` (as a root path). This may be sufficient for a small number of root policies. But when a large number of policies need protection, subpaths are a more elegant solution. Children no longer need a **Policy Shortcut** filter with the policy associated with the parent path as protector.
{{< /alert >}}

### Add a nested relative path

To add a child node to a relative path, right-click the appropriate relative path in the **Resolvers** window, and select the node type (for example, **Add** > **Relative Path**). You can add child nodes of the following type:

* Relative path
* Static content provider
* Servlet application

In the following example, the main relative path is `/`, which calls the **Protect** policy. Content is served by the underlying Servlet Application and Static Content resolvers only when the **Protect** policy succeeds:

![Nested Relative Path](/Images/docbook/images/general/relative_path_children.png)

The parent policy (in this case, **Protect**) must use the **Call Internal Service** filter. This acts as a loopback and enables child resolvers to be invoked. When this prerequisite is met, you can add nested relative paths as required.

### How to access message attributes from parent resolvers

Because invoking the **Call Internal Service** policy results in a new transaction being created (in a loopback connection), a new message whiteboard is generated. This means that you cannot access the message attributes from the parent resolver policy directly. However, in the API Gateway trace, the `dwe.protocol.loopback.message` message attribute is traced in the child policy. For example:

```
dwe.protocol.loopback.message {
  Value:com.vordel.dwe.http.HTTPMessage@df4117
  Type:com.vordel.dwe.http.HTTPMessage
}
```

This is the message object from the parent policy. For example, given a parent resolver policy that generates an `attr1` message attribute, you can use the following selector in the child resolver policy to access `attr1`:

```
${dwe.protocol.loopback.message.attr1}
```

### Order of resolution

The order of resolution for nested relative paths is to first resolve at the parent level. If resolution is successful, and there are children present, then attempt to resolve at the child level. There is no precedence between resolver node types (relative path, static content provider, and servlet application).

Path resolution is performed using the longest path match algorithm by default, regardless of whether nested subpaths are used.

### Example nested path resolution

Using the example relative path shown in [Add a nested relative path](#add-a-nested-relative-path), consider the following inbound requests to the Node Manager:

```
https://testpc:8090/api/deployment/domain/deployments
https://testpc:8090/common/themes/blue/images/server_icon.png
```

The `/api/deployment/domain/deployments` request is resolved as follows:

* Matches the root relative path `/`   as a longest path match
* Matches Servlet Application `/`   as a longest path match (1 char)
* Matches Servlet `/api`   as a longest path match (4 char)
* Matches Static Content `/`   as a longest path match (1 char)
* Does not match Servlet Application `/configuration`
* Does not match Static Content `/docs`
* Does not match Static Content `/kps`

In this example, there are two matches, the `api` Servlet under the Servlet Application on `/`, and the Static Content on `/`. Because the API Gateway uses the longest path match, the `api` Servlet wins, and the request is routed to that resolver. There is no precedence between resolvers, all resolvers are queried for a match.

The `/common/themes/blue/images/server_icon.png` request is resolved as follows:

* Matches the root relative path `/`   as a longest path match
* Matches servlet application `/`   as a longest path match (1 char)
* Does not match servlet `/api`
* Matches static content `/`   as a longest path match (1 char)
* Does not match servlet application `/configuration`
* Does not match static content `/docs`
* Does not match static content `/kps`

In this example, there is only one match, the Static Content on `/`. In this case, the Servlet Application on `/` is not considered because none of its children can resolve the request path.

{{< alert title="Note" color="primary" >}}If there are two resolver matches, and each matches on the same path length, the last resolver visited during path resolution is used, in the order in which the resolver was read and loaded from the configuration.{{< /alert >}}

## Static content providers

A *Static Content Provider* can be used with an HTTP Interface to serve static content from a specified directory. A relative path is associated with each Static Content Provider so that requests received on this path are dispatched directly to the provider and are not mapped to a policy in the usual way. For example, you can configure a Static Content Provider to serve content from the `/opt/mydirectory` folder when it receives requests on the relative path `/mydirectory`.

### Add a static content provider

 To add a Static Content Provider to an HTTP Services group (for example, **Default Services**):

1. Select **Environment Configuration** > **Listeners** > **API Gateway** > **Default Services** > **Paths**.
2. Right-click **Paths**, and select **Add** > **Static Content Provider**.
3. Complete the following fields on the **General** tab.

**Relative Path**: Enter the path that you want to receive requests for static content on.

**Content Directory**: Enter or browse to the location of the directory that you want to serve static content from.

**Index File**: Enter the name of the file to use as the index file for content retrieved from the directory specified in the field above. This file is retrieved by default if no resource is explicitly specified in the request URI. For example, if the client requests `http://[HOST]:8080/docs` (with only a relative path specified instead of a specific resource), the file specified here is retrieved. This file must exist in the directory specified in the previous field.

**Allow Directory Listings**: If this is selected, the Static Content Provider returns full directory listings for requests specifying a relative path only. For example, if selected, and if a request is received for `http://[HOST]:8080/docs/samples`, the list of directories under this directory is returned, assuming that this directory exists on the file system. It is recommended to always turn this option off to avoid leaking details about your server's configuration as described in [CWE-548](https://cwe.mitre.org/data/definitions/548.html). This option is disabled by default.

**HTTP Method**: The **HTTP Method** tab enables you to configure an accepted HTTP Method (for example, `POST`). The default is `*`, which accepts all HTTP methods. You can override the default behavior, and select an appropriate HTTP method for this resolver from the list.

## Static file providers

A *Static File Provider* can be used with an HTTP Interface to serve a static file from a specified directory. A relative path is associated with each Static File Provider so that requests received on this path are dispatched directly to the file provider and are not mapped to a policy in the usual way. For example, you can configure a Static File Provider to serve `/my_brand/favicon.ico` when it receives requests on the `/favicon.ico` relative path.

### Add a static file provider

 To add a Static File Provider to an HTTP Services group (for example, **Default Services**):

1. Select **Environment Configuration** > **Listeners** > **API Gateway** > **Default Services** > **Paths**.
2. Right-click **Paths**, and select **Add** > **Static File Provider**.
3. Complete the following fields on the **General** tab.

**Relative Path**: Enter the path that you want to receive requests for the static file on (for example, `/favicon.ico`).

**File**: Enter or browse to the location of static file you want to serve (for example, `$VDISTDIR/webapps/emc/favicon.ico`, where `$VDISTDIR` specifies the directory where the API Gateway is installed).

**HTTP Method**: This tab enables you to configure an accepted HTTP method for the static file. The default is `GET`, which means that only HTTP GET calls are accepted. You can override the default, and select a different HTTP method for this resolver from the list.

## Servlet applications

Developers can write their own Java servlets and deploy them under the API Gateway to serve HTTP traffic. Conversely, they can remove some of the default servlets from the out-of-the-box configuration (for example, to remove the ability to view logging remotely). This pairing down of unwanted functionality may be required to further lock down the machine on which the API Gateway is running.

{{< alert title="Caution" color="warning" >}}Adding and removing Servlet Applications should be performed only by developers with very specific requirements and under strict guidance from Axway Support. These instructions simply outline how to configure the fields on the dialogs used to set up Servlet Applications. For more detailed instructions, contact Axway Support. {{< /alert >}}

When editing Admin Node Manager or API Gateway Analytics configuration, there are some default Servlet Applications available under the **Management Services** group.

{{< alert title="Caution" color="warning" >}}Deleting any default Servlet Applications may prevent the API Gateway from functioning correctly. You should only delete default Servlet Applications under strict supervision of Axway Support. {{< /alert >}}

### Add a servlet application

To add a Servlet Application to an HTTP Services Group (for example, **Default Services**), perform the following steps:

1. Select **Environment Configuration** > **Listeners** > **API Gateway** > **Default Services** > **Paths**.
2. Right-click **Paths**, and select **Add** > **Servlet Application**.
3. Complete the following fields on the **General** tab.

**Relative Path**: Enter the servlet *context* in this field. You can add multiple servlets under this context, where each servlet is mapped to a unique URI.

**Session Timeout**: Enter the timeout in seconds after which an inactive session is closed. Click **OK**.

**HTTP Method**: This tab enables you to configure an accepted HTTP method (for example, `POST`). The default is `*`, which accepts all HTTP methods. You can override the default, and select an appropriate HTTP method for this resolver from the list.

### Add a servlet

The new Servlet Application now appears in the **Resolvers** window. To add a new servlet, right-click the new Servlet Application, and select **Add Servlet**. Configure the following fields on the **Servlet** dialog:

**URI**: The path entered maps incoming requests on a particular request URI to the Java servlet class entered in the field below. This path must be unique across all Servlets added under this Servlet Application (servlet context).

**Class**: Enter the fully qualified class name of the servlet class. You can add this class to the server runtime by adding the JAR, class file, or package hierarchy to the `[VDISTDIR]/ext/lib` folder. `VDISTDIR` is your API Gateway distribution directory, which is the location where the API Gateway is installed.

**Read Timeout**: Specify the time in seconds that the servlet should wait before closing an idle connection.

**Servlet Properties**: You can configure properties for each servlet by clicking the **Add** button, and entering a name and value in the fields provided on the **Properties** dialog.

## Web service resolvers

A web service resolver is used to identify messages destined for a web service, and to map them to the **Service Handler** (**Web Service Filter**) for that web service. When you import a WSDL file in the **Web Service Repository**, a new web service resolver node is created for each imported web service under the **Paths** for the relevant HTTP services group (for example, Default Services). You can edit the web service resolver settings by right-clicking its tree node, and selecting **Edit**.

The following settings are available in the **Web Service Resolver** dialog:

**Enable this Web service resolver**: Specify whether to enable this web service resolver. This is enabled by default.

**Name**: You can edit the name of the web service resolver.

**Web service**: Click the browse button to select a web service to resolve to. Defaults to the web service imported into the Web Services Repository when this resolver was created.

**Policies**: On the **Policies** tab, select the path and the policies to use for the web service. You can specify a single policy or a chain of policies. Policies are called in the order displayed on this tab. The global request policy, the policy automatically generated when the WSDL file is imported, and the global response policy are all selected in a chain by default. Complete the following fields:

* **Matches the paths in the WSDL**: Select this option if you want the resolver to use the paths specified in the WSDL file. This is the default.
* **Matches this path**: Select this option if you want to specify a different path from the WSDL file, and enter the path.
* **Global Request Policy**: If a global request policy is configured, when you select this setting, the global request policy is called first in the policy chain.
* **Path Specific Policy**: To configure a path-specific policy, select this setting, and browse to select a policy from the dialog.
* **Global Response Policy**: If a global response policy is configured, when you select this setting, the global response policy is called last in the policy chain.

Policies are only evaluated when selected, and when the policy can be reached. If any selected policy fails, the chain fails, and no more policies are evaluated.

**Logging Settings**: The **Logging Settings** tab enables you to configure the logging level for all filters executed on the web service, and to configure when message payloads are logged. The default logging level for all filters on the web service is `Failure`. These logging settings are the same as those already described for the relative path.

**HTTP Method**: The **HTTP Method** tab enables you to configure an accepted HTTP Method (for example, `POST`). The default is `*`, which means that all HTTP Methods are accepted. You can override the default behavior, and select an appropriate HTTP method from the list. The **HTTP Method** settings are the same as those already described for the relative path.

### Edit service handler options

You also edit options for the **Service Handler** for the web service. Right-click the Web Service Resolver node, and select **Quick-Edit Policy** to display a dialog that enables you to configure the following options:

* **Validation**: To use a dedicated validation policy for all messages sent to the web service, select this check box, and click the browse button to configure a policy in the dialog. For example, this enables you to delegate to a custom validation policy used by multiple web services.
* **Routing**: To use a dedicated routing policy to send all messages on to the web service, select this check box, and click the browse button to configure a policy in the dialog. For example, this enables you to delegate to a custom routing policy used by multiple web services.
* **WSDL Access Options**: Select whether to make the WSDL for this web service available to clients. The **Allow the API Gateway to publish WSDL to clients** check box is selected by default. The published WSDL represents a virtualized view of the web service. Clients can retrieve the WSDL from the API Gateway, generate SOAP requests, and send them to the API Gateway, which routes them on to the web service.

These options enable you to configure the underlying autogenerated Service Handler (**Web Service Filter**) without navigating to it in the **Policies** tree. These are the most commonly modified **Web Service Filter** options after importing a WSDL file. Changes made in this dialog are visible in the underlying **Web Service Filter**.

## Check URI path syntax in incoming HTTP requests

You can configure whether to enable strict checking of URI syntax for incoming HTTP requests. If this is enabled, requests with invalid URI path syntax are rejected (the default). If this is disabled, the API Gateway finds the best path match for the URI string.

The main use case for disabling this setting is for compatibility with legacy API Gateway behavior. If you already accept and process client URLs that are not strictly correct, you might want to continue. For example, the Microsoft IIS Web server accepts URLs that contain a backslash character (for example, `http://myserver.com\resource`). You might want to put an API Gateway in front of your IIS server, and continue to accept these non-standard URLs.

To configure this setting, perform the following steps:

1. Edit the following file:

   ```
   INSTALL-DIR/apigateway/system/conf/jvm.xml
   ```
2. Configure the following setting to `true` or `false` as appropriate:

   ```
   <VMArg name="-Dcom.vordel.strictUriSyntaxChecking=true"/>
   ```

   The default and recommended setting is `true`.