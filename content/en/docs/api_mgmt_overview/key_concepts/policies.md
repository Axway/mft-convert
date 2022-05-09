{
    "title": "Introduction to policies",
    "linkTitle": "Policies",
    "weight": "15",
    "date": "2020-04-15",
    "description": "Learn what a policy is and what it is used for, and understand the different types of policies."
}

## What is a policy?

A policy is a network of message filters, and each filter is a modular unit that processes a message. A message can traverse different paths through the policy, depending on which filters succeed or fail. An important aspect of a policy is the context of a policy that the filters can access (read from or write to). The context that a policy is injected with depends on the context in which the policy is used, for example, an Alert policy compared with an API Manager request policy. A policy can also contain other policies, so you can build modular, reusable policies.

![Sample Policy in Policy-Studio](/Images/api_mgmt_overview/sample-policy.png)

Amplify API Management provides a number of built-in policies that you can apply to APIs. In addition, policy developers can use Policy Studio, a graphical tool, to develop custom policies.

In Policy Studio, a policy is assembled by selecting filters from the filter palette on the right and dragging and dropping them onto the policy canvas to be configured. The configured filters are then connected to a policy using success and failure paths to trace a path through a set of filters and create sophisticated rules. Some filters require configuring additional resources or settings before the filters can be used. You can find these additional resources and settings from the node tree on the left.

To get started with policies, see [Get started with Policy Studio](/docs/apim_policydev/apigw_poldev/gs_concepts/).

### Security policies

You can use policies to enhance security. The support for a wide range of security standards enables identity mediation between different identity schemes.

Data is routed based on sender identity, content, and type. This means that messages are sent to the appropriate application in a secure manner. It also enables service virtualization, where services are exposed to clients with virtual addresses to mask their actual addresses and shield endpoint services from direct access for added security.

Data monitoring, redaction, encryption, and signing facilitates privacy compliance support. For example, you can encrypt sensitive information, such as customer names, or strip that information out of message traffic.

For identity management, you can configure different kinds of authentication policies in Policy Studio, and integrate with existing third-party Identity Management (IM) infrastructures for authentication and authorization.

To learn more about security topics, see the following sections:

