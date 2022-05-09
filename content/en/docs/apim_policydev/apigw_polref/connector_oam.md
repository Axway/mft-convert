{
"title": "Oracle Access Manager filters",
  "linkTitle": "Oracle Access Manager filters",
  "weight": 115,
  "date": "2019-10-17",
  "description": "Integrate with Oracle Access Manager."
}

## Oracle Access Manager certificate authentication filter

The **Log in with certificate** filter enables authentication to Oracle Access Manager (OAM) using an X.509 certificate presented by the client. After successful authentication, OAM issues a Single Sign On (SSO) token, which can then be used by the client for subsequent calls to the virtualized service.

Configure the following general settings:

**Name**: Enter an appropriate name for this filter to display in a policy.

**Attribute containing X509 certificate**: Enter the name of the message attribute that contains the user's X.509 certificate. By default, this is stored in the `certificate` message attribute.

**Attribute to contain SSO token id**: Enter the name of the message attribute to contain the user's SSO token. By default, the SSO token is stored in the `oracle.sso.token` message attribute.

### Resource settings

Configure the following resource settings:

**Resource Type**: Enter the type of the resource for which you are requesting access. For example, when seeking access to a web-based URL, enter `http`.

**Resource Name**: Enter the name of the resource for which the user is requesting access. By default, this field is set to `//hostname${http.request.uri}`, which contains the original path requested by the client.

**Operation**: In most access management products, it is common to authorize users fora limited set of actions on the requested resource. For example, users with management roles may be able to write (HTTP POST) to a certain web service, but users with more junior roles might only have read access (HTTP GET) to the same service.

You can use this field to specify the operation to grant the user access to on the specified resource. By default, this field is set to the `http.request.verb` message attribute, which contains the HTTP verb used by the client to send the message to the API Gateway (for example, POST).

**Include query string**: Select whether the query string parameters are used by the OAM server to determine the policy that protects this resource. This setting is optional if the policies configured do not rely on the query string parameters.

### Session settings

Configure the following session settings:

**Location**: If the client location must be passed to OAM for it to make its decision, you can enter a valid DNS name or IP address to specify this location.

**Parameters**: You can add optional additional parameters to be used in the authentication decision. The available optional parameters include the following:

* `ip`: IP address, in dotted decimal notation, of the client accessing the resource.
* `operation`: Operation attempted on the resource (for HTTP resources, one of `GET`, `POST`, `PUT`, `HEAD`, `DELETE`, `TRACE`, `OPTIONS`, `CONNECT`, or `OTHER`).
* `resource`: The requested resource identifier (for HTTP resources, the full URL).
* `targethost`: The host (`host:port`) to which resource request is sent.

{{< alert title="Note" color="primary" >}}One or more of these optional parameters might be required by certain authentication schemes, modules, or plug-ins configured in the OAM server. To determine which parameters to add, see your OAM server documentation.{{< /alert >}}

### Certificate authentication OAM Access SDK settings

Configure the following fields for the OAM Access SDK:

**OAM ASDK Directory**: Enter the path to your OAM Access SDK directory. For more details on the OAM Access SDK, see your Oracle Access Manager documentation.

**OAM ASDK Compatibility Mode**: Select the Oracle Access Manager server version to which this filter connects (`OAM 10g` or `OAM 11g`). Defaults to `OAM 11g`.

## Oracle Access Manager authorization filter

The **Authorization** filter enables you to authorize an authenticated user for a particular resource against Oracle Access Manager (OAM). The user must first have been authenticated to OAM using HTTP basic or digest authentication. After successful authentication, OAM issues a Single Sign On (SSO) token, which can then be used instead of the user name and password.

Configure the following general fields:

**Name**: Enter a descriptive name for this filter to display in a policy.

**Attribute Containing SSO Token**: Enter the name of the message attribute that contains the user's SSO token. This attribute is populated when authenticating to Oracle Access Manager using HTTP basic authentication or HTTP digest authentication. By default, the SSO token is stored in the `oracle.sso.token` message attribute.

