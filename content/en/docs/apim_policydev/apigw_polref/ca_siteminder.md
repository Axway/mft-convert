{
"title": "CA SiteMinder filters",
"linkTitle": "CA SiteMinder filters",
"weight": 111,
"date": "2019-10-17",
"description": "Integrate with CA SiteMinder."
}

## Prerequisites

Integration with CA SiteMinder requires CA SiteMinder SDK version 12.52-sp02 or later. You must add the required third-party binaries to your API Gateway and Policy Studio installations.

1. Ensure that any SiteMinder binaries you may have previously added to API Gateway have been deleted.
2. Install CA SiteMinder SDK.
3. Copy the following jar files from the java directory of the CA SDK :
    * `cryptoj.jar`
    * `smagentapi.jar`
    * `smjavasdk2.jar`
4. Add the files to the following directory on API Gateway:

    ```
    INSTALL_DIR/apigateway/<platform>/lib
    ```

5. Restart API Gateway.

## CA SiteMinder session validation filter

CA SiteMinder can authenticate end users and authorize them to access protected web resources. When API Gateway has authenticated successfully to SiteMinder on behalf of an end user, SiteMinder can issue a *single sign-on* token (a session cookie) and return it to API Gateway. API Gateway returns the cookie along with the response to the end user. API Gateway can also insert the cookie into a SAML attribute assertion for downstream web services to consume.

The client then includes the session cookie in subsequent requests to API Gateway. API Gateway can then use the **Session Validation**
filter to ensure that the session cookie is still valid and hence that the user is still authenticated. This means that API Gateway does not have to authenticate every request to SiteMinder and unnecessary round-trips to SiteMinder can be avoided.

Configure the following fields:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**Agent Name**:
Click the browse button, and select a previously configured agent to connect to SiteMinder. This name *must* correspond with the name of an agent previously configured in the CA SiteMinder Policy Server. At runtime, API Gateway connects as this agent to a running instance of SiteMinder.

To add an agent, in the node tree, click **Environment Configuration > External Connections**, right-click the **SiteMinder/SOA Security Manager Connections**, and select **Add a SiteMinder Connection**. For more details on how to configure a SiteMinder connection, see
[Configure SiteMinder/SOA Security Manager connections](/docs/apim_policydev/apigw_external_connections/connector_ca_connection/).

**Resource**:
Enter the name of the protected resource for which the end user must be authenticated. You can enter a selector representing a message attribute that is expanded to a value at runtime. For example, by default API Gateway specifies the original path the end user requested as the resource:

```
${http.request.uri}
```

**Action**:
The user must be authenticated for a specific action on the protected resource. By default, API Gateway takes this action from the HTTP verb used in the incoming request. You can use the following selector to get the HTTP verb:

```
${http.request.verb}
```

Alternatively, any user-specified value can be entered.

**Selector Expression to retrieve session**:
Enter the name of the message attribute that contains the session cookie SiteMinder generated. By default, the token is stored in the `siteminder.session`
message attribute.

## CA SiteMinder logout filter

When API Gateway has authenticated successfully to CA SiteMinder on behalf of an end user, SiteMinder can issue a *single sign-on* token (a session cookie) and return it to API Gateway. API Gateway returns the cookie along with the response to the end user. The client includes the session cookie in subsequent requests to API Gateway. API Gateway can then use the **Session Validation** filter to ensure that the session cookie is still valid and hence that the user is still authenticated.

You can use the **Logout** filter to terminate a previously issued session cookie. After the cookie has been terminated, the client is no longer considered authenticated.

