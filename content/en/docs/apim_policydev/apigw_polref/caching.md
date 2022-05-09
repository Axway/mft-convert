{
"title": "Caching filters",
  "linkTitle": "Caching filters",
  "weight": 75,
  "date": "2019-10-17",
  "description": "Filters to cache parts of a message, and work with cached attributes."
}
To learn more about caching, and for an example of a caching policy, see [Configure caching](/docs/apim_policydev/apigw_poldev/general_cache/).

## Cache attribute filter

The **Cache Attribute** filter allows you to configure what part of the message to cache. Typically, response messages are cached and so this filter is usually configured *after* the routing filters in a policy. In this case, the `content.body` attribute stores the response message body from the web service and so this message attribute should be selected in the **Attribute Name to Store** field.

Configure the following fields:

**Name**: Enter a name for this filter to display in a policy.

**Select Cache to Use**: Click the button on the right, and select the cache to store the attribute value. The list of currently configured caches is displayed in the tree. To add a cache, right-click the **Caches** tree node, and select **Add Local Cache** or **Add Distributed Cache**. Alternatively, you can configure caches under the **Environment Configuration** > **Libraries** node in the Policy Studio tree.

**Attribute Key**: The value of the message attribute entered here acts as the key into the cache. In the context of a caching policy, it *must* be the same as the attribute specified in the **Attribute containing key** field on the **Is Cached?** filter.

**Attribute Name to Store**: The value of the API Gateway message attribute entered here is cached in the cache specified in the **Cache to use** field above.

## Is cached filter

The **Is Cached?** filter looks up a named cache to see if a specified message attribute has already been cached. A message attribute (usually `message.key`) is used as the key to search for in the cache. If the lookup succeeds, the retrieved value overrides a specified message attribute, which is usually the `content.body` attribute.

For example, if a response message for a particular request has already been cached, the response message overrides the request message body so that it can be returned to the client using the **Reflect** filter.

Configure the following fields:

**Name**: Enter a suitable name for this filter to display in a policy.

**Select Cache to Use**: Click the button on the right, and select the cache to lookup to find the attribute specified in the **Attribute containing key** field below. The list of currently configured caches is displayed in the tree. To add a cache, right-click the **Caches** tree node, and select **Add Local Cache** or **Add Distributed Cache**. Alternatively, you can configure caches under the **Environment Configuration** > **Libraries** node in the Policy Studio tree.

**Attribute containing key**: The message attribute entered here is used as the key to lookup in the cache. In the context of a caching policy, the attribute entered here *must* be the same as the attribute specified in the **Attribute key** field on the **Cache Attribute** filter.

**Overwrite Attribute Name if Found**: Usually the `content.body` is selected here so that value retrieved from the cache (which is usually a response message) overrides the request `content.body` with the cached response, which can then be returned to the client using the **Reflect** filter.

## Create key filter

The **Create Key** filter is used to identify the part of the message that determines whether a message is unique. For example, you can use the request message body to determine uniqueness so that if two successive identical message bodies are received by the API Gateway, the response for the second request is taken from the cache.

You can also use other parts of the request to determine uniqueness (for example, HTTP headers, client IP address, client SSL certificate, and so on). This means that you can use the **Create Key** filter to create keys for a range of different caching scenarios, for example, caching a user's role, or caching a session for a user.

Configure the following fields:

**Name**: Enter a suitable name for this filter.

**Attribute Name**: Select or enter the name of the message attribute to use to determine whether an incoming request is unique or not. For example, if `http.request.clientcert` (the client SSL certificate) is selected, the API Gateway takes a cached response for successive requests in which the client SSL certificate is the same. Defaults to `content.body`.

**Output attribute name**: Select or enter the name of the output message attribute to be used as the key for objects in the cache. Defaults to `message.key`. This attribute contains a hash of the request message, which can then be used as the key for objects in the cache.

## Remove cached attribute filter

The **Remove Cached Attribute** filter allows you to delete a message attribute value that has been stored in a cache. Each cache is essentially a map of name-value pairs, where each value is keyed on a particular message attribute. For example, you can store a cache of request messages according to their message ID. In this case the message's `id` attribute is the key into the cache, which stores the value of the request message's `content.body` message attribute.

In this example, the **Remove Cached Attribute** filter can be used to remove a particular entry from the cache based on the runtime value of a particular message attribute. By specifying the `id` message attribute to remove, the API Gateway looks up the cache based on the value of the `id` message attribute. When it finds a matching message ID in the cache, it removes the corresponding entry from the cache.

The example described above might be useful in cases where a request message needs to be cached and stored until the request is fully processed and a response returned to the client. For example, if the request must be routed on to a back-end web service, but that web service is temporarily unavailable, you can configure the policy to resend the cached request instead of forcing the client to retry.

Configure the following fields:

**Name**: Enter a name for this filter to display in a policy.

**Select Cache to Use**: Click the button on the right, and select the cache that contains the cached values that have been keyed according to the message attribute specified below. The list of currently configured caches is displayed in the tree. To add a cache, right-click the **Caches** tree node, and select **Add Local Cache** or **Add Distributed Cache**. Alternatively, you can configure caches under the **Environment Configuration** > **Libraries** node in the Policy Studio tree.

**Attribute Key**: Enter the message attribute that is used as the key into the cache in this field. At runtime, the API Gateway populates the value of this message attribute, which is then used to lookup the cache selected in the table above. If a match is found in the cache, the corresponding entry is deleted from the cache.