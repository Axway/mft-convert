{
    "title": "Example: Configure OAuth (External) security for a front-end API",
    "linkTitle": "Example: Secure a front-end API with OAuth",
    "weight": "61",
    "date": "2020-07-10",
    "description": "This example shows how to configure API Gateway and API Manager to secure a front-end API using an OAuth (External) security device."
}

In this example, the OAuth server returns a JWT bearer, and API Gateway analyzes this JWT to grant or refuse access to the resource.

Here is a sample payload from the JWT returned by the OAuth server:

```json
{
 "jti": "6723520f-abe9-45ec-8b6f-5092455342a4",
 "exp": 1568363943,
 "nbf": 0,
 "iat": 1568363643,
 "iss": "https://oauthnserver/auth/realms/oauth",
 "aud": "account",
 "sub": "c4884fd3-fe51-43ae-8119-dd32ccb49981",
 "typ": "Bearer",
 "azp": "client",
 "auth_time": 0,
 "session_state": "1015c238-5760-4d59-9cec-e8b6c5877a37",
 "scope": "openid email profile",
 "email_verified": false,
 "preferred_username": "user"
}
```

API Manager needs the following information:

* `oauth.token.client_id` which is the `azp` parameter
* `oauth.token.scopes` which are in the `scope` parameter
* `oauth.token.valid` which can be verified by using the `exp` parameter to see if the token is expired or not.

## Configure a token information policy

Follow these steps in Policy Studio.

### Import a certificate

First, import the certificate to use to sign the JWT under **Environment Configuration > Certificates and Keys > Certificates**.

### Configure the token information policy

Next, configure a token information policy similar to this example:

![Token information policy](/Images/docbook/images/api_mgmt/api_mgmt_oauth_ext_tokeninformationpolicy.png)

This example policy processes the JWT using the following filters:

* Verify the JWT using a **JWT Verify** filter:

    ![JWT Verify filter](/Images/docbook/images/api_mgmt/api_mgmt_oauth_ext_jwtverifyfilter.png)

    * API Manager returns the token in the `access_token` attribute, so set **Token location** with an `${access_token}` selector.
    * Use the **X509 certificate** you imported earlier.

* The payload of the JWT is now available in the `jwt.body` attribute, but it needs to be set in the `content.body` attribute to  extract information from it. Use a **Set Message** filter:

    ![Set Message filter](/Images/docbook/images/api_mgmt/api_mgmt_oauth_ext_setmessagefilter.png)

* Use a **JSON Path** filter to extract the required information:

    ![JSON Path filter](/Images/docbook/images/api_mgmt/api_mgmt_oauth_ext_jsonpathfilter.png)

    * `oauth.token.client_id` is extracted from `$.azp`
    * `oauth.token.scopes` is extracted from `$.scope`
    * `oauth.token.exp` is extracted from `$.exp`
    * Everything is unmarshalled as `java.lang.String`

* The scopes in the token are space separated, but API Manager needs them to be comma separated. Use a **String Replace** filter to format the attribute:

    ![String Replace filter](/Images/docbook/images/api_mgmt/api_mgmt_oauth_ext_stringreplacefilter.png)

    * The filter changes the `oauth.token.scopes` attribute
    * It replaces space characters (specified in the **Match > Straight** field) with commas (specified in **Replacement string** field)

* Validate the expiration time of the token with a **Validate Timestamp** filter:

    ![Validate Timestamp filter](/Images/docbook/images/api_mgmt/api_mgmt_oauth_ext_validatetimestampfilter.png)

    * The timestamp is in the attribute `oauth.token.exp`
    * The timestamp is in seconds with 10 digits, so its format is `ssssssssss`
    * It is valid if in the future

* If the token is not expired, use a **Set Attribute** filter to set the `oauth.token.valid` attribute to `true`. (Set it to `false` if the token is expired).

    ![Set Attribute filter](/Images/docbook/images/api_mgmt/api_mgmt_oauth_ext_setattributefilter.png)

### Set the policy for API Manager

Finally, set this policy in the API Manager settings configuration under **Server Settings > API Manager > OAuth Token Information Policy**.

## Configure the security device on the front-end API

Follow these steps in API Manager:

1. In the front-end API to be secured, select the **OAuth (External)** security device.
2. Set the **Token information policy** to the policy you created earlier.
3. Update the **Grant Type** tab with your OAuth server URLs.
4. In the application that is granted access to your API, add the `client_id` to verify under **Authentication > External credentials**.

## Test the configuration

When the configuration is complete you can request a token from your OAuth server and access your API using it, for example:

```bash
curl --header "Authorization: Bearer my.JWT.token" https://apimanager:8065/api/example
```
