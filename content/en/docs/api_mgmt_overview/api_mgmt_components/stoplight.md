{
"title": "API design and prototyping with Stoplight",
"linkTitle": "Stoplight",
"weight":"5",
"date": "2020-04-14",
"description": "API-first design with Amplify API Management and Stoplight."
}

For API design, Axway and [Stoplight](https://stoplight.io) work in partnership. This page gives an overview of the possibilities Stoplight offers and how it can be integrated into your Amplify API Management platform. Following the API-first approach, it is important to have a powerful API design tool that can be used by non-technical people to develop OpenAPI 2.0 / 3.0 API definitions.

## Stoplight Studio

Stoplight Studio supports the following:

* Form-based or code-based OpenAPI 3.0 and 2.0 editor.
* Available as rich client (Windows, Mac, and Linux) or Web-based client.
* Fully based on Git, easy integration into GitHub, GitLab, Bitbucket, and so on.
* API design, file, and documentation view.
* Integrated mock service based on [Prism](https://stoplight.io/mocking).

![Stoplight Studio](/Images/api_mgmt_overview/stoplight_studio.png)

To get a full overview about what Stoplight Studio provides, see [Stoplight API Design](https://stoplight.io/design/).

Watch this video to learn how to use Stoplight:

{{< youtube 7olnV8rR1xc >}}

## Integrate into the Amplify API Management platform

Stoplight creates standard OpenAPI (Swagger) files under the hood, which you can directly transfer to the API management platform. You can import the OpenAPI specification manually using the API Manager web UI, or using [`apim-cli`](/docs/api_mgmt_overview/api_mgmt_components/tools/#api-manager-cli) directly, or a CI/CD pipeline. Direct integration using `apim-cli` is preferable because it makes the process fast, easy, and repeatable. In addition, Stoplight Studio works on the basis of a checked out Git repository, which allows a fully automatic coupling with a CI/CD system.

The following diagram shows simplified integration between Stoplight and the AMPLIfY API Management solution:

![Stoplight and API management integration](/Images/api_mgmt_overview/stoplight-integration-overview.png)

The following video demonstrates the possible integration and how the process looks like:

{{< youtube jKfpCE64vHc >}}

Learn more about [how to integrate API management into a CI/CD pipeline](/docs/api_mgmt_overview/api_mgmt_components/pipeline/).

In principle, this type of integration is possible with any API design tool that generates standard API specifications (OpenAPI or Swagger) and is ideally based on a Git repository.
