{
"title": "SSO troubleshooting",
  "linkTitle": "Troubleshooting",
  "date": "2019-09-17",
  "description": "Common problems and solutions that you might encounter when configuring API Manager SSO, and how to enable traces for SSO."
}

## Logging in both as administrator and SSO user

API Manager does not work properly when logging in both as administrator and SSO user.

If an administrator wants to log in to API Manager both as an API administrator and an SSO user, it is recommended that the administrator uses separate browsers to do this. Using separate tabs in the same browser is not enough, because the tabs share the same session.

## Cannot access API Manager after successful login

You cannot access API Manager after a successful login using SSO.

Verify that there are no errors in the SSO agent log file due to a misconfiguration. Additionally, make sure you are accessing API Manager using the same host name or IP address as the one specified in the API Manager IdP metadata (`idp.xml` file) used by the Identity Provider:

* Do not use `localhost` because some browsers cannot create cookies for this host name.
* Do not mix host name and IP address. Because cookies are linked to the string used in the URL for the host, there is no IP address resolution.

## IdP site cannot be reached

You attempt to log in to API Manager using SSO and the following message appears in your browser:

```
This site can’t be reached

<HostName> refused to connect.
ERR_CONNECTION_REFUSED
```

The URL in the browser address bar contains the address of the IdP, for example, `https://keycloak.example.com:8086/idp/profile/SAML2/POST/SSO`.

In this case, the Identity Provider (for example, Shibboleth, KeyCloak, Active Directory Federation Services, and so on) is either not running or not reachable. Contact your system administrator for support. The system administrator should confirm that the service is running and that there are no firewall restrictions preventing access to the service.

## Internal error if API Gateway and IdP clocks out of sync

When attempting to log in to API Manager using SSO, an Internal Error appers if the clock of your API Gateway server and your IdP server are not correctly synchronized.

To confirm the cause of the error, check the trace file:

```
ERROR   15/sept./2016:14:41:02.018 [3942:dd96da570400a4392aa28808] verifier: io.axway.commons.sso.agent.verifiers.ResponseValidityPeriodVerifier@2edb5d28
ERROR   15/sept./2016:14:41:02.019 [3942:dd96da570400a4392aa28808] Internal error: io.axway.commons.sso.agent.ServiceProviderValidationException: Received response not valid. See log for details.
```

Ensure that the time on the API Gateway server is synchronized with the IdP.

## LDAP response timeout during login

When attempting to log in to API Manager using SSO, you see a message similar to:

```
Login Failure: javax.naming.NamingException: LDAP response read timed out, timeout used:3000ms.
```

This is due to a problem between the IdP and the LDAP service. Contact your network administrator.

## Invalid user or password error after successful login

When attempting to log in to API Manager using SSO, you see the following message:

```
Invalid user or password
```

To confirm the cause of the error, check the trace file:

```
ERROR   07/Sep/2016:17:11:32.804 [23b8:343cd0572c036c31ecc3f2f7] SSO - The user's organization could not be located. Check your service-provider.xml file to see if an OutputAttribute of type 'organization' has been setup. Also check the organization has already been setup in API Manager
ERROR   07/Sep/2016:17:11:32.805 [23b8:343cd0572c036c31ecc3f2f7] SSO - Unexpected exception authenticating : javax.ws.rs.WebApplicationException: HTTP 401 Unauthorized
```

In this scenario, you have logged in successfully using SSO, but the organization associated with your login is not set up. Either the organization is not configured correctly in the `service-provider.xml` file or the organization does not exist in API Manager.

The following message appears in the browser page if the organization does not exist in API Manager:

```
{"errors":[{"code":403,"message":"User was logged in using SSO but failed on permission checks"}]}
```

Ensure that the organization is configured correctly in the `service.provider.xml` file and that the organization exists in API Manager.

For example, the `service-provider.xml` file contains the following `FilterMapping` section:

```
<FilterMapping>
   <Filter>(name=RD Admin)</Filter>
   <OutputAttribute name="role">administrator</OutputAttribute>
   <OutputAttribute name="organization">Research</OutputAttribute>
</FilterMapping>
```

In this example the organization name is `Research`.

Log in to API Manager as the `apiadmin` user (using the non-SSO login URL), and select **Client Registry > Organizations**. If the organization called `Research` does not exist, you must add it.

## Service Provider has no valid certificate

This internal server error occurs when the alias defined by the `keyAlias` attribute (found in the `ServiceProvider` element within `service-provider.xml` file) could not be found in the `keystore` file.

```
ERROR 14/Jan/2020:13:48:13.095 [0523:9dc61d5e141fcd2ba54007f1] An error occurred during SSO processing:
io.axway.commons.sso.agent.ServiceProviderException: Service Provider has no valid certificate. Unable to generate SAML authentication request
```

## Shibboleth IdP logout failure

You are using Shibboleth as an IdP and a logout attempt fails with a message similar to the following:

