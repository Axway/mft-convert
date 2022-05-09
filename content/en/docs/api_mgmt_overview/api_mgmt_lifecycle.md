{
    "title": "API lifecycle",
    "linkTitle": "API lifecycle",
    "weight": "3",
    "date": "2020-04-14",
    "description": "An overview of the entire API lifecycle and which API management components play a role in it."
}

You must consider the entire API lifecycle from both sides, the service provider and service consumer, to get the most value from the platform.

## Self-service is the key

API management is the process of publishing, promoting, and managing Application Programming Interfaces (APIs) in a secure, scalable environment. It includes the creation of API consumer support resources that define and document APIs to facilitate easy consumption. API management supports business initiatives to enable easy interaction with customers and partners.

A well-executed API strategy helps create more selling channels, better engage with customers, and offer more value to partners. This practice of doing better business through the effective delivery of APIs enables the API economy. API management uses Web-Oriented Architecture (WOA) technologies such as REST, JSON, and OAuth instead of traditional Service-Oriented Architecture (SOA) technologies.

The role of central IT with specialists in API security should not be responsible for each API, but should develop global security policies that are utilized by each API.

It is important for service providers that the integration of their interfaces does not become an additional daily burden, but can be integrated into the existing workflow. This is the only way to ensure that APIs are integrated into the platform early enough. This means that the API management platform should be integrated by way of an automatic CI/CD pipeline for APIs.

The service consumer expects an API developer portal, which is the central entry point into your API economy. In the API developer portal, the consumer can find APIs from various service providers, both enterprise services and integrated cloud services. These should be well documented, divided into categories (business expertise, maturity level), available for self-service test and, if necessary, consumable.

The following illustration shows the complete API lifecycle, which is passed through by various personas, including the API service provider and consumer.

![Entire API-Lifecycle](/Images/api_mgmt_overview/api-lifecycle-overview.png)

## API design and prototype

The availability of large numbers of pure APIs in an API management platform does not necessarily make it a success. More important are valuable APIs, often called _value APIs_ (VAPI), which are based on a well considered API design and always have the consumption of the API and not the existing data structures in mind.