* [Integrate with Identity Management servers](/docs/apigtw_auth_auth/)
* [Integrate with Kerberos](/docs/apigtw_kerberos/)
* [Security guidance](https://docs.axway.com/bundle/apim-security-guide/page/api_management_security_guide.html)
* [Configure OAuth](/docs/apim_policydev/apigw_oauth/)

### API Manager policies

APIs are registered in the API Manager and configured according to how they are to be processed. This includes, for example, inbound and outbound security, quotas, and so on. However, it is sometimes important to use custom policies to enhance processing or to be able to control it. For example, to check special request headers, write them to log files, and so on. Policies can also be developed for this purpose, which are made available to API manager as policy callbacks.

The following diagram illustrates how an API request is processed at runtime by the API Gateway and at which time policies can be used.

![Request processing](/Images/api_mgmt_overview/api-manager-request-processing.png)

#### Example: Prioritization of APIs

Suppose you want to prioritize APIs differently at peak times, so that lower prioritized APIs will be processed secondary to ensure that enough capacity is available for the most important requests. You can use API Manager policies for this use case.

As explained earlier, each policy gets context injected and can use it to make decisions, so the context controls the execution of the policy. For this use case you need context information that tells the policy about the prioritization of the API, and you can configure and use a [custom attribute](/docs/apim_administration/apimgr_admin/api_mgmt_custom/#add-a-custom-property-to-apis) `Prioritization` in API Manager. This allows the API service providers to define for their APIs whether it is high, medium, or low priority. This information becomes available as part of the context to the runtime policies and they can use it to control the execution.

![Custom property prioritization](/Images/api_mgmt_overview/api-manager-custom-prop-prio.png)

For example, you can configure the following policy in Policy Studio. This policy is either added to an existing corporate global request policy using a [policy shortcut](/docs/apim_policydev/apigw_polref/utility_additional/#policy-shortcut-filter) or configured as a dedicated request policy.

![Handle prioritization policy](/Images/api_mgmt_overview/handle-prioritization-policy.png)

This policy starts with a general [throttling action](/docs/apim_policydev/apigw_polref/content_max_messages/) every API must pass. If the configured throttling threshold value is reached, the policy follows the red path and the API is checked to see if it is a high priority API. If it is high priority, it is processed immediately. If not, processing is [paused](/docs/apim_policydev/apigw_polref/utility_additional/#pause-processing-filter) (for example, for 200ms). Next, the system checks whether the API is a medium priority API, and so on. The pause times can be obtained from the context or loaded dynamically from the [KPS](/docs/apim_policydev/apigw_kps/) to be able to react on actual requirements.

#### Supported API Manager policies

| Type                           | Description |
|--------------------------------|-------------|
| Global Request-Policies        | Global request policies are set by administrators and each API must pass through the configured policy. Practical examples are corporate security policies that are not individual per API. |
| Request-Policies               | Policy developers can make a set of request policies available to the API developers so that they can be assigned to the individual APIs. The API developer can decide if and which policy is assigned. |
| Routing-Policies               | For each API registered in the API Manager, it is defined which backend host is to be addressed in order to operate the service. By default, the API gateway reacts like a normal proxy and will address the HTTP(S) backend host. Including configured outbound configuration. If you need to control this routing specifically, use routing policies. |
| Response-Policies              | Response policies are similar to request policies, but are executed when the response is received from the backend server. Further checks can be performed with the response policy. |
| Global Response-Policies       | Global response policies also work in the same way as global request policies and, if set by an API administrator, are valid for every API in the system. |
| Token-Information Policies     | If access tokens (e.g. JWT) must be validated by external token providers for access to an API, this work is performed by a token information policy. This will either use the token inspection point for validation or check the token itself using signature, validity, audience, and so on, and report the result back to the runtime. |
| Inbound-Security Policies      | In addition to standard inbound security devices, such as OAuth, API key, etc., you can also use a custom inbound authentication policy. This policy has the task of determining the application using request parameters (header, payload) and can also perform other tasks, such as user authentication/authorization. |
| Fault-Handler Policies         | If an error occurs during the processing of an API, for example if the backend is not reachable, you can use a fault-handler policy to handle different error classes, so that customized error messages can be returned to the client. |
| Alerts                         | Besides standard notifications via email to API developers and API consumers, there is sometimes the need to integrate the API management solution into existing processes. For example, based on events in the API management system, start workflows in a ticket system that can be used to process a problem. Alert policies that can be configured for a whole range of events are suitable for this. |
| API-Promotion                  | One way to promote APIs from one stage to the next is the promotion policy, which can be triggered via the API Manager Web UI for a selected API. This policy gets the current API configuration from input as JSON payload, can manipulate it and then pass it on accordingly. Either directly to the higher stage, open a ticket with the API as attachment or send information via email. |

Learn more about [how to set up API Manager policies](/docs/apim_administration/apimgr_admin/api_mgmt_config_ps/#global-request-policies) and [how to set up alerts](/docs/apim_administration/apimgr_admin/api_mgmt_alerts/).

### Integration policies

In Amplify API Management, API Gateway provides integration across systems and compatibility. REST-SOAP conversion enables you to make enterprise application data and operations available to mobile apps. You can convert a legacy SOAP service, and deploy it as a REST API. API Gateway then can expose the REST API that maps to the SOAP service, dynamically creating a SOAP request based on the REST API call.

For more details, see [How to configure external systems from API Gateway](/docs/apim_policydev/apigw_external_connections/).

### Native policies

Unlike API Manager policies, which are called by the API Gateway in the context of an API Manager API, you can also use native API Gateway policies. These are typically exposed on other list sockets and can be used for all possible use cases. For example, to expose an internal service API that performs maintenance tasks and is called at the end of a workflow in the ticketing system, an API is published. Furthermore, such native policies can also be executed as scheduled policies.

The simplest example of a native policy is the Health Check API: `http://api-host:8080/healthcheck`.

Learn more about [how to configure a policy](/docs/apim_policydev/apigw_poldev/general_manual_policy/) in API Gateway.