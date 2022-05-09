{
    "title": "Get started with Policy Studio",
    "linkTitle": "Get started with Policy Studio",
    "weight": 1,
    "date": "2019-10-17",
    "description": "Explains some of the main components and concepts used in API Gateway policy development, and shows examples of how they are displayed in Policy Studio."
}

## Before you start

Before you start developing policies in Policy Studio:

* Install API Gateway and Policy Studio
* Create a managed domain
* Start API Gateway and Policy Studio

For details, see [Install API Gateway](/docs/apim_installation/apigtw_install/).

## Policy Studio projects

A Policy Studio project is a design-time store of API Gateway configuration on a local file system, which can be deployed to a running API Gateway instance on a server. Policy Studio projects enable you to use API Gateway as a design-time repository for policies as well as a runtime engine for executing those policies.

For example, policy developers can create specific API Gateway configuration projects on their file system, edit the API Gateway policies and other configuration items, and save their changes back to the file system. They can also then deploy their local Policy Studio project configuration to a running API Gateway instance to test those changes.

## API Gateway instances and groups

You can use Policy Studio to configure and deploy API Gateway instances and groups. An API Gateway instance is an API Gateway capable of running on a host. You can configure various features at the instance level as listeners (for example, HTTP(S) interfaces, file transfer services, JMS services, remote hosts, and so on).

An API Gateway group is a collection of one or more API Gateway instances that share the same configuration.

## Filters

A filter is an executable rule that performs a specific type of processing on a message. For example, the **Message Size**
filter rejects messages that are greater or less than a specified size. There are many categories of message filters available with the API Gateway, including authentication, authorization, content filtering, signing, and conversion. In the Policy Studio, a filter is displayed as a block of business logic that forms part of an execution flow known as a policy. The next section shows some example filters.

## Policies

A policy is a network of message filters in which each filter is a modular unit that processes a message. A message can traverse different paths through the policy, depending on which filters succeed or fail. For example, this enables you to configure policies that route messages that pass a **Schema Validation**
filter to a back-end system, and route messages that pass a different **Schema Validation**
filter to a different system. A policy can also contain other policies, which enables you to build modular reusable policies.

In the Policy Studio, the policy is displayed as a path through a set of filters. Each filter can have only one **Success Path**
and one **Failure Path**. You can use these success and failure paths to create sophisticated rules. For example, if the incoming data matches schema A, scan for attachments and route to service A, otherwise route to service B. You can configure the colors used to display success paths and failure paths in the Policy Studio **Preferences**
menu. You can also specify to **Show Link Labels** (**S** or **F**).

The following shows an example policy that performs XML signature verification:

![Example signature verification policy](/Images/docbook/images/getting_started/gs_example_policy.png)

A policy must have a **Start** filter (in this case, **Verify the Request XML Signature**). Filters labeled **End**
stop the execution of the policy (for example, the filter execution fails). A filter labeled **Start/End**
indicates that the policy execution starts there, and that the policy stops executing if this filter fails. A policy with a single filter labeled **Start/End**
is also valid.

## Message attributes

Each filter requires input data and produces output data. This data is stored in message attributes, and you can access their values in API Gateway configuration using a selector syntax (for example,`${attribute.name}`).

You can also use specific filters to create your own message attributes, and to set their values. The full list of message attributes flowing through a policy is displayed when you right-click the Policy Studio canvas, and select **Show All Attributes**. You can also hover your mouse over a filter to see its inputs and outputs. The **Trace**
filter enables you to trace message attribute values at runtime.

The message attribute *white board*
refers to the list of attributes that are available to a particular filter at runtime of the API Gateway during the processing of requests and responses. The following example shows the attributes displayed when hovering over a **Connect to URL**
filter at design time in Policy Studio:

![Example message attributes](/Images/docbook/images/getting_started/gs_example_policy_attributes.png)

If a filter requires an attribute as input that has not been generated in the previous execution steps, the filter is displayed in a different color in the Policy Studio (default is red). You can configure the color used to display missing attributes in the Policy Studio **Preferences**
menu. Alternatively, you can also view all required attributes by right-clicking the canvas, and selecting **Show All Attributes**.

A missing attribute might indicate a problem that you need to investigate (for example, in the chaining of filters or policies, or that the policy cannot run on its own). This is often the case for reusable filters, such as those displayed in the previous example.

