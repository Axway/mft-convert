{
"title": "Additional integrity filters",
  "linkTitle": "Additional integrity filters",
  "weight": 97,
  "date": "2019-10-17",
  "description": "Sign and verify JWT and SMIME messages."
}
## JWT Sign filter

You can use the **JWT Sign** filter to sign arbitrary content (for example, a JWT claims set). The result is called JSON Web Signature (JWS).

JWS represents digitally signed or MACed content using JSON data structures and base64url encoding.

A JWS represents the following logical values:

* JOSE header
* JWS payload
* JWS signature

The signed content is outputted in JWS Compact Serialization format, which is produced by base64 encoding the logical values and concatenating them with periods (`‘.’`) in between. For example:

```
{"iss":"joe",
"exp":1300819380,
"http://example.com/is_root":true}
```

When the JOSE header, JWS payload, and JWS signature is combined as follows:

```
BASE64URL(UTF8(JWS Protected Header)) '.'
BASE64URL(JWS Payload) '.'
BASE64URL(JWS Signature)
```

The following string is returned:

```
eyJhbGciOiJSUzI1NiJ9
.
eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFt
cGxlLmNvbS9pc19yb290Ijp0cnVlfQ
.
cC4hiUPoj9Eetdgtv3hF80EGrhuB__dzERat0XF9g2VtQgr9PJbu3XOiZj5RZmh7
AAuHIm4Bh-0Qc_lF5YKt_O8W2Fp5jujGbds9uJdbF9CUAr7t1dnZcAcQjbKBYNX4
BAynRFdiuB—f_nZLgrnbyTyWzO75vRK5h6xBArLIARNPvkSjtQBMHlb1L07Qe7K
0GarZRmB_eSN9383LcOLn6_dO—xi12jzDwusC-eOkHWEsqtFZESc6BfI7noOPqv
hJ1phCnvWh6IeYI2w9QOYEUipUTI8np6LbgGY9Fs98rqVt5AXLIhWkWywlVmtVrB
p0igcN_IoypGlUPQGe77Rw
```

Configure the following settings on the **JWT Sign** window:

* **Name**: Enter an appropriate name for the filter to display in a policy.
* **Token location**: Enter the selector expression to obtain the payload to be signed. The content can be JWT claims, encrypted token, or you can enter a different option.

### Signature Key and Algorithm

On the **Signature Key and Algorithm** tab, you can select either a symmetric or an asymmetric key to sign the JWT. Select the appropriate option and configure the fields in the corresponding section.

* **Key type**: Select whether to sign with a private key (asymmetric) or HMAC (symmetric key).

#### Asymmetric key type

If you selected the asymmetric key type, configure the following fields in the **Asymmetric** section:

* **Signing key**: Select a certificate with a private key from the certificate store. The private key is used to sign the payload, while the certificate is used to generate key related headers in the JOSE header.
* **Selector expression**: Alternatively, enter a selector expression to get the alias of the private key in the certificate store.
* **Algorithm**: Select one of the available algorithms to sign the JWT.

  | Algorithm | Description                                    |
  | --------- | ---------------------------------------------- |
  | ES256     | ECDSA using P-256 and SHA-256                  |
  | ES384     | ECDSA using P-384 and SHA-384                  |
  | ES512     | ECDSA using P-521 and SHA-512                  |
  | RS256     | RSASSA-PKCS1-v1_5 using SHA-256                |
  | RS384     | RSASSA-PKCS1-v1_5 using SHA-384                |
  | RS512     | RSASSA-PKCS1-v1_5 using SHA-512                |
  | PS256     | RSASSA-PSS using SHA-256 and MGF1 with SHA-256 |
  | PS384     | RSASSA-PSS using SHA-384 and MGF1 with SHA-384 |
  | PS512     | RSASSA-PSS using SHA-512 and MGF1 with SHA-512 |

  The selected algorithm must be compatible with the selected certificate. When a certificate is selected from the certificate store, this will be validated when the filter is saved. A selector based alias can only be validated at runtime, and an incompatible certificate will cause the filter to fail.

