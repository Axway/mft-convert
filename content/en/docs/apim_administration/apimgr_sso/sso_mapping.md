{
"title": "Single sign-on reference",
  "linkTitle": "Single sign-on reference",
  "date": "2019-09-17",
  "description": "Single sign-on mapping syntax and examples, and a reference to elements in the `service-provider.xml` configuration file."
}

## Mapping syntax

An IdP sends information about the SSO user to an SP (API Manager) using attributes. These attributes contain information about the user, such as the user's name, department, organization, email address, phone number, and so on.

This section describes how to define mappings from an IdP to API Manager. An IdP can name attributes associated with the authenticated user in a variety of different ways (for example, `mail`, `email`, or `e-mail`). API Manager expects attributes with specific names, so you might need to transform the IdP attributes to the API Manager attributes using a rename mapping. In addition, an IdP might not provide some attributes that API Manager requires, so you might need to use a filter mapping to assign required attributes based on a filter.

The mappings are defined in the `Mappings` section of the `SAMLIdentityProvider` section in the `service-provider.xml` file.

Two types of mappings are supported:

* Rename mapping – This mapping enables you to rename an attribute from the IdP, keeping its value.
* Filter mapping – This mapping creates output attributes when a filter matches the input attributes from the IdP. For more information, see [Filter syntax](#filter-syntax).

The following sections describe the mandatory and optional attributes expected by API Manager, and give examples of mappings that you can use to provide them.

{{< alert title="Note" color="primary" >}}API Manager attribute names are all lowercase. The attribute names are case-sensitive.{{< /alert >}}

### Mandatory attributes

#### `name`

The logged in user name.

Sample RenameMapping if the IdP provides an attribute which should be renamed:

```
<RenameMapping source="user" target="name"/>
```

#### `organization`

The API Manager organization associated with the logged in user. The organization must already exist in API Manager. Organizations can be added by an API Manager administrator. See [Usage guidelines](/docs/apim_administration/apimgr_sso/saml_sso_config/#usage-guidelines) for additional details and exceptions.

The IdP does not need to provide this value. If it does and the IdP attribute has a different name, you can use a RenameMapping to transform it to an `organization` attribute. If the IdP does not provide the value associated with the organization at all, you can use an `OutputAttribute` to assign an organization to the logged in user. For example:

```
<OutputAttribute name="organization">Research</OutputAttribute>
```

#### `role`

The API Manager role associated with the logged in user. Permitted substring values: `Administrator`, `Operator`, `User`. See [Usage guidelines](/docs/apim_administration/apimgr_sso/saml_sso_config/#usage-guidelines) for details of how these SSO roles are mapped to API Manager roles.

The IdP does not need to provide this value. If it does and the IdP attribute has a different name, you can use a RenameMapping to transform it to a `role` attribute. If the IDP does not provide the value associated with the role at all, you can use an `OutputAttribute` to assign a role to the logged in user. For example:

```
<OutputAttribute name="role">Administrator</OutputAttribute>
```

### Optional attributes

#### `orgs2Role`

Use this property to be able to assign a user membership to multiple organizations in API Manager. This property is only available with [version 1.4 of the API Manager API](https://docs.axway.com/category/api).

The format required is as follows:

```
[{"organization1Name": "role"},{"organization2Name": "role"},.............]
```

The `role` value can be either `user` or `oadmin`. If the IdP provides this value but it has a different name, you can use a RenameMapping to transform the value to a `orgsRole` attribute. If the IdP does not provide the value associated with the `orgs2Role`, you can use an `OutputAttribute` to assign multiple organization membership to the logged in user. For example:

```
<OutputAttribute name="orgs2Role">[{"organization1Name": "user"}]</OutputAttribute>
```

{{< alert title="Note">}}If setting the `orgs2Role` attribute in the IdP, you might need to escape the quote character. For example:

```
[{\"organization1Name\": \"user\"}]
```

{{< /alert >}}

#### `mail`

The email address associated with the logged in user.

While the email is optional, it is strongly recommended that the IDP sends this value and that the mapping is present in the configuration file. This will help to future-proof SSO implementations in case email becomes a mandatory field in the future. Including it now will also help to limit outages and provide a more complete user profile.

Sample RenameMapping if the IdP provides an attribute which should be renamed:

```
<RenameMapping source="email" target="mail"/>
```

#### `description`

The description text associated with the logged in user.

Sample RenameMapping if the IdP provides an attribute which should be renamed:

```
<RenameMapping source="userDescription" target="description"/>
```

#### `department`

The department that the logged in user belongs to.

Sample RenameMapping if the IdP provides an attribute which should be renamed:

```
<RenameMapping source="businessUnit" target="department"/>
```

#### `telephonenumber`

The telephone number associated with the logged in user.

Sample RenameMapping if the IdP provides an attribute which should be renamed:

```
<RenameMapping source="phone" target="telephonenumber"/>
```

#### `userfullname`

The name associated with the logged in user.

Sample RenameMapping if the IdP provides an attribute which should be renamed:

```
<RenameMapping source="displayName" target="userfullname"/>
```

{{< alert title="Note" color="primary" >}}
By default the `userNameAttribute` populates the user name for API Manager from the IdP. If the IdP assertion response contains the user name in an attribute other than `userNameAttribute`, you must remap it to the target `name` for API Manager.

Previously, the account login name and name were both populated with the SAML response value for the `userNameAttribute` or `name` remapping value from the IdP. The `userfullname` target mapping now allows the API Manager account name field to be populated independently of the login name (which still comes from the `userNameAttribute` to `name` remapping).

When using an IdP such as Keycloak or Shibboleth, the `userNameAttribute` source attribute (see [`<Identity Providers>`](#identityproviders) is used to extract a principal name value. When using LDAP as an IdP, you can use the following rename mapping example, which uses a source attribute different from the one specified by `userNameAttribute`.

```
<LdapIdentityProvider entityId="[https://ldap|https://ldap/]" useSSL="false" userProvider="ldap://ldap.xxxxxxxxx/OU=Employees,DC=axway,DC=int" userFilter="mailNickName=\{USERNAME}" authIdentity="\{USERNAME}@[axway.com|https://axway.com/]" attributes="cn,mailNickName,mail,department" >
    <Mappings>
        <FilterMapping>
            <Filter>(department=xxxxxxxxxxx))</Filter>
            <OutputAttribute name="role">R&amp;D</OutputAttribute>
        </FilterMapping>
        <RenameMapping source="mailNickname" target="name"/>
    </Mappings>
</LdapIdentityProvider>
```

{{< /alert >}}

### Rename mapping example

If the IdP generates a attribute name that is different to the attribute name expected by API Manager (for example, `e-mail` rather than `mail`), you can use a RenameMapping directive to effectively rename the IdP attribute to the API Manager attribute.

For example, to rename the IdP attribute name `e-mail` to the API Manager attribute `mail`, use the following RenameMapping:

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
      <OutputAttribute name="role">administrator</OutputAttribute>
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
           <OutputAttribute name="role">API Server Administrator</OutputAttribute>
           <OutputAttribute name="organization">Production</OutputAttribute>
       </FilterMapping>
    </Mappings>
```

### Filter syntax

A filter is specified using the [LDAP Search Filter](https://docs.oracle.com/cd/E19528-01/819-0997/gdxpo/index.html) syntax. Only a subset of the full syntax is supported as detailed in this section.

A filter consists of one or more criteria. If more than one criterion exists in one filter definition, they can be concatenated by logical operators.

#### Criteria

* The criteria must be put in parentheses.
* A criteria can only be an equality.

Example:

```
(givenName=Sandra)
```

{{< alert title="Note" color="primary" >}}
If the criteria value contains an equal character `=`, you must escape it using the hexadecimal code `3d`. For example:

```
(subjectDN=CN\3dcommon-name,OU\3dorganization-unit,O\3dorganization)
```

{{< /alert >}}

#### Operators

* The logical operators must be placed in front of the criteria.
* The whole term must be put in parentheses.

##### AND operator

criteria1 AND criteria2:

```
(&(criteria1)(criteria2))
```

More than two criteria:

```
(&(criteria1)(criteria2)(criteria3)...(criteria n)
```

##### OR operator

criteria1 OR criteria2:

```
(|(criteria1) (criteria2))
```

More than two criteria:

```
(|(criteria1) (criteria2) (criteria3)...(criteria n))
```

##### NOT operator

NOT criteria1:

```
(!(criteria1))
```

##### Nested operators

You can combine logical operators.

(criteria1 OR criteria2) AND ( criteria3 OR criteria4):

```
(&(|(criteria1) (criteria2))(|(criteria3) (criteria4)))
```

## `service-provider.xml` configuration file reference

This section describes the elements in the `service-provider.xml` configuration file.

### `<SSOConfiguration>`

This is the root element of the configuration descriptor. This section contains one `<CertificateValidation>` element (optional), one `<ServiceProvider>` element and one `<IdentityProviders>` element.

### `<CertificateValidation>`

This element describes the certificate validation. You can configure certificate validation to validate the SP and IdP certificates at startup. The following attributes are supported:

| Attribute                 | Description                                                                                                                                                                                                                      |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pathValidation                       | If set to `true`, the certification path for each certificate will be checked. If set to `false`, the agent verifies only the validity period of each certificate.                                                                |
| enableRevocation                     | If set to `true`, the agent also verifies if the certificates are not revoked.                                                                                                                                                   |
| trustStorePath                       | The path to the trust store containing the trusted certificates.                                                                                                                                                                 |
| trustStorePassword                   | The password to access the trust store.                                                                                                                                                                                           |
| intermediateStorePath (optional)     | The path to a store containing intermediate certificates that can appear in certificate chains.                                                                                                                                   |
| intermediateStorePassword (optional) | The password to access the intermediate certificates store.                                                                                                                                                                       |
| delayBetweenValidations (optional)   | Defines at which interval certificate validation occurs, in hours.                                                                                                                                                               |

To disable certificate validation, set `pathValidation` to false. For example:

```
<CertificateValidation
    pathValidation="false"
    ...
</CertificateValidation>
```

### `<ServiceProvider>`

This element describes the SP. The following attributes are supported:

| Attribute           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| entityId            | Sets the unique identifier of the SP. This identifier is sent to the IdP so it can know who is requesting an authentication or logging out.                                                                                                                                                                                                                                                                                                                                                                    |
| useAppSessions      | Delegates the session management to the application. The default value is `true`.                                                                                                                                                                                                                                                                                                                                                                                                                              |
| filteredUri         | Specifies the URI of the SSO filter entry point for authentication. Set this value to `/sso/login`. The SSO filter only manages login URI, for other requests the application must redirect to SSO filter to manage authentication. If the user is not authenticated, a SAML authentication request is built and sent to the IdP. Otherwise, the security token is forwarded to the application.                                                                                                               |
| logoutUri           | Specifies the URI of the SSO filter entry point for logout process. Set this value to `/sso/logout`. The SSO filter generates a logout request and sends it to the IdP.                                                                                                                                                                                                                                                                                                                                        |
| logoutRedirectUri   | Specifies the URI where to redirect after the logout process. Set this value to `/api/portal/v1.3/sso/login` to redirect the user to the login page after logout.                                                                                                                                                                                                                                                                                                                                              |
| keystore            | Specifies the name of a keystore where the private key of the SP is stored. The default value is `conf/sso.jks`. The SP uses this private key to sign messages to the IdP, and to decrypt messages from the IdP that the IdP has encrypted with the SP's public key. When you set this attribute, you must also set the associated attributes `keystorePassphrase` and `keyAlias`. The keystore must be in the `classpath` of the application or in its working directory. The keystore format must be `.jks`. |
| keystorePassphrase  | Specifies the password of the keystore.                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| keyAlias            | Specifies the alias of the SP's private key in the keystore.                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| sessionIdCookieName | Sets the name of the cookie where the SSO session identifier is stored if the SSO module is the session manager. The recommended value is `spSessionId2`.                                                                                                                                                                                                                                                                                                                                                      |

### `<AssertionConsumerService>`

This element specifies an entry point for receiving SAML assertions from the IdP.

### `<SingleLogoutService>`

This element specifies the IdP URL where the logout responses are sent. Only `HTTP-POST` binding is managed.

### `<IdentityProviders>`

This element describes the entity that exchanges SAML messages with the SSO filter. This section contains a section called `<SamlIdentityProvider>`, which supports the following attributes:

| Attribute                 | Description                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| entityId format           | Sets the unique identifier of the IdP. These values must match the `entityId` and `format` values of the Issuer element in the SAML assertions. If the SAML assertion does not have the `format` set, omit the `format` element.                                                                                                                                                                                                                      |
| metadataUrl               | Specifies the URL of the metadata file. The default value is `./idp.xml`.                                                                                                                                                                                                                                                                                                                                                                             |
| userNameAttribute         | Specifies the name of the IdP attribute that provides the user name. The default value is `urn:oid:2.5.4.42`. When a user is authenticated, the SSO filter sets a principal on the `HttpServletRequest`. By default, the name of this principal is extracted from the `Subject` element in the assertions of an authentication response. If `userNameAttribute` is set, the name of the principal is set to the value of the specified IdP attribute. |
| verifyAssertionExpiration | Verifies the validity period of a SAML assertion. The default value is `false`.                                                                                                                                                                                                                                                                                                                                                                       |

### `<Mappings>`

This element contains the mappings to be applied on the IdP attributes.

### `<Features>`

You can set extra features in the configuration file to fine-tune the SP and the IdPs.