At the policy level, you also can click the horizontal bar at the top of the Policy Studio canvas to show the list of all attributes required as input to the entire policy. If any attributes are generated by the policy, you can click a corresponding bar at the bottom to see a list of generated attributes. The following example shows a required attribute:

![Required message attribute](/Images/docbook/images/getting_started/gs_example_policy_attributes_required.png)

## Selectors

A selector is a special syntax that enables API Gateway configuration settings to be evaluated and expanded at runtime based on metadata (for example, in message attributes, a Key Property Store (KPS), or environment variables). For example, the following selector returns the value of a message attribute:

```
${http.request.clientaddr}
```

Selectors are powerful a feature when extending the API Gateway to integrate with other systems. For more details on selectors, see [Select configuration values at runtime](/docs/apim_policydev/apigw_poldev/general_selector/).

## Faults and errors

When a transaction fails, you can use a fault to return error information to the client application. The API Gateway provides the **SOAP Fault**, **JSON Error**, and **Generic Error**
filters. By default, the API Gateway returns a very basic fault to the client when a message filter fails. You can add a specific fault filter to a policy to return more detailed error information to the client.

For example, the following screen shows an authentication policy that includes a **SOAP Fault**:

![Example SOAP fault in authentication policy](/Images/docbook/images/getting_started/authentication_fault.png)

## Policy shortcuts

A policy shortcut enables you to create a link from one policy to another policy. For example, you could create a policy that inserts security tokens into a message, and another that adds HTTP headers. You can then create a third policy that calls the other two policies using **Policy Shortcut**
filters.

A policy shortcut chain enables you to run a series of policies in sequence without needing to create a policy containing policy shortcuts. In this way, you can create modular policies to perform certain specific tasks, such as authentication, content filtering, returning faults, or logging, and then link these policies together in a sequence using a policy shortcut chain.

## Alerts

The API Gateway can send alert messages for specified events to various alerting destinations. System alerts are usually sent when a filter fails, but they can also be used for notification purposes. For example, the API Gateway can send system alerts to the following destinations:

* Email Recipient
* Check Point Firewall-1 (OPSEC)
* Local Sylsog
* Remote Sylsog
* SNMP Network Management System
* Twitter
* Windows Event Log

You can configure alert destinations, and then add an **Alert**
filter to a policy, specifying the appropriate alert destination.

## Policy containers

A policy container is used to group similar policies together (for example, all authentication or logging policies), or policies that relate to a particular service. A number of useful policies are provided in the **Policy Library**
container (for example, policies that return faults, and policies that block threatening content). You can add your own policies to this container, and add your own policy containers to suit your requirements.

## Policy contexts

Policies can execute in a specified context. For example, you can set a context by associating a relative execution path or listener with a policy. When a policy is called from another policy, the context is set to the calling policy name (for example, **Authenticate**).

In Policy Studio, you can select a context from the **Context**
drop-down list at the bottom of the policy canvas. The Policy Studio then displays whether the attributes required for execution are available in that context. The **Context**
list includes all connected relative paths, listeners, web services, SMTP services, and policy shortcuts that use the selected policy. Click **View navigator node**
to display the selected context.

## Listeners

You can define different types of listeners and associate them with specific policies. For example, listeners include the following types:

* HTTP/HTTPS
* Directory Scanner
* POP mail server connection
* JMS connection
* TIBCO Rendezvous connection

The API Gateway can be used to provide protocol mediation (for example, receiving a SOAP request over JMS, and transforming it into a SOAP/HTTP request to a back-end service). For HTTP/HTTPS listeners, policies are linked to a relative path. Otherwise, policies are linked to the listener itself. You can associate a single policy with multiple listeners.

## Remote hosts

You can define a remote host when you need more control of the connection settings to a particular server. The available connection settings include the following:

* HTTP version
* IP addresses
* Timeouts
* Buffers
* Caches

For example, by default, the API Gateway uses HTTP 1.0. You can force it to use HTTP 1.1 using remote host settings. You must also define a remote host to track real-time metrics for a particular host.

## Servlet applications

The API Gateway provides a web server and servlet application server that can be used to host static content (for example, documentation), or servlets providing internal services. Static content or servlets can be accessed from a policy using the **Call Internal Service**
filter. This feature is not meant to replace an enterprise J2EE server, but rather to enable you to write functionality using technology such as servlets.

## Service virtualization