* **Use Key ID (kid)**: Select to add a `kid` header parameter to the JOSE header part of the token. The `kid` header parameter is a hint indicating which public/private key pair was used to secure the JWS. The following options are available:

    * **Use Cert Alias**: The alias of the selected Certificate.
    * **Compute Cert x5t**: A Base64Url encoded SHA1 digest (thumbprint) of the DER encoded X509 Certificate.
    * **Compute Cert x5t#256**: A Base64Url encoded SHA256 digest (thumbprint) of the DER encoded X509 Certificate.
    * **Selector Expression**: A static string or selector expression can be used to set a custom key ID that has a contextual meaning.

#### Symmetric key type

If you selected the **Symmetric key type** option, complete the following fields in the **Symmetric** section:

* **Shared key**: Enter the shared key used to sign the payload. The key should be given as a base64-encoded byte array and must use the following minimum lengths depending on the selected algorithm used to sign:

  | Algorithm                  | Minimum key length  |
  | -------------------------- | ------------------- |
  | HMAC using SHA-256 (HS256) | 32 bytes (256 bits) |
  | HMAC using SHA-384 (HS384) | 48 bytes (384 bits) |
  | HMAC using SHA-512 (HS512) | 64 bytes (512 bits) |
* **Selector expression**: Alternatively, enter a selector expression to obtain the shared key. The value returned from the selector should contain:

    * Byte array (possibly produced by a different filter)
    * Base64-encoded byte array
* **Algorithm**: Select the algorithm used to protect the token.
* **Use Key ID (kid)**: Adds a `kid` header parameter to the JOSE header part of the token. The `kid` header parameter is a hint indicating which public/private key pair was used to secure the JWS. This value can be defined as a static string or a selector expression.

### Signature JOSE Header

This tab configures which claims are present in the JWT header.

* **Generate 'typ' claim**.
* **JWK Set URL (jku)**: Specifies the `jku`. If the selector evaluates as empty or null, the filter will fail.
* **Embed all key related claims in the 'jwk' claim (except for 'jku')**: If selected, all of the following header claims will be embedded in a JWK object within the header.
    * **Generate 'x5t' thumbprint**: SHA1 thumbprint derived from the signing certificate.
    * **Generate 'x5t#256' thumbprint**: SHA256 thumbprint derived from the signing certificate.
    * **Include 'x5c' certificate chain**: Adds the PEM encoded certificate chain of the signing certificate.
    * **Include 'x5u' certificate URL**: SpecifIES the `x5u`. If the selector evaluates as empty or null, the filter will fail.

