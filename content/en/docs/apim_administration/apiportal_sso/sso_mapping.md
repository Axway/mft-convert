---
title: Single sign-on reference
linkTitle: Single sign-on reference
weight: 2
date: 2019-07-30
description: Single sign-on mapping syntax and examples, and a reference to
  elements in the `service-provider-apiportal.xml` configuration file.
---
<!-- TODO when APIM SSO is migrated we can replace this with a ref to that topic -->

## Mapping syntax

The IdP sends information about the SSO user to the SP (in the case of API Portal, this is API Manager) using attributes. These attributes contain information about the user, such as the user's name, department, organization, email address, phone number, and so on.

This section describes how to define mappings from an IdP to API Portal. An IdP can name attributes associated with the authenticated user in a variety of different ways (for example, `mail`, `email`, or `e-mail`). API Portal expects attributes with specific names, so you might need to transform the IdP attributes to the API Portal attributes using a rename mapping. In addition, an IdP might not provide some attributes that API Portal requires, so you might need to use a filter mapping to assign required attributes based on a filter.

The mappings are defined in the `Mappings` section of the `SAMLIdentityProvider` section in the `service-provider-apiportal.xml` file. If you have configured SSO for API Manager, note that the mappings for API Portal are independent from the mappings for API Manager.

Two types of mappings are supported:

* Rename mapping – This mapping enables you to rename an attribute from the IdP, keeping its value.
* Filter mapping – This mapping creates output attributes when a filter matches the input attributes from the IdP. A filter is specified using the [LDAP Search Filter](https://docs.oracle.com/cd/E19528-01/819-0997/gdxpo/index.html) syntax. For more details, see [Filter syntax](/docs/apim_administration/apimgr_sso/sso_mapping/#filter-syntax).

Following are the attributes expected by API Portal, and  examples of mappings that you can use to provide them.

`name` (mandatory)

* The logged in user name.
* Sample RenameMapping if the IdP provides an attribute which should be renamed:

```
<RenameMapping source="user" target="name"/>
```

`organization` (mandatory)

* The API Portal organization associated with the logged in user. The organization must already exist in API Portal. Organizations can be added by an API Manager administrator in API Manager.
* If the IdP provides this value, and the IdP attribute has a different name, you can use a `RenameMapping` to transform it to an organization attribute. If the IdP does not provide the value associated with the organization, you can use an `OutputAttribute` to assign an organization to the logged in user. For example:

```
<OutputAttribute name="organization">Research</OutputAttribute>
```

`role` (mandatory)

* The API Portal role associated with the logged in user. Permitted substring values: `Operator`, `User`.
* The IdP provides this value, and the IdP attribute has a different name, you can use a `RenameMapping` to transform it to a role attribute. If the IDP does not provide the value associated with the role, you can use an `OutputAttribute` to assign a role to the logged in user. For example:

```
<OutputAttribute name="role">Operator</OutputAttribute>
```

`mail` (optional)

* The email address associated with the logged in user.

    While `email` is optional, it is strongly recommended that the IDP sends this value and that the mapping is present in the configuration file. This will help to future-proof SSO implementations in case `email` becomes a mandatory field in the future. Including it now will also help to limit outages and provide a more complete user profile.
* Sample RenameMapping if the IdP provides an attribute which should be renamed:

```
<RenameMapping source="email" target="mail"/>
```

`description` (optional)

* The description text associated with the logged in user.
* Sample RenameMapping if the IdP provides an attribute which should be renamed:

```
<RenameMapping source="userDescription" target="description"/>`
```

`department` (optional)

* The department that the logged in user belongs to.
* Sample RenameMapping if the IdP provides an attribute which should be renamed:

```
<RenameMapping source="businessUnit" target="department"/>
```

`telephonenumber` (optional)

* The telephone number associated with the logged in user.
* Sample RenameMapping if the IdP provides an attribute which should be renamed:

```
<RenameMapping source="phone" target="telephonenumber"/>
```

{{< alert title="Note" color="primary" >}}API Portal attribute names are all lowercase, and they are case-sensitive. {{< /alert >}}

### Rename mapping example

If the IdP generates a attribute name that is different to the attribute name expected by API Portal (for example, `e-mail` rather than `mail`), you can use a RenameMapping directive to effectively rename the IdP attribute to the API Portal attribute.

For example, to rename the IdP attribute name `e-mail` to the API Portal attribute `mail`, use the following RenameMapping:

```
<RenameMapping source="e-mail" target="mail"/>
```

The `source` attribute refers to the attribute supplied by the IdP that you want to rename.

The `target` attribute refers to the name of the attribute after it has been renamed.

### Multiple rename mappings example

You can have multiple RenameMapping directives.

In the following example, two rename mappings are used:

* The IdP presents an attribute called `email`. Using the RenameMapping, this is transformed to `mail`.
* The IdP presents an attribute called `phone`. Using the RenameMapping, this is transformed to `telephonenumber`.

In addition, a filter mapping is used to achieve the following:

* If a user logs in with the transformed `mail` attribute set to `sjones@research.activedirectory2012.lab.chicago.acme.int` the user is assigned a `role` of `User` and an `organization` of `Research`.

For example:

```
<Mappings>
    <RenameMapping source="phone" target="telephonenumber"/>
    <RenameMapping source="email" target="mail"/>
    <FilterMapping>
        <Filter>(mail=sjones@research.activedirectory2012.lab.chicago.acme.int)</Filter>
        <OutputAttribute name="role">User</OutputAttribute>
        <OutputAttribute name="organization">Research</OutputAttribute>
    </FilterMapping>
</Mappings>
```

### Filter mapping examples

Add the two required attributes when the `department` attribute from the IdP is set to `RD Admin`:

```
<Mappings>
    <FilterMapping>
        <Filter>(department=RD Admin)</Filter>
        <OutputAttribute name="role">operator</OutputAttribute>
        <OutputAttribute name="organization">RD</OutputAttribute>
    </FilterMapping>
</Mappings>
```

Add the two required attributes when the `mail` attribute from the IdP is set to `john.doe@prov.org`:

```
<Mappings>
    <FilterMapping>
        <Filter>(mail=john.doe@prov.org)</Filter>
        <OutputAttribute name="role">operator</OutputAttribute>
        <OutputAttribute name="organization">prov</OutputAttribute>
    </FilterMapping>
</Mappings>
```

Add the two required attributes when the `department` attribute from the IdP is set to `RD User`:

```
<Mappings>
    <FilterMapping>
        <Filter>(department=RD User)</Filter>
        <OutputAttribute name="role">user</OutputAttribute>
        <OutputAttribute name="organization">prov</OutputAttribute>
    </FilterMapping>
</Mappings>
```

Filter by the user’s email, and assign a role and an organization:

```
<Mappings>
    <RenameMapping source="email" target="mail"/>
    <FilterMapping>
        <Filter>(mail=jsmith@activedirectory2012.prod.acme.org)</Filter>
        <OutputAttribute name="role">operator</OutputAttribute>
        <OutputAttribute name="organization">Production</OutputAttribute>
    </FilterMapping>
</Mappings>
```

## `service-provider-apiportal.xml` configuration file reference

This section describes the elements in the `service-provider-apiportal.xml` configuration file.

### `<SSOConfiguration>`

This is the root element of the configuration descriptor.

### `<CertificateValidation>`

This element describes the certificate validation. You can configure certificate validation to validate the SP and IdP certificates at startup.

The following attributes are supported:

`pathValidation`

* If set to `true`, the certification path for each certificate will be checked. If set to `false`, the agent verifies only the validity period of each certificate.
* A trust store must be specified if this attribute is `true`.

`enableRevocation`

* If set to `true`, the agent also verifies if the certificates are not revoked.

`trustStorePath`

* If set to `true`, the agent also verifies if the certificates are not revoked.

`trustStorePassword`

* The password to access the trust store.

`intermediateStorePath`

* The path to a store containing intermediate certificates that can appear in certificate chains.

`intermediateStorePassword`

* The password to access the intermediate certificates store.

`delayBetweenValidations`

* Defines at which interval certificate validation occurs, in hours.
* To disable certificate validation, set `pathValidation` to false. For example:

```
<CertificateValidation
    pathValidation="false"
    ...
</CertificateValidation>
```

### `<ServiceProvider>`

This element describes the SP.

The following attributes are supported:

`entityId`

* Sets the unique identifier of the SP. This identifier is sent to the IdP so it can know who is requesting an authentication or logging out.
* The default value is `api-portal`. The same value must also be set in the Joomla! Administrator Interface (JAI), see [Enable SSO in API Portal](/docs/apim_administration/apiportal_sso/sso_config/#enable-sso-in-api-portal).

`excludeHostInEndpointURICheck`

* An endpoint URI check examines the intended destination endpoint as determined by the IdP and the actual receiver endpoint for a message. In other words, where the message was intended nd where it was actually delivered.
* By default, the check mandates that these two values must match exactly. However, in the case of reverse proxying, the message destination endpoint server forwards the messages on for rocessing to another server, and the check must be relaxed slightly.
* This setting specifies if that the host name in the message destination endpoint is not checked although the path is checked. Set this value to true.

`relaxedEndpointURICheckHostDetails`

* This setting defines a CSV list of hosts that the host in the message destination endpoint is checked against.
* This setting is optional. If you do not intend to use this, delete this attribute from the configuration file. This configuration is less secure but more flexible.
* If you include the list of safe hosts, include the host details of your API Portal (`https://<FQDN>:<port>`) in the list. The endpoint URI check succeeds only if the host name in the message destination endpoint is on the list.

`useAppSessions`

* Delegates the session management to the application. The default value is `true`.

`filteredUri`

* Specifies the URI of the SSO filter entry point for authentication. Set this value to `/sso/externallogin`.
* The SSO filter only manages login URI, for other requests the application must redirect to SSO filter to manage authentication.
* If the user is not authenticated, a SAML authentication request is built and sent to the IdP. Otherwise, the security token is forwarded to the application.

`logoutUri`

* Specifies the URI of the SSO filter entry point for logout process. Set this value to `/sso/externallogout`.

`logoutRedirectUri`

* Specifies the URI of the SSO filter entry point for logout process. Set this value to `/sso/externallogout`.
* The SSO filter generates a logout request and sends it to the IdP.

`logoutRedirectUri`

* Specifies the URI where to redirect after the logout process. Set this value to `/api/portal/v1.3/sso/proxylogout`.

`keystore`

* Specifies the name of a keystore where the private key of the SP is stored. The default value is `conf/sso.jks`.
* The SP uses the private key to decrypt messages from the IdP that the IdP has encrypted with the SP's public key. If the IdP does not encrypt the messages, the keystore can be omitted.
* When you set this attribute, the `keystorePassphrase` and `keyAlias` attributes must also be set.
* The keystore must be in the `classpath` of the application or in its working directory. The keystore format must be `.jks`.

`keystorePassphrase`

* Specifies the password of the keystore, if a password is set.

`keyAlias`

* Specifies the alias of the SP's private key in the keystore, if an alias is set.

`sessionIdCookieName`

* Sets the name of the cookie where the SSO session identifier is stored if the SSO module is the session manager. The recommended value is `spExternalSSOSessionId3`.

### `<AssertionConsumerService>`

This element specifies an entry point in API Portal for receiving SAML assertions from the IdP.

### `<SingleLogoutService>`

This element specifies the IdP URL where the logout responses are sent. Only `HTTP-POST` binding is managed.

### `<IdentityProviders>`

This element describes the entity that exchanges SAML messages with the SSO filter. This section contains a section called `<SamlIdentityProvider>`, which supports the following attributes:

`entityId` and `format`

* Sets the unique identifier of the IdP. These values must match the `entityId` and `format` values of the Issuer element in the SAML assertions.

`metadataUrl`

* Specifies the URL of the metadata file. The default value is `./idp.xml`.

`userNameAttribute`

* Specifies the name of the IdP attribute that provides the user name. The default value is `urn:oid:2.5.4.42`.
* When a user is authenticated, the SSO filter sets a principal on the `HttpServletRequest`. By default, the name of this principal is extracted from the `Subject` element in the assertions of an authentication response. If `userNameAttribute` is set, the name of the principal is set to the value of the specified IdP attribute

`verifyAssertionExpiration`

* Verifies the validity period of a SAML assertion. The default value is `true`.

### `<Mappings>`

This element contains the mappings to be applied on the IdP attributes.

### `<Features>`

You can set extra features in the configuration file to fine-tune the SP and the IdPs.