### Request settings

Configure the following fields to authorize a user for a particular resource against Oracle Access Manager:

**Resource Type**: Enter the resource type for which you are requesting access (for example, `http` for access to a web-based URL).

**Resource Name**: Enter the name of the resource for which the user is requesting access. The default is `//hostname${http.request.uri}`, which contains the original path requested by the client.

**Operation**: In most access management products, it is common to authorize users fora limited set of actions on the requested resource. For example, users with management roles may be able to write (HTTP POST) to a certain web service, but users with more junior roles might only have read access (HTTP GET) to the same service.

You can use this field to specify the operation to grant the user access to on the specified resource. By default, this field is set to the `http.request.verb` message attribute, which contains the HTTP verb used by the client to send the message to the API Gateway (for example, POST).

**Include Query String**: Select whether the OAM server uses the HTTP query string parameters to determine the policy that protects this resource. This setting is optional if the configured policies do not rely on the query string parameters. This setting is not configured by default.

### Authorization OAM Access SDK settings

Configure the following fields for the OAM Access SDK:

**OAM ASDK Directory**: Enter the path to your OAM Access SDK directory. For more details on the OAM Access SDK, see your Oracle Access Manager documentation.

**OAM ASDK Compatibility Mode**: Select the Oracle Access Manager server version to which this filter connects (`OAM 10g` or `OAM 11g`). Defaults to `OAM 11g`.

## Oracle Access Manager SSO token validation filter

The **SSO Token Validation** filter enables you to check an Oracle Access Manager Single Sign On (SSO) token to ensure that it is still valid. The SSO token is issued by Oracle Access Manager (OAM) after the API Gateway authenticates to it on behalf of an end user using HTTP basic authentication or HTTP digest authentication. After successfully authenticating to OAM, the SSO token is stored in the `oracle.sso.token` message attribute.

Oracle Access Manager SSO enables a client to send up its user name and password once, and then receive an SSO token (for example, in a cookie or in the XML payload). The client can then send up the SSO token instead of the user name and password.

Configure the following fields to validate an SSO token issued by Oracle Access Manager:

**Name**: Enter a descriptive name for the filter to display in a policy.

**Attribute Containing SSO Token ID**: Enter the name of the message attribute that contains the SSO token to validate. This attribute is populated when authenticating to Oracle Access Manager using *HTTP basic authentication* on page 1 or *HTTP digest authentication* on page 1. By default, the SSO token is stored in the `oracle.sso.token` message attribute.

**OAM ASDK Directory**: Enter the path to your OAM Access SDK directory. For more details on the OAM Access SDK, see your Oracle Access Manager documentation.

**OAM ASDK Compatibility Mode**: Select the Oracle Access Manager server version to which this filter connects (`OAM 10g` or `OAM 11g`). Defaults to `OAM 11g`.

## Oracle Access Manager SSO session logout filter

The **Log out session** filter enables you to log out a session from Oracle Access Manager by invalidating the SSO token that is associated with this session.

Configure the following fields to explicitly log out (invalidate)an SSO token from Oracle Access Manager:

**Name**: Enter a descriptive name for this filter to display in a policy.

**Attribute Containing SSO Token ID**: Enter the name of the message attribute that contains the SSO token to invalidate. This attribute is populated when authenticating to Oracle Access Manager using HTTP basic authentication or HTTP digest authentication. By default, the SSO token is stored in the `oracle.sso.token` message attribute.

**OAM ASDK Directory**: Enter the path to your OAM Access SDK directory. For more details on the OAM Access SDK, see your Oracle Access Manager documentation.

**OAM ASDK Compatibility Mode**: Select the Oracle Access Manager server version to which this filter connects (`OAM 10g` or `OAM 11g`). Defaults to `OAM 11g`.