For more information on each header, see [JWS RFC 7515](https://tools.ietf.org/html/rfc7515#section-4).

Enabling all of the settings produces a header with the following structure:

```
{
  "jku": "https://axway.com/api/jwk_uri",
  "typ": "JOSE",
  "jwk": {
    "kty": "RSA",
    "x5t#S256": "3WcxVjJWOUxIIgMmLOf20hj-lR2qn-mwXZHIU8D9CAk",
    "e": "AQAB",
    "x5t": "2pdrqe1djoNnHxebh_MfLYl3hFg",
    "kid": "CN=Change this for production",
    "x5c": [
      "MIICwzCCAasCBgE6HBsdpzANBgkqhkiG9w0BAQUFADAlMSMwIQYDVQQDExpDaGFuZ2UgdGhpcyBmb3IgcHJvZHVjdGlvbjAeFw0xMjEwMDExMTMyMDBaFw0zNzEwMDExMTMyMDBaMCUxIzAhBgNVBAMTGkNoYW5nZSB0aGlzIGZvciBwcm9kdWN0aW9uMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAm2I2+GHcXXzwyjqMP6E4shjxfpAfgqbCY/nF5oTq0SkcRKvsdJzuLbmufkqx1rQqxwF/aZnbZppcVtR4TAhExmo2NnV7WjSwdd+EynQJrkWlsuK1UQ3JHMo5iAAEQ11xoMBIsUwfg5HYKCELmjnWetwhm5aUJ9Gq45v9kzeZki2oCoVe5LQfVVHEYssr+SfVrhi6+OffeefgCRse6vv5T4zlh4xXKDNUsBxYYB3Vg97tDcdgpfx8BudpBx+1ITk9Dazu8eegXN5KdRqJGgM5LSRIWjK+OumR1a2ReUcXlglWTVfsG43UUUby2bql3E3uc7XpxzQaPpt4aDqfOYMUxwIDAQABMA0GCSqGSIb3DQEBBQUAA4IBAQAl+yHca9jCZ/zVgtITGWGKQiNb8UqFJE+QxmLt+j2lEWpG3Fd1M40faRrDujbk8WvG4Iz/NamlvvkbpbMSRY67lPpjsZOKlezTTE2YQTtyuFT7QQTYHYPZWK4Dg8QisMI5vHnrzsPc9ZAHm+IZrxbuVXsZQoU7qyaMdG27WWVa6vJ4nXjuMO6sOtl+UnEXpn3vCpNzkkbJW2LvFCs0Ymnx7Wet3inskOKg//AGuv+m3rD/Byphd8Iblt3jSNDwMcG+Yhpi/Wd50iMFFkTnrkEmosvqWL5j6N7eJZszgdL7Zz9ztASutzU4a0YFpv111NxpBdNpphOVED85IbRHxTjL"
    ],
    "x5u": "https://axway.com/path/to/cert",
    "n": "m2I2-GHcXXzwyjqMP6E4shjxfpAfgqbCY_nF5oTq0SkcRKvsdJzuLbmufkqx1rQqxwF_aZnbZppcVtR4TAhExmo2NnV7WjSwdd-EynQJrkWlsuK1UQ3JHMo5iAAEQ11xoMBIsUwfg5HYKCELmjnWetwhm5aUJ9Gq45v9kzeZki2oCoVe5LQfVVHEYssr-SfVrhi6-OffeefgCRse6vv5T4zlh4xXKDNUsBxYYB3Vg97tDcdgpfx8BudpBx-1ITk9Dazu8eegXN5KdRqJGgM5LSRIWjK-OumR1a2ReUcXlglWTVfsG43UUUby2bql3E3uc7XpxzQaPpt4aDqfOYMUxw"
  }
  "alg": "RS256",
}
```

### Payload

This tab configures the contents of the JWS payload.

#### Generate JWS

Select this option to generate the JWS payload from an arbitrary data sequence taken from a message attribute using a selector expression.

* **Selector expression**: A selector is used to generate the payload. If the selector evaluates as empty or null, the filter will fail.
* **Generate 'cty' claim (forced for nested JWT/JWS)**: If selected, the `cty` header parameter is generated by examining the Content-Type of the attribute referenced by the selector. For ore information on the `cty` header, see [JWS RFC 7515](https://tools.ietf.org/html/rfc7515#section-4.1.10).

#### Generate JWT

Select this option to create a JWT from a JSON template.

You can add a standard set of JWT claims to the template to produce the JWT. For more information on each claim, see [JWT RFC 7519](https://tools.ietf.org/html/rfc7519#section-4.1). This option also supports OpenID specific claims `at_hash` and `c_hash`. For more information on these claims, see [Open ID Connect Core Specification](https://openid.net/specs/openid-connect-core-1_0.html).

* **JSON Template**: A JSON object containing a JWT claims set. Use this option to specify Public Claim Names and Private Claim Names.

The following claims allow to specify the contents of the Registered Claim Names:

* **Issuer (iss)**: Specifies the `iss` claim. If the selector evaluates as empty or null, the claim will not be added to the JWT.
* **Subject (sub)**: Specifies the `sub` claim. If the selector evaluates as empty or null, the claim will not be added to the JWT.
* **ID (jti)**: Specifies the `jti` claim. If the selector evaluates as empty or null, the claim will not be added to the JWT.
* **Audience (aud)**: A single case-sensitive string containing a StringOrURI for the `aud` claim.
* **Delay before usage (nbf) (relative to system time)**: The value in seconds for the `nbf` claim.
* **Delay before expiration (exp) (relative to 'nbf')**: The value in seconds for the `exp` claim.
* **Include system time (iat)**: If selected, the `iat` (issued at) time is included in the generated payload, this will be the Unix epoch time in seconds.

The following options allow to add claims to support OpenID Connect.

* **Access Token (generate 'at_hash' half hash)**: Specifies the Access Token used to generate the `at_hash` claim. If the selector evaluates as empty or null, the claim will not be added to the JWT.
* **Authorization Code (generate 'c_hash' half hash)**: Specifies the Authorization Code used to generate the `c_hash` claim. If the selector evaluates as empty or null, the claim will not be added to the JWT.

### Advanced (JWS/JWT)

You can configure the following settings in this tab:

* **Critical extensions (crit)**: Set of values to add to the `crit` header.
* **Extend JWS Header using a Policy**: Select a policy that when called, the contents of its invocation are added to the JWS Header.
* **Alternate JWT Audiences**: Allows to set the JWT `aud` value to be an array of case-sensitive strings, each string containing a StringOrURI value.
* **Extend JWT Payload using a Policy**: Select a policy that when called, the contents of its invocation are added to the JWT Payload.

### Sign Output

The **Output** tab lets you configure how the filter returns a JWS object. You can configure the following options:

* **Set an attribute with the generated signature**: Takes an **Attribute Name** as a parameter. When the filter completes, the JWS object will be accessible in that attribute through the use of a selector.
* **Add the generated signature to an HTTP Header**: Sets the JWS as an HTTP header on the current circuit Message. You must enter the header name, and select one of the following options:
    * **Overwrite existing value**: Select this option if the header already exists. If this option is disabled, the header will be appended creating multiple headers of the same name.
    * **Use body headers**: The header is stored with the body, `${content.body}`.
    * **Use message headers**: The header is stored in a distinct headers message attribute, `${http.headers}`.
* **Detach signature with unencoded payload**: Detaching the payload will create a JWS token in the format `<header>..<signature>`. This payload is stored in a separate message attribute to be returned to the user, typically in the `content.body`. This option is used when signing a JSON response, and it supports the OBIE Open Banking message signing specification.

For more information on JWS Detached Content, see [Appendix F of RFC 7515](https://tools.ietf.org/html/rfc7515#appendix-F). This payload is unencoded, and the JWS header contains a header specifying `b64=false` as described in [RFC 7797 Unencoded Payload Option](https://tools.ietf.org/html/rfc7797).

{{< alert title="Note" color="primary" >}}
The message attribute names for **Generated signature** and **Detach signature payload** must be distinct from one another. Using the same attribute name will result in a validation error in Policy Studio. {{< /alert >}}

## JWT Verify filter

You can use the **JWT Verify** filter to verify a signed JSON Web Token (JWT) with the token payload. Upon successful verification, the **JWT Verify** filter stores the original header and payload in message attributes, so they can be available in the subsequent filters and in callback policies. For example, when you verify the following signed JWT payload input:

```
eyJhbGciOiJSUzI1NiJ9
.
eyJpc3MiOiJqb2UiLA0KICJleHAiOjEzMDA4MTkzODAsDQogImh0dHA6Ly9leGFt
cGxlLmNvbS9pc19yb290Ijp0cnVlfQ
.
cC4hiUPoj9Eetdgtv3hF80EGrhuB__dzERat0XF9g2VtQgr9PJbu3XOiZj5RZmh7
AAuHIm4Bh-0Qc_lF5YKt_O8W2Fp5jujGbds9uJdbF9CUAr7t1dnZcAcQjbKBYNX4
BAynRFdiuB—f_nZLgrnbyTyWzO75vRK5h6xBArLIARNPvkSjtQBMHlb1L07Qe7K
0GarZRmB_eSN9383LcOLn6_dO—xi12jzDwusC-eOkHWEsqtFZESc6BfI7noOPqv
hJ1phCnvWh6IeYI2w9QOYEUipUTI8np6LbgGY9Fs98rqVt5AXLIhWkWywlVmtVrB
p0igcN_IoypGlUPQGe77Rw
```

The resulting payload output is:

```
{"iss":"joe",
"exp":1300819380,
"http://example.com/is_root":true}
```

The **JWT Verify** filter automatically detects whether the input JWT is signed with hash-based message authentication code (HMAC) or asymmetric key, and it uses the corresponding settings as appropriate. For example, you can configure verification with HMAC or certificate, depending on the type of JWT received as input.

You can configure the following settings on the **JWT Verify** dialog:

**Name**: Enter an appropriate name for the filter to display in a policy.

**Token location**: Enter the selector expression to retrieve the JWT to be verified. This must contain the value of token in the format of `HEADER.PAYLOAD.SIGNATURE`, but without the `Bearer` prefix. You can use a filter, such as [Retrieve attribute from HTTP header](/docs/apim_policydev/apigw_polref/attributes_retrieve/#retrieve-from-http-header-filter), in your policy to get the token from any header. For example: `${http.headers["Authorization"].substring(7)}`

### Key and Algorithm

On the **Key and Algorithm** tab you can configure the location of the verification key, which validates the JWS token. You can choose one of these two options for selecting the key, **Call Policy to discover key** or **Select static key or selector**.

#### Call policy to discover key

Use this option to select a policy to find the appropriate key to verify a JWS Token.

You must ensure that the key in the selected policy follows these two requirements:

* It is either in JWK (a single JWK or a JWK set) or PEM (a PEM encoded X.509 certificate or an RSA Public key) format. You can select one of each, or both options.
* It is placed in the `content.body` message attribute.

If no key is returned or the key is not in the correct format, the filter fails. If the key cannot verify the JWS signature, the filter fails.

Before the policy is called, two message attributes are created containing the JWS header and payload, `jwt.header` and `jwt.body`, respectively. If a detached signature is configured, the `jwt.body` is not created, and the payload must be provided separately.

You can use these attributes in the discovery policy to locate the correct key. For example, you can use:

* `${jwt.header.jku}` with the [Connect to URL](/docs/apim_policydev/apigw_polref/routing_common/#connect-to-url-filter) filter to retrieve a JWK from an external source.
* `${jwt.header.x5u}` to retrieve a PEM encoded certificate from an external source.
* `${jwt.body}` to identify the subject of the JWS, and retrieve a key from a data source, such as the KPS.

Other useful headers for identifying the key are: `${jwt.header.kid}` and `${jwt.header.jwk}`.

#### Select static key or selector

This option allows you to directly specify a certificate for asymmetric keys, a shared secret for HMAC base JWS tokens, or a JWK. Each of these options can be explicitly enabled or disabled. At runtime, the filter will identify the appropriate key based on the algorithm in the token header. If an asymmetric or symmetric key is not available, the filter will use the JWK.

You can configure the following optional settings in the **Verify using RSA/EC public key** section:

**X509 certificate**: Select the certificate that is used to verify the payload from the certificate store.

{{< alert title="Note" color="primary" >}}Asymmetric keys are associated with the x509 certificate, but for verification, you only need the public key, which is encoded in the certificate. Alternatively, you can use a JSON Web Key (JWK) with a **Connect to URL** filter to download the key from a known source.{{< /alert >}}

**Selector expression**: Alternatively, enter a selector expression to retrieve the alias of the certificate from the certificate store.

You can configure the following optional settings in the **Verify using symmetric key** section:

**None**: Select if you do not want to verify tokens signed with HMAC.

**Shared key**: Enter the shared key that was used to sign the payload. The key should be provided as a base64-encoded byte array.

**Selector expression**: Alternatively, enter a selector expression to obtain the shared key. The value returned by the selector should contain:

* Byte array (possibly produced by a different filter)
* Base64-encoded byte array

You can configure the following optional setting in the **JWK from external source** section:

**JSON web key**: You can verify signed tokens using a selector expression containing the value of a `JSON Web Key (JWK)`. The return type of the selector expression must be of type String.

#### Accepted Algorithms

This option shows a list of **Accepted Algorithms**, which is populated with all the algorithms available for JWT signing. It requires that at least one algorithm is selected. The selected algorithms will be validated against the "alg" header of the JWT token being processed. If none are selected, the following message is displayed, **You must enter a value for 'Accepted Algorithms'.**

The runtime validation works as follows:

* Successful, if the "alg" value of the incoming JWT token matches an algorithm on the accepted list.
* Fail with `reason: The JWS token is not correct`, if the "alg" value of the incoming JWT is null.
* Fail with `reason: The JWS token is not correct`, if there is no "alg" value in the incoming JWT.
* Fail with `reason: Alg received not supported in the 'Accepted Algorithms' list defined in the JWT verification filter`, if the "alg" value of the incoming JWT is not selected from the list of accepted algorithms.

### Header Validation

#### Critical Headers

You can add a list of acceptable “crit” headers (list of JWT claims), which will be validated against the list of claims present in the “crit” header of the JWT token being processed. The validation works as follows:

* Successful, if all claims present in the “crit” header list of the JWT token match the lists you have configured.
* Fail with `reason: unknown header`, if any of the claims present is the JWT token do not match the lists you have configured.
* Fail with `reason: unknown header`, if the JWT token has a crit header list specified and you did not configured any list for your JWT verify filter.
* Fail with `reason: crit header cannot be empty`, if the JWT token has an empty “crit” header list.

#### Claims

**Header claim validation policy**: You can select a policy that allows you to validate a claim. The header value is made available to the policy via the `${jwt.header}` message attribute. For example, the following selector returns a `jwk` claim from the header: `${jwt.header.jwk}`. If a JWT Header claim policy is defined, the validation of the claim works as follows:

* Successful: The policy is invoked and displayed in the policy execution path, in [Traffic monitor](/docs/apim_reference/monitor_traffic_events_metrics/).
* Fail: The failure path from the JWT Verify filter is executed.

**Type & Content Type Headers**: You can add a list of acceptable "typ" headers and a list of acceptable "cty" headers. The headers will be validated against the "typ" and "cty" header values present in the JWT being processed.

The list of acceptable headers for either "typ" or "cty" must entirely match an incoming JWT header value. For example, an incoming token with a content type `json` will not match an `application/json` string in the accepted list.

The runtime validation works as follows:

* Successful, if the "typ" or "cty" value of the incoming JWT matches a value on the accepted lists.
* Successful, if the acceptable "typ" or "cty" lists are empty.
* Fail with `reason: unknown header`, if the "typ" or "cty" value of the incoming JWT is not present in the accepted list.
* Fail with `reason: typ/cty header cannot be empty`, if the "typ" or "cty" value of the incoming JWT is empty, and a list is provided.

### Advanced

You can configure the following settings in this tab:

**Detached Signature**: The format of a detached JWS is `<header>..<signature>`. When a JWS is in a detached format, its payload is omitted from the JWS and it is sent separately. The payload missing from the JWS must be added to the Message via a selector expression, which is typically a `${content.body}`, but it is configurable in the filter.

The detached signature is disabled by default in the JWT Verify filter. To enable it, select **Support detached payload** and specify the location of the payload, which defaults to `${content.body}`.

The validation of the detached signature works as follows:

* When detached signatures are enabled and a JWS token with a detached signature is processed at runtime, the filter will:
    * pass, if the correct payload can be found in the location specified.
    * fail, if the correct payload cannot be found in the location specified.
* When detached signatures are disabled and a JWS token with a detached signature is processed at runtime, the filter will fail.
* When detached signatures are either enabled or disabled, and a compact JWS token is processed at runtime, the filter will pass if the token is correct.

For more information about detached JWS, see [Appendix F of JWS RFC 7515](https://tools.ietf.org/html/rfc7515#appendix-F).

{{< alert title="Note" color="primary" >}}When using detached signatures, the detached payload must not be base64 encoded. You must add a `"b64: false"` header claim to the JWS token to enforce this behavior. See [JWS Unencoded Payload Option RFC 7797](https://tools.ietf.org/html/rfc7797) for more information.{{< /alert >}}

**Validate payload claims**: Select a policy to perform additional validation of the token payload. The payload value is made available to the policy via the `${jwt.body}` message attribute. If a detached signature is configured, the `${jwt.body}` is not created and the attribute with the location of the payload, which defaults to `${content.body}`, should be used instead.

### Verify Output

The **Output** tab lets you configure how the filter returns a JWS object. Configuring the payload options in this tab is not necessary if **Detached Signature** is selected in the **Advanced** tab, as the payload will be available in the `detached payload message` attribute instead.

You can configure the following options:

* **Set payload as Content Body**: Writes the JWT payload to the `${content.body}` attribute.
* **Use cty header for content-type**: Sets the content-type of the message body to be the value of the `cty` header in the JWT.
* **Default content-type**: Defines a default content-type, which is used if **Use cty header for content-type** is not selected, or if it is selected but not present. Default value is `application/octet-stream`.
* **Set payload message attribute**: Takes an **Attribute Name** as a parameter. When the filter completes, the JWT payload is accessible in that attribute through the use of a selector. There is also an **Attribute Format** selector to specify whether the attribute should be a `string` or a `JSON object`.
* **Set header message attribute**: Takes an **Attribute Name** as a parameter. When the filter completes, the JWT header is accessible in that attribute through the use of a selector.

{{< alert title="Note" color="primary" >}}When setting the payload message attribute with the **Attribute Format** set to *JSON*, the filter will fail if the payload is not valid *JSON*.{{< /alert >}}

### Additional JWT verification steps

The **JWT Verify** filter verifies the JWT signature with the token payload only. The following additional verification steps are also typically required:

* Make sure that the certificate used to generate the signature is valid (for example, check that it is not blacklisted or expired). You can use the API Gateway CRL and OCSP filters in your policy for this step.
* Validate the JWT token claims. For example, this includes the following checks:

    * `aud`: Audience—check that the token has been created for the correct user.
    * `iss`: Issuer—check that the token was issued by a trusted token provider.
    * `exp`: Expiry time—check that the token has not already expired.

## Sign SMIME message filter

You can use the **SMIME Sign** filter to digitally sign a multipart Secure/Multipurpose Internet Mail Extensions (SMIME) message when it passes through the API Gateway core pipeline. The recipient of the message can then verify the integrity of the SMIME message by validating the Public Key Cryptography Standards (PKCS) #7 signature.

Complete the following fields:

**Name**: Enter an appropriate name for the filter to display in a policy.

**Sign Using Key**: Select the certificate that contains the public key associated with the private signing key to be used to sign the message.

**Create Detached Signature in Attachment**: Specifies whether to create a detached digital signature in the message attachment. This is selected by default. For example, this is useful when the software reading the message does not understand the PKCS#7 binary structure, because it can still display the signed content, but without verifying the signature.

If this is not selected, the message content is embedded with the PKCS#7 binary signature. This means that user agents that do not understand PKCS#7 cannot display the signed content. Intermediate systems between the sender and final recipient can modify the text content slightly (for example, line wrapping, whitespace, or text encoding). This might cause the message to fail signature validation due to changes in the signed text that are not malicious, nor necessarily affecting the meaning of the text.

## Verify SMIME message filter

You can use the **SMIME Verify**
filter to check the integrity of a Secure/Multipurpose Internet Mail Extensions (SMIME) message. This filter enables you to verify the Public Key Cryptography Standards (PKCS) #7 signature over the message.

You can select the certificates that contain the public keys to be used to verify the signature. Alternatively, you can specify a message attribute that contains the certificate with the public key to be used.

Complete the following fields:

**Name**: Enter an appropriate name for the filter to display in a policy.

**Certificates from the following list**: Select the certificates that contain the public keys to be used to verify the signature. This is the default option.

**Certificate in attribute**: Alternatively, enter the message attribute that specifies the certificate that contains the public key to be used to verify the signature. Defaults to `${certificate}`.

**Remove Outer Envelope if Verification is Successful**: Select this option to remove the PKCS#7 signature and all its associated data from the message if it verifies successfully.