```
2016-08-30 12:40:23,678 - INFO [org.opensaml.saml.common.binding.security.impl.SAMLProtocolMessageXMLSignatureSecurityHandler:134] - Message Handler:  Validation of protocol message signature succeeded, message type: {urn:oasis:names:tc:SAML:2.0:protocol}LogoutRequest
2016-08-30 12:40:23,688 - INFO [net.shibboleth.idp.saml.saml2.profile.impl.ProcessLogoutRequest:315] - Profile Action ProcessLogoutRequest: No active session(s) found matching LogoutRequest
2016-08-30 12:40:23,689 - WARN [org.opensaml.profile.action.impl.LogEvent:76] - An error event occurred while processing the request: SessionNotFound
```

Set the following options for your Shibboleth IDP (as detailed in the sample file `INSTALL_DIR/apigateway/samples/sso/ShibbolethIDP/conf/idp.properties.forlogout`):

```
idp.cookie.secure = true
idp.cookie.path = "/"

idp.errors.detailed = true
idp.errors.signed = true
idp.session.enabled = true
idp.session.StorageService = shibboleth.ClientSessionStorageService
idp.session.trackSPSessions = true
idp.logout.elaboration = true
idp.logout.authenticated = false
idp.storage.htmlLocalStorage = true

# for troubleshooting
idp.loglevel.idp=DEBUG
idp.loglevel.ldap=DEBUG
idp.loglevel.messages=DEBUG
idp.loglevel.opensaml=DEBUG
```

## Logout issues with Active Directory Federation Services

If your SAML IDP is Active Directory Federation Services (AD FS), you may have issues with the SSO logout.

You must add the a claim rule to enable SSO logout:

1. In AD FS management, click **Trust Relationships > Relying Party Trusts > Edit Claim Rules**, and select **Add Rule**.
2. Set the **Claim Rule Template Type** to **Send Claims Using a Custom Rule**.
3. Give the claim rule a name, and add the following rule:

   ```
   c:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, Value = c.Value, ValueType = c.ValueType, Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/format"] = "urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress", Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/spnamequalifier"] = "<your SAML Relying Party Trust>", Properties["http://schemas.xmlsoap.org/ws/2005/05/identity/claimproperties/namequalifier"] = "http://<your Active Directory server host>/adfs/services/trust");
   ```

    Replace `<your SAML Relying Party Trust>` with the name of your SAML Relying Party Trust, and `<your Active Directory server host>` with the value of your Federation Service Identifier. To get the value of your Federation Service Identifier, click **AD FS > Edit Federation Service Properties**.

## Signature validation error

The API Gateway trace contains the text:

```
ERROR Could not validate signature on provided assertion
ERROR Assertion identity provider name: '<Identity Provider Name>' format: 'urn:oasis:names:tc:SAML:2.0:nameid-format:entity' does not match any allowed identity provider.
```

Consider adding the `format` attribute (as described in the trace text) to the `SamlIdentityProvider` tag in your `service-provider.xml` file.

For example:

```
<IdentityProviders>
  <SamlIdentityProvider
    entityId="<Identity Provider Name>"
    format="urn:oasis:names:tc:SAML:2.0:nameid-format:entity"
    …
  </SamlIdentityProvider>
</IdentityProviders>
```

Ensure that you restart API Gateway after saving the file.

## SAML assertion validation fails

There are several possible reasons why the SAML assertion validation fails. To get more detailed information on this error, temporarily enable traces for SSO and restart API Gateway.

If the detailed trace indicates that validation failed because the SAML assertion has expired, there is a conflict in server time between the IdP server and API Gateway, and the SAML assertion is deemed to have expired.

Synchronize the IdP server time with API Gateway. On Linux, check that the NTP service is running.

Alternatively, open the `service-provider.xml` file, in the `SamlIdentityProvider` section, set `verifyAssertionExpiration` to `false`, and save and redeploy `service-provider.xml` to API Gateway.

After you have fixed the issue, disable the traces for SSO, and restart API Gateway.

## Invalid requester in Keycloak page

On IdP's side in **Events**, the following `LOGIN_ERROR` event could be found:

```
invalid signature error
```

The key in `sso.jks` and the public key stored in Keycloak’s SAML keys for the application do not match as a keypair. Check the SAML keys in the IdP client, and import the correct certificate.

## API Manager login page not shown

If the API Manager login page cannot be loaded and a NullPointerException is logged in the instance trace file as follows:

