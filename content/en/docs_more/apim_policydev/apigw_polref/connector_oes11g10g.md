{
"title": "Oracle Entitlements Server filters",
  "linkTitle": "Oracle Entitlements Server filters",
  "weight": 116,
  "date": "2019-10-17",
  "description": "Integrate with Oracle Entitlements Server 10g or 11g."
}

## Oracle Entitlements Server 11g authorization filter

This filter enables you to authorize an authenticated user for a particular resource against Oracle Entitlements Server (OES) 11g. The user must first have been authenticated to OES 11g (for example, using HTTP basic authentication or HTTP digest authentication).

This filter enables you to configure API Gateway to delegate authorization to OES 11g. You can configure API Gateway to authorize an authenticated user for a particular resource against OES 11g. Credentials used for authentication can be extracted from the HTTP Basic header, WS-Security Username Token, or the message payload. After successful authentication, the API Gateway can authorize the user to access a resource using OES 11g.

Configure the following fields:

**Name**: Enter an appropriate descriptive name for this filter.

**Resource**: Enter the URL for the target resource to be authorized (for example, web service). Alternatively, if this policy is reused for multiple services, enter a URL using selectors, which are expanded at runtime to the value of the specified attributes. For example:

```
${http.destination.protocol}://${http.destination.host}:${http.destination.port}${http.request.uri}
```

**Action**: Enter the HTTP verb (for example, `POST`, `GET`, `DELETE`, and so on). Alternatively, if this policy is reused for multiple services, enter a selector, which is expanded at runtime to the value of the specified attribute (for example, `${http.request.verb}`).

**Environment/Context attributes**: Click **Add** to specify optional Application Contexts as name-value pairs. Enter a **Name** and **Value** in the **Properties** dialog. Repeat to specify multiple properties.

## Oracle Entitlements Server 10g authorization filter

This filter enables you to authorize an authenticated user for a particular resource against Oracle Entitlements Server (OES) 10g. The user must first have been authenticated to OES 10g (for example, using HTTP basic authentication or HTTP digest authentication).

This filter enables you to configure API Gateway to delegate authorization to OES 10g. You can configure API Gateway to authorize an authenticated user for a particular resource against OES 10g. Credentials used for authentication can be extracted from the HTTP Basic header, WS-Security Username Token, or the message payload. After successful authentication, the API Gateway can authorize the user to access a resource using OES 10g.

### Settings

Configure the following fields on the **Settings** tab:

**Resource**: Enter the URL for the target resource (for example, web service). Alternatively, if this policy is reused for multiple services, enter a URL using message attribute selectors, which are expanded at runtime to the value of the specified attribute. For example:

```
${http.destination.protocol}://${http.destination.host}:${http.destination.port}${http.request.uri}
```

**Resource Naming Authority**: Enter `apigatewayResource` to match the Naming Authority Definition loaded in the OES 10g settings. For more details, see
[Oracle Security Service Module settings (10g)](/docs/apim_policydev/apigw_poldev/security_server_settings/#configure-oracle-security-service-module-settings-10g).

**Action**: Enter the HTTP verb (for example, `POST`, `GET`, `DELETE`, and so on). Alternatively, if this policy is reused for multiple services, enter a message attribute selector, which is expanded at runtime to the value of the specified attribute (for example, `${http.request.verb}`).

**Action Naming Authority**: Enter `apigatewayAction` to match the Naming Authority Definition loaded in the OES 10g settings.

**How access request is processed**: Select one of the following options:

* `ONCE`
* Specifies that the authorization query is only asked once for a resource and action.
* `POST`
* Specifies that the authorization query is asked after a resource is acquired, but before it has been processed or presented.
* `PRIOR`
* Specifies that the authorization query is asked before a resource is acquired.

### Application Context

Configure the following field on the **Application Context** tab:

**Application's Current Context**: Click **Add** to specify optional Application Contexts as name-value pairs. Enter a **Name** and **Value** in the **Properties** dialog. Repeat to specify multiple properties.

## Get roles from Oracle Entitlements Server 10g filter

This filter enables you to get the set of roles that are assigned to an identity for a specific resource (for example, web service) and a specific action (for example, HTTP POST) from Oracle Entitlements Server (OES) 10g.

### Get roles settings

Configure the following fields on the **Settings** tab:

**Resource**: Enter the URL of the target resource (for example, web service). Alternatively, if this policy is reused for multiple services, enter a URL using message attribute selectors, which are expanded at runtime to the value of the specified attribute. For example:

```
${http.destination.protocol}://${http.destination.host}:${http.destination.port}${http.request.uri}
```

**Resource Naming Authority**: Enter `apigatewayResource` to match the Naming Authority Definition loaded in the OES 10g settings.

**Action**: Enter the HTTP verb (for example, `POST`, `GET`, `DELETE`, and so on). Alternatively, if this policy is reused for multiple services, enter a message attribute selector, which is expanded at runtime to the value of the specified attribute (for example, `${http.request.verb}`).

**Action Naming Authority**: Enter `apigatewayAction` to match the Naming Authority Definition loaded in the OES 10g settings.

### Get roles application context

Configure the following field on the **Application Context** tab:

**Application's Current Context**: Click **Add** to specify optional Application Contexts as name-value pairs. Enter a **Name** and **Value** in the **Properties** dialog. Repeat to specify multiple properties.
