{
"title": "Throttling filter",
  "linkTitle": "Throttling filter",
  "weight": 59,
  "date": "2019-10-17",
  "description": "Use the Throttling filter to rate limit calls to a back-end service."
}

The **Throttling** filter enables you to limit the number of requests that pass through an API Gateway in a specified time period. This enables you to enforce a specified message quota or *rate limit* on a client application, and to protect a back-end service from message flooding.

{{< alert title="Note" color="primary" >}}This filter requires a [Key Property Store (KPS)](/docs/apim_policydev/apigw_kps/get_started/) table, which can be, for example, an API Manager KPS table.{{< /alert >}}

You can configure this filter, for example, to allow only a specified number of messages from a specified client over a configured time period through to a virtualized API. If the number of messages exceeds the specified limit, the filter fails for the excess messages.

When the configured constraints are breached, the API Gateway behavior is determined by the filter that is next in the policy failure path from the **Throttling** filter. Typically, an **Alert**, **Trace**, or **Log** filter is configured as the successor filter in the failure path.

Some example use cases for the **Throttling** filter include:

* Protect a back-end service that can handle a maximum of only 20 messages per client per second.
* Enforce a specific message rate limit or quota where a customer has purchased a maximum of 100 messages per client per hour only.

{{< alert title="Note" color="primary" >}}API Gateway version 7.6.0 introduced the **Smooth Rate Limiting** algorithm for improved handling of high traffic levels. The existing algorithm has been renamed as **Floating Time Window**, and is still provided for backwards compatibility with previous API Gateway versions. Existing configuration from previous API Gateway versions is preserved during upgrade and executes as before.{{< /alert >}}

## Rate limit algorithm options

You can select the following options from the rate limit algorithm drop-down list:

* **Smooth Rate Limiting**: This algorithm smooths out the traffic by dividing per second limits into regular millisecond intervals. For example, a setting of 500 requests per second results in 1 request being accepted every 2 milliseconds. It provides most protection to back-end servers, and is especially suitable for back-end server throttling in elastic environments, but can also be used in on-premise deployments.

    The Smooth Rate Limiting algorithm distributes rate limits among running API Gateways evenly (round robin) or dynamically (based on past traffic) to match your load balancing strategy. It also keeps track of the number of running API Gateways and dynamically updates the limits for each API Gateway when there is a change in the number of running API Gateways.

    The smooth rate limits are stored in an Apache Cassandra database. If you want to use this algorithm, you must first configure an Apache Cassandra connection.

* **Floating Time Window**: This algorithm is provided for backwards compatibility with previous API Gateway versions. It does not include any traffic smoothing, and is suitable for lower traffic levels, over longer time intervals (for example, 10 transactions per minute or 100 transactions per hour). This means that if a rate limit is set to 100 requests per minute, all 100 can arrive in the first 10 seconds, and will be served. But any requests in next 50 seconds will be rejected. Floating time window is the default algorithm. Its rate limits are stored in a local or distributed cache.

### Smooth rate limiting algorithm settings

The following settings are available when you select **Smooth Rate Limiting** algorithm:

**Name**:

Enter a name for this filter to display in a policy.

**Allow**:

Enter the number of messages to allow in a one second time interval.

**Strategy**:

Select a rate limiting strategy.

* **Evenly distributed**: Select this option to evenly distribute the configured rate limit across all API Gateway instances. This is the default option, and is most suitable when your API Gateway load is handled by a round-robin load balancer.

    For example, if you configure a rate limit of 100 transactions per second, and have 5 API Gateway instances, 20 transactions are allocated to each API Gateway instance. During the specified interval, 1 second, if an API Gateway instance receives more than 20 transactions, the excess transactions will be lost. In addition, if an API Gateway instance is added or removed, a recalculation of the distribution is performed.
* **Dynamically distributed**: Select this option to dynamically distribute the configured rate limit across all API Gateway instances based on the recent load experienced by each node. This is more suitable when load distribution is based on geography.

    On startup, the rate limit is distributed equally among the active API Gateways. Over time, the rate limit distribution reflects the load experienced by each API Gateway in the previous refresh interval. In addition, if the number of API Gateways has increased (scaled out) or decreased (scaled in), the rate limit for each API Gateway is recalculated accordingly.

**Burst size**:

Enter the number of requests in the configured time period to allow as a burst to handle spikes in incoming message traffic. The burst size defines how many requests that a client can make in excess of the specified rate limit at the millisecond level. Defaults to `0` requests.

For example, a configured rate limit of 500 transactions per second means 1 request per 2 milliseconds, so 2 milliseconds is the burst zone. If the burst size is 0, and 2 requests are received in the 2 millisecond zone, one request is processed, and all other requests are rejected. However, if you set the burst size to 10, the following occurs:

* First 2 ms: 11 requests received—1 is allowed as per rate limit, and another 10 are allowed per burst size. Now the burst count is full at 10 (no more space).
* Second 2 ms: 2 requests received—1 is allowed per rate limit, but the second is rejected because the burst queue is full.
* Third 2 ms: No requests received—1 is reduced from the burst queue, and it has 9 requests.
* Fourth 2 ms: 2 requests received—1 is allowed per rate limit, and 1 is added to the burst queue of 9, so now it is full.
* Fifth 2 ms: No requests received—burst queue decreased to 9, and so on.