When you register an API service or web service, and deploy it to the API Gateway, the API Gateway virtualizes the service. Instead of connecting to the service directly, clients connect through the API Gateway. The API Gateway can then apply policies to messages sent to the destination service (for example, to enable security, monitoring, and acceleration).

## API Gateway settings

You can configure the underlying configuration settings for API Gateway using the **Environment Configuration > Server Settings** node in the Policy Studio tree. This includes the following settings:

* API Manager
* General
* Logging
* Messaging
* Monitoring
* Security

## Certificates and keys

API Gateway must be able to trust X.509 certificates to establish SSL connections with external servers, validate XML Signatures, encrypt XML segments for certain recipients, and for other such cryptographic operations. Similarly, a private key is required to carry out certain other cryptographic operations, such as message signing and decrypting data.

The **Certificate Store** contains all the certificates and keys that are considered to be trusted by the API Gateway. Certificates can be imported into or created by the certificate store. You can also assign a private key to the public key stored in a certificate, by importing the private key, or by generating one using the provided interface.

For more information on importing and creating certificates and keys, see [Manage certificates and keys](/docs/apim_administration/apigtw_admin/general_certificates/).

## API Gateway user store

Users are mainly used for authentication purposes in API Gateway. In this context, the **User Store** acts as a repository for user information against which users can be authenticated. You can also store user attributes for each user or user group. For example, you can then use these attributes when generating SAML attribute assertions on behalf of the user.

[Manage API Gateway users](/docs/apim_administration/apigtw_admin/manage_user_access/) contains more details on how to create users, user groups, and attributes.

## Black list and White list

The **White list** is a global library of regular expressions that can be used across several different filters. For example, the **Validate HTTP Headers**, **Validate Query String**, and **Validate Message Attributes** filters all use regular expressions from the **White list** to ensure that various parts of the request contain expected content.

The **White list** is prepopulated with regular expressions that can be used to identify common data formats, such as alphanumeric characters, dates, email addresses, IP addresses, and so on. For example, if a particular HTTP header is expected to contain an email address, the **Email Address** expression from the library can be run against the HTTP header to ensure that it contains an email address as expected. This is yet another way that the API Gateway can ensure that only the correct data reaches the web service.

While the **White list** contains regular expressions to identify valid data, the **Black list** contains regular expressions that are used to identify common attack signatures. For example, this includes expressions to scan for SQL injection attacks, buffer overflow attacks, ASCII control characters, DTD entity expansion attacks, and many more.

You can run various parts of the request message against the regular expressions contained in the **Black list** library. For example, the HTTP headers, request query string, and message (MIME) parts can be scanned for SQL injection attacks by selecting the SQL-type expressions from the **Black list** .The **Threatening Content** filter also uses regular expressions from the **Black list**
to identify attack signatures in request messages.

## Scripts

The **Scripts** library contains the JavaScript and Groovy scripts that API Gateway can use to interact with the message as it is processed. For example, you use these scripts with the **Scripting Filter** to get, set, and evaluate specific message attributes.

In the Policy Studio navigation tree, you can access the global scripts library by selecting **Resources > Scripts**. Select a child node to view or edit its contents. To add a script, right-click the **Scripts** node, and select **Add Script**.

For more details on using the **Scripts Library** dialog to add scripts, and on configuring API Gateway to use scripts, see the **Scripting language** filter.

## Stylesheets

The **Stylesheets**
library contains the XSLT style sheets that API Gateway can use to transform incoming request messages. The **XSLT Transformation**
filter enables you convert the contents of a message using these style sheets. For example, an incoming XML message that adheres to a specific XML schema can be converted to an XML message that adheres to a different schema before it is sent to the destination web service.

In the Policy Studio navigation tree, you can access the global style sheet library by selecting **Resources > Stylesheets**. Select a child node to view or edit its contents. To add a style sheet, right-click the **Stylesheets** node, and select **Add Stylesheet**.

## References

References can occur between API Gateway configurations items (for example, a policy might include a reference to an external connection to a database). You can view references between configuration items in Policy Studio by right-clicking an item, and selecting **Show All References**. References are displayed in a tab at the bottom of the window.

The **Show All References** option is enabled only for items that have references to other items. For an example in a default API Gateway installation, right-click **Environment Configuration > External Connections > LDAP Connections > Sample Active Directory Connection**, and select **Show all References**. Showing all references is useful for impact analysis (for example, before upgrading or migrating), and is a general navigation aid.
