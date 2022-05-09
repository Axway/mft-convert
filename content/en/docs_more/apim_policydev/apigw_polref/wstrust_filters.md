{
"title": "WS-Trust filters",
  "linkTitle": "WS-Trust filters",
  "weight": 101,
  "date": "2019-10-17",
  "description": "Create or consume various types of WS-Trust messages."
}

## Create WS-Trust message filter

You can configure the API Gateway to create various types of WS-Trust messages. The API Gateway can act both as a WS-Trust client when generating a`RequestSecurityToken` (RST) message. It can also act as a WS-Trust service, or Security Token Service (STS), when generating the following messages:

* `RequestSecurityTokenResponse`   (RSTR)
* `RequestSecurityTokenResponseCollection`   (RSTRC)

A token requestor generates an RST message and sends it to the STS, which generates the required token and returns it in an RSTR message. If several tokens are required, the requestor can send up multiple RST messages in a single `RequestSecurityTokenCollection` (RSTC) request. The STS generates an RSTR for each RST in the RSTC message and returns them all in batch mode in an RSTRC message.

For more details on the various types of WS-Trust messages and their semantics and format, see the WS-Trust specification.

Configure the fields in the following sections.

### Create message types

The **Create WS-Trust** filter can create the following types of WS-Trust message. Select the appropriate message type based on your requirements:

* **RST:RequestSecurityToken**:   The RST message contains a request for a single token to be issued by the STS.
* **RSTR:RequestSecurityTokenResponse**:   The RSTR message is sent in response to an RST message from a token requestor. It contains the token issued by the STS.
* **RSTRC:RequestSecurityTokenResponseCollection**:   The RSTRC message contains an RSTR (containing a single issued token) for each RST that was received in an RSTC message.

### General message creation settings

The settings on this tab specify characteristics of the WS-Trust message. The following fields are available:

**Insert Token Type**: Select the type of token requested from the list. The type of token selected here is returned in the response from the STS. By default, the Security Token Context type is used, which is identified by the URI `http://schemas.xmlsoap.org/ws/2005/02/sc/sct`.

**Binary Exchange**: You can use a `<BinaryExchange>` when negotiating a secure channel that involves the transfer of binary blobs as part of another security negotiation protocol (for example, SPNEGO). The contents of the blob are always Base64-encoded to ensure safe transmission.

Select the **Binary Exchange** option to use a negotiation-type protocol for the exchange of keys, such as SPNEGO. The URI selected in the **Value Type** field identifies the type of the negotiation in which the blob is used. The URI is placed in the `ValueType` attribute of the `<BinaryExchange>` element.

**Entropy**: The client can provide its own key material (entropy) that the token issuer may use when generating the token. The issuer can use this entropy as the key itself, it can derive another key from this entropy, or it can choose to ignore the entropy provided by the client altogether in favor of generating its own entropy.

Select this option to generate some entropy, which is included in the `<wst:entropy>` element of the `<wst:RequestSecurityToken>` block.

**Insert Key Size**: The client can request the key size (in number of bits) required in a`<RequestSecurityToken>` request. However, the WS-Trust token issuer does not have to use the requested key size. It is merely intended as an indication of the strength of security required.The default request key size is 256 bits.

**Insert Lifetime**: Select this option to insert a `<Lifetime>` element into the WS-Trust message. Use the associated fields to specify when the message expires. The lifetime of the WS-Trust message is expressed in terms of `<Created>` and `<Expires>` elements.

**Lifetime Format**: The specified date and time pattern string determines the format of the`<Created>` and `<Expires>` elements. The default format is `yyyy-MM-dd'T'HH:mm:ss.SSS'Z'` ,which can be altered if necessary. For more details on how to use this format, see the Javadoc for the `java.text.SimpleDateFormat` Java class in the Java API specification.

**Insert RequestedTokenCancelled**: Select this option to insert a `<RequestedTokenCancelled>` element into the generated WS-Trust message.

### RST creation settings

The following configuration fields specify the way in which a WS-Trust RST message is created:

**Insert Request Type**: You can create two types of RST message. Select one of the following request types from the list:

* **Issue**: RST message used to request the STS to *issue*   a token for the requestor.
* **Cancel**: RST message used to cancel a specific token.

**Insert Key Type**: Select this option to insert the key type into the RST WS-Trust message.

**Insert Computed Key Algorithm**: Select this option to insert the computed key algorithm into the message.

**Insert Endpoint Reference**: Select this option and enter a suitable endpoint to include an endpoint reference in the RST message.

### RSTR creation settings

The following configuration fields determine the way in which a WS-Trust RSTR message is created:

**Insert RequestedProofToken**: Select this check box to insert a `<RequestedProofToken>` element into the generated WS-Trust message. The type of this token can be set to either`computedKey` or `encryptedKey` using the associated list.

**Insert Authenticator**: Select this option to insert an authenticator into the RSTR message.

### Create advanced settings

This section enables you to configure certain advanced aspects of the SOAP message that is sent to the WS-Trust service.

**WS-Trust Namespace**: Enter the WS-Trust namespace to bind all WS-Trust elements to in this field. The default namespace is `http://schemas.xmlsoap.org/ws/2005/02/trust` .

**WS-Addressing Namespace**: Select the WS-Addressing namespace version to use in all created WS-Trust messages.

**WS-Policy Namespace**: Select the appropriate WS-Policy namespace from the list. The selected version selected can affect the ordering of tokens that are inserted into the WS-Security header of the SOAP message.