To learn more about good API design, see [The Design of Web APIs](https://www.manning.com/books/the-design-of-web-apis).

Value APIs usually start with an API-first approach and not with the implementation of an API. API-first means that the API interface, that is, the design of the API, is defined first, and this design is the starting point for further steps, such as mocking and then implementation of the API.

For API design, Axway works with Stoplight, which offers a form-based API design editor for both OpenAPI 2.0 and 3.0. Stoplight also offers enterprise features for tracking and approving changes in larger teams.

To learn more about API design with Stoplight, see [API design and prototyping with Stoplight](/docs/api_mgmt_overview/api_mgmt_components/stoplight/).

Stoplight is only one option and any API design tool can be used and integrated if it generates standard API specifications based on OpenAPI 3.0 or Swagger 2.0 or 1.2.

## Mock

When using the API-first approach it makes sense to think about how to quickly and easily mock new APIs. Mocks help to separate the API service provider from the service consumer, and give potentially interested customers an understanding of what this new API will deliver.

While service providers are implementing a service, they can provide a mock, allowing service consumers to advance the implementation of the client. In addition, mocks help to improve the feedback loop, as potential consumers can provide information about the payload to optimize it for different application purposes.

To learn more about mocking, see [mock an API in API builder](/docs/api_mgmt_overview/api_mgmt_components/apibuilder/#mock-an-api).

## Develop and integrate

The API management platform is not responsible for the actual development of APIs, as these are provided by back-end applications.
Sometimes, however, back-end APIs are too technical to provide the desired value, that is, they do not represent a value API.
In this context, the term _develop_ is used to describe, for example, the orchestration or optimization of APIs.

To implement the desired API design, you can orchestrate a series of technical APIs or cloud applications; or optimize the payload depending on the client. For example, mobile application compared with single page application. In addition, a use case in the development area is the kind of integration flow to define, which includes other systems, for example, to send a notification (Teams, Slack, email, push message, and so on) when an order is received.

To learn more about API Builder, see [Introduction to API Builder](/docs/api_mgmt_overview/api_mgmt_components/apibuilder/).

Another approach to developing or integrating into back-end legacy applications is the use of [policies](/docs/api_mgmt_overview/key_concepts/policies/). For example, this includes exposing a SOAP Web service as a REST API or implementing an OAuth client. API development based on policies is performed by a policy developer using Policy Studio. For more details on creating APIs using the REST API development wizard, see [Develop REST APIs in Policy Studio](/docs/apim_policydev/apigw_web_services/register_rest_apis/).

## Configure

In addition to the pure API specification in the form of an OpenAPI (Swagger) definition, the API management system needs to know how the API should be managed. Which security, tags, custom policies, certificates, images, custom properties, consumer quotas, access rights, and so on should be managed? All this information must be configured or prepared on the platform. An API can be configured manually using the API Manager Web UI or using an APIs as code approach where a CLI or CI/CD-Pipeline deploys the API automatically into the platform.  

Depending on the chosen approach, [manual configuration](/docs/api_mgmt_overview/api_mgmt_components/apimanager/) or [automatic deployment](/docs/api_mgmt_overview/api_mgmt_components/pipeline/), you must consider different steps or processes. Keep in mind that the API service provider, that is, the developer, should have as little effort as possible to increase the acceptance of the platform. This is the only way to ensure that APIs are registered in the platform in the early API design phase and that the Agile approach can be built with feedback loops.

## Test

API developers must test every API, however, a general recommendation is to build an appropriate test suite to ensure that the endpoints work according to the specification. These are integration tests that check each API endpoint to see if it responds with the correct response (code and response) depending on various input parameters.

This test suite is essential for lifecycle management and version management, as it allows you to detect whether changes lead to a breaking change for consuming applications or not. Changes to the APIs might be implemented after six months or one year, at which time another developer is responsible who does not know every detail - to give this developer security, this test suite is needed for each API.

To learn more about testing your API, see [Test your APIs](/docs/api_mgmt_overview/key_concepts/api_mgmt_tests).

## API staging

Customers divide systems into zones for development, pre-production, production, and so on. The API management platform is also deployed in each zone. APIs are developed and tested in the development zone and then deployed to the next zone, and so on. This means that a deployment concept is needed, which should be automated if possible to promote the APIs and policies from one stage to the next.
How the exact process is set up depends on the requirements of your company, for example, to use release artifacts or not, and which groups of people have which responsibilities.

There are two deployment artifacts in the Amplify API Management solution:

* **Policies**: Generally valid policies that are referenced by different APIs. These policies map security, routing, integration, and other use cases. They are developed by policy developers using Policy Studio and have their own deployment pipeline. They are deployed relatively rarely.
* **API**: The API is defined by the API specification and the API configuration. It is developed by service providers and deployed as a single unit.

Both deployment units must support the staging concept, since certain components (for example, passwords, host names, and so on) are different for each stage.

To learn more about how to promote APIs and policies, see [Staging](/docs/api_mgmt_overview/api_mgmt_components/pipeline/#staging).

## Pipeline-based integration

It is best to provide the API management platform, which extends over several stages, with policies and APIs using a CI/CD pipeline.
This enables an Agile approach and allows APIs to be deployed quickly, receive feedback, and feed it back into the API.
Additionally, you can extend this pipeline with additional customer-specific requirements, for example to establish governance steps, plausibility checks, and so on. Lastly, it increases the acceptance of API service providers as they can focus on developing services instead of registering APIs.

Typical systems and tools for pipeline-based integration are Jenkins, Bamboo, Maven, and possibly an artifact repository like Nexus or Artifactory.

To learn more, see [Integrate your API management solution into a CI/CD pipeline](/docs/api_mgmt_overview/api_mgmt_components/pipeline/).

## Govern and monitor

When an API is in production it is important to monitor it. Monitoring includes various aspects, such as a health check, which not only checks whether an API exists but also makes a call through to the back-end system. This is the only way to ensure that the API  works from end to end.

Another aspect is the runtime monitoring of security rules, SLAs, where the consumers come from, and the performance. You can use Amplify API Management dashboard tools for this, or you can integrate the platform into existing central monitoring cockpits.

To learn more about how to govern and monitor, see [Introduction to API Gateway](/docs/api_mgmt_overview/api_mgmt_components/apigateway/).

## Manage access and secure your APIs

When an API is registered in the API management platform, it is exposed to consumers through the API Gateway. The API Gateway acts like a reverse proxy and is able to apply different security mechanisms to the exposed API. There are standard security mechanisms such as API key, OAuth, or the connection to existing access token providers.

In addition to applying security mechanisms, the primary task is to determine the consuming application so that it can be checked accordingly. Does the application have a subscription, is the application valid, is it within the configured quota? If the application is recognized and the access is valid, further custom policies registered for the API are executed. These examine the API request in more detail, adopt an adapted routing towards the back-end, or check the final response to the consumer.

To learn more about access management and security, see [Introduction to API Manager](/docs/api_mgmt_overview/api_mgmt_components/apimanager/).

## Consume APIs in the developer portal

The API developer portal is the central contact point for internal and external API service consumers. It offers self-service interfaces for registration, research, testing, and subscribing to APIs.

In addition to the pure API functions, the developer portal should be designed to offer support options and general instructions on the API economy. For example, an explanation of how to get access tokens or an API glossary.

API consumers can also monitor the access of their own applications and manage applications.

To learn more, see [Introduction to API Portal](/docs/api_mgmt_overview/api_mgmt_components/apiportal/).

## Analytics

Long-term analyses and statistics provide information on how the platform is developing. How is the performance in the long run, which APIs are working well and which are not, are the error rates stable, and so on.

The analytics components are not intended to display the operational monitoring with a detailed view of each individual API request, but are used for evaluation and planning for the platform. Amplify API Management provides an out of the box analytics component with Embedded Analytics for API management. With open log formats, such as open logging, the solution can also be connected to existing systems such as Elasticsearch or Splunk.

To learn more about analytics options, see [API management analytics](/docs/api_mgmt_overview/api_mgmt_components/analytics/).

## API versioning

The versioning of APIs, that is, the associated lifecycle, is important. You should try to stay on a stable API (`/api/v1/resource`) for as long as possible. You should also avoid exposing the same API but with individual versions per consumer. This leads to point-to-point integrations that should be avoided as they are difficult to maintain.

Lifecycle management should be considered from the beginning and, if possible, new requirements should be integrated into the same API version as a non-breaking change. In other words, the API is subject to a natural evolution with improvements, extensions, fixes, and so on, and the process must be designed to support this evolution.

When version changes occur, Amplify API Management supports processes to move consumers from one API version (`/api/v1/res`) to the next (`/api/v2/res`).