{{< alert title="Note" color="primary" >}}You must have already validated the session before calling the **Logout** filter in your policy. For more details, see [CA SiteMinder session validation](#ca-siteminder-session-validation-filter).{{< /alert >}}

## CA SiteMinder certificate authentication filter

CA SiteMinder can authenticate end users and authorize them to access protected web resources. When API Gateway retrieves an X.509 certificate from a message or during an SSL handshake, it can use the **Certificate Authentication** filter to authenticate to SiteMinder on behalf of the user using the certificate. SiteMinder decides whether the user should be authenticated, and API Gateway then enforces this decision.

Configure the following fields:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**Agent Name**:
Click the browse button, and select a previously configured agent to connect to SiteMinder. This name *must* correspond with the name of an agent previously configured in the CA SiteMinder Policy Server. At runtime, API Gateway connects as this agent to a running instance of SiteMinder.

To add an agent, in the node tree, click **Environment Configuration > External Connections**, right-click the **SiteMinder/SOA Security Manager Connections**, and select **Add a SiteMinder Connection**.

**Resource**:
Enter the name of the protected resource for which the end user must be authenticated. You can enter a selector representing a message attribute that is expanded to a value at runtime. For example, by default API Gateway specifies the original path the end user requested as the resource:

```
${http.request.uri}
```

**Action**:
The user must be authenticated for a specific action on the protected resource. By default, API Gateway takes this action from the HTTP verb used in the incoming request. You can use the following selector to get the HTTP verb:

```
${http.request.verb}
```

Alternatively, any user-specified value can be entered.

**Single Sign-On Token**:
By default, when an end user has been authenticated for a given resource, SiteMinder generates a *single sign-on token* (a session cookie). API Gateway stores this cookie in a user-specified message attribute. API Gateway returns the cookie to the end user along with the response. The end user can then pass this cookie with future requests to API Gateway. When API Gateway receives such a request, it can validate the session cookie using the **Session Validation** filter. The end user stays authenticated for the entire lifetime of the session cookie. As long as the session cookie is valid, API Gateway does not need to re-authenticate the end user against SiteMinder for every request. This increases throughput and performance considerably.

Typically, API Gateway copies the cookie to the `attribute.lookup.list` message attribute using the **Copy / Modify Attributes**
filter, before inserting the cookie into a SAML attribute statement using the **Insert SAML Attribute Assertion** filter. The attribute statement is then returned to the end user to be used use in subsequent requests.

**Put Token in Message Attribute**:
Enter the name of the message attribute where you wish to store the session cookie. By default, the cookie is stored in the `siteminder.session` attribute.

For more details how to integrate API Gateway with SiteMinder, see the
[API Gateway Authentication and Authorization Integration Guide](/docs/apigtw_auth_auth/).

## CA SiteMinder authorization filter

CA SiteMinder can authenticate end users and authorize them to access protected web resources. API Gateway can use the **Authorization** filter to interact directly with SiteMinder, and ask SiteMinder to make authorization decisions on behalf of end users that have successfully authenticated to API Gateway. API Gateway then enforces the decisions made by SiteMinder.

{{< alert title="Note" color="primary" >}}You must configure authentication to CA Siteminder before you configure the **Authorization** filter. For example, you could use a **HTTP Basic authentication** filter configured with the CA Siteminder authentication repository, or a CA SiteMinder **Certificate Authentication** filter. {{< /alert >}}

Configure the following fields on the **Authorization** filter:

**Name**:
Enter an appropriate name for the filter to display in a policy.

**Attributes**:
After the end user is successfully authorized, the attributes listed here are returned to API Gateway and stored in the `attribute.lookup.list` message attribute. API Gateway can use these attributes in subsequent filters in a policy to make decisions based on their values. Alternatively, API Gateway can insert the attributes into a SAML attribute assertion so that the target service can apply some business logic based on their values (for example, if role is CEO, escalate the request, and so on).

To specify an attribute to fetch from SiteMinder, select the **Retrieve attributes from CA SiteMinder**option, and click the **Add** button. If you select the **Retrieve attributes from CA SiteMinder** option, and do not specify attribute names to be retrieved, all attributes returned by SiteMinder are added to the `attribute.lookup.list` message attribute.