**SOAP Version**: Select the SOAP version to use when creating the WS-Trust message.

**Overwrite SOAP Method**: Select this option if the WS-Trust token should overwrite the SOAP method in the request. In this case, the token appears as a direct child of the SOAP Body element. Use this option to preserve the contents of the SOAP Header, if present.

**Overwrite SOAP Envelope**: Select this option if the generated WS-Trust message should form the entire contents of the message. In other words, the generated WS-Trust message replaces the original SOAP request.

**Content-Type**: Specify the HTTP content-type of the WS-Trust message. For example, for Microsoft Windows Communication Foundation (WCF), you should use `application/soap+xml` .

**Generate Authenticator Using**: You can verify the authenticator using the **Generated** or **Consumed** message. In either case, select the appropriate type of WS-Trust message from the available options.

## Consume WS-Trust message filter

You can configure the API Gateway to consume various types of WS-Trust messages. For example, `RequestSecurityToken` (RST), `RequestSecurityTokenResponse` (RSTR), and `RequestSecurityTokenResponseCollection` (RSTRC).

For more details on WS-Trust messages and their semantics and format, see the WS-Trust specification.

Configure the fields in the following sections.

### Consume message types

The API Gateway can consume the following types of WS-Trust messages.Select the appropriate message type based on your requirements:

* **RST:RequestSecurityToken**:   The RST message contains a request for a single token to be issued by the Security Token Service (STS).
* **RSTR:RequestSecurityTokenResponse**:   The RSTR message is sent in response to an RST message from a token requestor. It contains the token issued by the STS.
* **RSTRC:RequestSecurityTokenResponseCollection**:   The RSTRC message contains an RSTR (containing a single issued token) for each RST that was received in an RSTC message.

### Message consumption settings

The configuration options available on the **Message Consumption** tab enable you to extract various parts of the WS-Trust message and store them in message attributes for use in subsequent filters.

**Extract Token**: Extracts a `<RequestedSecurityToken>` from theWS-Trust message and stores it in a message attribute. Select the expected value of the `<TokenType>` element in the `<RequestSecurityToken>` block. The default URI is `http://schemas.xmlsoap.org/ws/2005/02/sc/sct`.

**Extract BinaryExchange**: Extracts a `<BinaryExchange>` token from the message and stores it in a message attribute. Select the `ValueType` of the token from the list.

**Extract Entropy**: The client can provide its own key material (entropy) that the token issuer may use when generating the token. The issuer can use this entropy as the key itself, it can derive another key from this entropy, or it can choose to ignore the entropy provided by the client altogether in favor of generating its own entropy.

**Extract RequestedProofToken**: Select this option to extract a `<RequestedProofToken>` from the WS-Trust message and store it in a message attribute for later use. You must select the type of the token (`encryptedKey` or `computedKey`) from the list.

**Extract CancelTarget**: You can select this option to extract a `<CancelTarget>` block from the WS-Trust message and store it in a message attribute.

**Extract RequestedTokenCancelled**: You can select this option to extract a `<RequestedTokenCancelled>` block from the WS-Trust message and store it in a message attribute.

**Match Context ID** :
Select this option to correlate the response message from the STS with a specific request message. The `Context` attribute on the `RequestSecurityTokenResponse` message is compared to the value of the `ws.trust.context.id` message attribute, which contains the context ID of the current token request.

**Extract Lifetime**: Select this option to remove the `<Lifetime>` elements from the WS-Trust token.

**Extract Authenticator**: Select this option to extract the `<Authenticator>` from the WS-Trust token and store it in a message attribute.

### Consume advanced settings

The following fields can be configured on the **Advanced** tab:**WS-Trust Namespace**: Enter the WS-Trust namespace that you expect all WS-Trust elements to be bound to in tokens that are consumed by this filter. The default namespace is `http://schemas.xmlsoap.org/ws/2005/02/trust`.

**Cache Security Context Session Key**: Click the browse button, and select the cache to store the security context session key. The session key (the value of the `security.context.session.key` attribute), is cached using the value of the `security.context.token.unattached.id` message attribute as the key into the cache.

You can select a cache from the list of currently configured caches in the tree. To add a cache, right-click the **Caches** tree node, and select **Add Local Cache** or **Add Distributed Cache**. Alternatively, you can configure caches under the **Libraries** node in the Policy Studio tree.

**Lifetime of ComputedKey**: The settings in this section enable you to add a time stamp to the extracted `computedKey` using the values specified in the `<Lifetime>` element. This section is enabled only after selecting the **Extract RequestedProofToken** check box above, selecting the `computedKey` option from the associated list, and finally by selecting **Extract Lifetime**. Configure the following fields in this section:

* **Add Lifetime to ComputedKey**:   Adds the `<Lifetime>`   details to the`security.context.session.key`   message attribute.This enables you to check the validity of the key every time it is used against the details in the `<Lifetime>`   element.
* **Format of Timestamp**:   Specify the format of the time stamp using the Java date and time pattern settings.
* **Timezone**:   Select the appropriate time zone from the list.
* **Drift**:   To allow for differences in the clock times on the machine on which the WS-Trust token was generated and the machine running the API Gateway, enter a drift time. This allows for differences in the clock times on these machines and is used when validating the time stamp on the `computedKey`.

**Verify Authenticator Using**: You can verify the authenticator using either the **Generated** or **Consumed** message. In either case select the appropriate type of WS-Trust message from the available options.