```
ERROR   25/Jun/2020:09:38:10.280 [1e3b:000000000000000000000000] error handling connection: Success. SSL system call failed
DEBUG   25/Jun/2020:09:38:10.283 [52da:7262f45eb20c460188ef1944] SSO - Parameters - Request URI Path : /api/portal/v1.3/currentuser [getCommonLayer #0]
DEBUG   25/Jun/2020:09:38:10.286 [52da:7262f45eb20c460188ef1944] SSO - Error calculating the default fallback SSO Common Object Instance. Ex: :
java.lang.NullPointerException
  at com.vordel.common.apiserver.filter.sso.SSOJerseyFilter.getFallbackCommonLayer(SSOJerseyFilter.java:337)
  at com.vordel.common.apiserver.filter.sso.SSOJerseyFilter.getCommonLayer(SSOJerseyFilter.java:441)
  at com.vordel.common.apiserver.filter.sso.SSOJerseyFilter.filter(SSOJerseyFilter.java:227)
  at org.glassfish.jersey.server.ContainerFilteringStage.apply(ContainerFilteringStage.java:132)
  ...
  at org.glassfish.jersey.servlet.ServletContainer.service(ServletContainer.java:341)
  at com.vordel.apiportal.api.PortalServletContainer.service(PortalServletContainer.java:78)
  at org.glassfish.jersey.servlet.ServletContainer.service(ServletContainer.java:228)

DEBUG   25/Jun/2020:09:38:10.286 [52da:7262f45eb20c460188ef1944] SSO - defaulting to the fallback API Manager SSO Common Object Instance. [getCommonLayer #6]
ERROR   25/Jun/2020:09:38:10.286 [52da:7262f45eb20c460188ef1944] SSO - There are not enough details in the request to determine the COI. Throwing an Invalid SSO request
```

The likely cause of this issue is that the servlet filter (`com.vordel.common.apiserver.filter.SSOBindingFeature`) has been configured for the API Manager Portal listen socket, but the required SSO configuration files (`service-provider.xml` and `service-provider-apiportal.xml`) are missing from the API Gateway instance `conf` folder.  

## Error on signing assertions

After the user enters the credentials on the Keycloak page, the following error is seen:

```
ERROR   14/Feb/2017:15:08:18.850 [22a1:621da358040092586aa13729] Assertion MUST be signed
ERROR   14/Feb/2017:15:08:18.852 [22a1:621da358040092586aa13729] An error occurred during SSO processing: io.axway.commons.sso.agent.ServiceProviderValidationException: Received response not valid. See log for details.
    at io.axway.commons.sso.agent.builders.AuthenticationResponseBuilder.verifyResponse(AuthenticationResponseBuilder.java:94)
```

The **Sign Assertions** setting in Keycloak is switched off. Ensure that the setting is switched on.

## Keycloak fails to authenticate the user

After the user enters the credentials on the Keycloak page, Keycloak fails to authenticate the user with a message similar to the following:

```
ERROR   13/Feb/2017:17:39:23.346 [6831:4befa158040021d4ae09eee1] SSO - The user's organization could not be located. Check your service-provider.xml file to see if an OutputAttribute of type 'organization' has been setup. Also check the organization has already been setup in API Manager
ERROR   13/Feb/2017:17:39:23.349 [6831:4befa158040021d4ae09eee1] SSO - Failed to authenticate user : [G-fb7c839b-a879-4231-bae9-1c05e3ba6f04]. Exception: : javax.ws.rs.WebApplicationException: HTTP 401 Unauthorized
```

The username `G-fb7c839b-a879-4231-bae9-1c05e3ba6f04` does not correspond to a real username, because the SAML response is not parsed correctly. This happens because the mappings are not set up in the IdP. Ensure the mappings are set correctly in Keycloak. The following shows shows an example on the mappings in Keycloak:

![Screenshot showing a sample mapping in Keycloak](/Images/docbook/images/api_mgmt/keycloak_sample_mapping.png)

## Error retrieving metadata

When using the option to [specify the IdP by URL](/docs/apim_administration/apimgr_sso/saml_sso_config/#specify-the-idp-by-url), the API Manager, at startup, tries to download the required IdP-Metadata information from the given Metadata-URL. This might fail if the server-certificate cannot be validated with an error message like:

```
Error retrieving metadata from /auth/realms/master/protocol/saml/descriptor
javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

You must add the complete server-certificate chain of your `SamlIdentityProvider:metadataUrl` into the trustore referenced in your `service-provider.xml` to avoid this error.

## Enable traces for SSO

To enable the traces for SSO, change the log level for the `org.opensaml` logger and for the `io.axway` logger in the `INSTALL_DIR/apigateway/system/conf/log4j2.yaml` file to debug:

```
# Logging for SSO
- AppenderRef:
    - ref: VordelTrace
    - ref: STDOUT
  additivity: "false"
  level: debug
  name: io.axway

  # Logging for OpenSAML library
- AppenderRef:
    - ref: VordelTrace
    - ref: STDOUT
  additivity: "false"
  level: debug
  name: org.opensaml
- level: false

```

You must also activate the traces in the API Gateway configuration in Policy Studio:

1. Navigate to **Environment Configuration > Server Settings** in the Policy Studio tree.
2. Click **General** and select the `DEBUG` level from the **Tracing level** field.
3. Deploy the changes.

Finally, you must restart the API Gateway instance to enable the changes in `log4j2.yaml` to be applied.

After SSO is working, disable the debugging level by changing the trace levels for SSO and OpenSAML back to the default levels, `error` and `info`, respectively, and restart the gateway.