API Gateway keeps count of how many requests in excess of the configured rate limit have been sent in a bursts queue.

{{< alert title="Note" color="primary" >}}The smooth rate limiting algorithm uses Apache Cassandra for rate limit storage. You must configure the Cassandra settings under the **Server Settings > Cassandra > Throttling** node in Policy Studio before using the smooth rate limiting.{{< /alert >}}

### Floating time window algorithm settings

The following settings are available when you select **Floating Time Window** algorithm:

**Name**:

Enter a name for this filter to display in a policy.

**Allow**:

Enter the number of messages to allow in the time interval specified in the adjoining **Messages every** field. If the API Gateway receives more than the specified number of messages during the specified time interval, the filter fails. Otherwise, the filter passes.

**Messages every**:

Enter the allowed time interval. If the API Gateway receives more than the number of messages in the **Allow** field in the time interval specified, the filter fails. The time interval depends on the units selected (**seconds**, **minutes**, **hours**, **days**, or **weeks**). Defaults to **seconds**.

The specified time period starts when a message is received. When the time period is over, the message count is reset, and the counter starts again when another message is received.

**Store rate limits in**:

Select a cache for rate limit storage. You can configure a local cache or a distributed cache:

* *Local cache*: Use when you have a single API Gateway deployed. The **Throttling**   filter uses a preconfigured local cache called **Local maximum messages**   by default. To configure a different local cache, click the button on the right.
* *Distributed cache*: Use when you have multiple API Gateways deployed for load balancing purposes, and you want to maintain a single count of all messages processed by all API Gateway instances.

**Rate limit based on**:

Select this setting to configure API Gateway to keep track of request messages based on a specified key value (for example, to track rates based on client IP address). All rate limits are stored in a cache, and a key is required to look up the cache. The key can be any value that identifies the message sender. For example, this includes the authenticated subject, the client IP address, or even a combination of API method name and IP address. If you do not specify a key, the cache contains a single entry for the filter, and the associated rate limit is used each time the filter is invoked.

For example, to track rate limits based on the client IP address, use the following default setting:

```
${http.request.clientaddr.getAddress()}
```

The value entered can also be a combination of a fixed string value and an API Gateway message attribute selector. For example, use the following value to keep track of the number of times an API named `StockQuote` is requested by an authenticated subject:

```
StockQuote-${authentication.subject.id}
```

#### When do days and weeks start?

**A day starts on the following hour**:

You must configure this field if you select `Day` from the list when configuring the **messages every** field. For example, if you select `Day`, and enter `00:00` in this field, this means that only the specified number of messages can be received in a one day period starting from midnight tonight until midnight the next day.

**A week starts on the following day**:

You must configure this field if you select `Week` from the list when configuring the **messages every** field. For example, if you select `Week` and `00:00`, and enter `Sunday` in this field, this means that the time period starts next Sunday at midnight, and lasts for one week exactly. The time period is reset on midnight of the next Sunday.

## Rate limit HTTP headers settings

The following settings apply when the **Smooth Rate Limiting** or **Floating Time Window** algorithms are selected:

**Include remaining limit in HTTP response headers**:

Specifies whether to include the following `X-Rate-Limit` headers in the HTTP response message:

* `X-Rate-Limit-Limit`: Rate limit ceiling for the given request (for example, `100` messages).
* `X-Rate-Limit-Remaining`: Number of requests left for the time window (for example, `45` messages).
* `X-Rate-Limit-Reset`: Remaining time window before the rate limit resets in UTC epoch seconds (for example, `1353517297`).

**Begin rate limit HTTP Headers with**:

Specifies the string used to begin the name of rate limit HTTP headers. Defaults to `X-Rate-Limit-`. For example, if you specify a value of `My-Corp-Quota-`, the following HTTP headers are inserted into the response message:

* `My-Corp-Quota-Limit`
* `My-Corp-Quota-Remaining`
* `My-Corp-Quota-Reset`

## Multiple throttling filters

Using multiple **Throttling** filters depends on whether you have selected the **Smooth Rate Limiting** or **Floating Time Window** algorithm.

### Smooth rate limiting

When **Smooth Rate Limiting** is selected, you must use only one **Throttling** filter for each back-end to be protected. For example, if you configure two filters for the same back-end, the final rate limit is an aggregate of the limits set in both filters.

### Floating time window

When the **Floating Time Window** algorithm is selected, to use two or more **Throttling** filters to maintain separate message counts, you must use a different rate limit value for each filter, or use different caches for each filter.

#### Use a different rate limit per filter

With this approach, you can use a unique **Rate limit based on** value in each **Throttling** filter. The easiest way to do this is to prepend the `${http.request.clientaddr.getAddress()}` selector value with the filter name, for example:

```
Acme Corp Quota Filter ${http.request.clientaddr.getAddress()}
```

This ensures that each filter maintains its own separate message count in the selected cache.

#### Use a unique cache per filter

Alternatively, you can use a unique cache to store the message count of each **Throttling** filter. With this solution, you must configure a separate cache for each **Throttling** filter that you have configured throughout *all* policies running on the API Gateway.
