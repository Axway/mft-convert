{
"title": "Integrate API management into a CI/CD pipeline",
"linkTitle": "Pipeline",
"weight":"40",
"date": "2020-04-14",
"description": "Learn how to integrate the API management solution into a pipeline."
}

These days, the speed at which you can bring new products to market plays an enormous role in your company's success. In the software environment, an integration pipeline is typically used to compile, test, and deploy the software. APIs are no exception and should be deployed in the same way, if necessary even parallel to the application that provides the API.

In addition to the speed advantage, the integration of Amplify API Management into an integration pipeline brings several benefits:

* Automated testing facilitates version management
* Less effort for the developer to manage the API
* Better feedback loop leads to a better product (better API)
* Working collaboratively is made easier

To integrate Amplify API Management, two deployment artifacts must be considered and deployed in separate pipelines:

* Policies - Policies provide security, integration, and routing functions and are developed by the policy developer. Policies are developed in a general way and are used by a variety of APIs. Policies are deployed to the corresponding API Gateways or the API Gateway group. Policy changes and the associated deployments are significantly less frequent than individual APIs.
* APIs - The APIs deployment unit is defined by the actual API specification and the configuration of how the API is to be managed on the API management system. Both can be described as API packages and are deployed individually. These packages are managed by API developers (also called producers).

Different deployment workflows are required for both approaches.

## Policy deployment

Policy developers develop policies with the help of Policy Studio, for example to realize security or integration use cases. The starting point is a checked-out policy configuration project, which is opened and edited with Policy Studio. After editing, the changes are checked back in, for example using Git. This enables integration into a CI/CD pipeline.

The policy configuration consists of:

* Policy file containing all artifacts that are the same per stage
* One or more environment files containing the configurations per stage

For deployment, the policy file and the corresponding environment file are merged to form the `fed` file, which is then deployed to the API Gateways.

The Amplify API Management solution provides predeployment scripts that you can use in an integration pipeline.

Instead of deploying policy and environment files directly from the version management system, it is best to build release packages that are stored in a release repository. This allows auditing, simple rollbacks, and the maintenance of dependencies.

A powerful framework for deploying policy configurations based on Maven is available at [Maven Plugin for API Gateway Policy Development on GitHub](https://github.com/Axway-API-Management-Plus/apigw-maven-plugin).

## API deployment

The goal is for an API developer to be able to automatically manage and deploy their own APIs in self-service mode. This means that the developer can work self-sufficiently and is not regularly dependent on other teams or persons, such as a CI/CD administrator or an API administrator.

The requirements for the process are as follows:

* API developer teams - Developers might work in different teams and not everybody should see everything. This is managed by access groups in Bitbucket or GitHub, which enable you to control access permissions for each repository.
* No Jenkins admin - By using an organization folders approach, no centralized Jenkins administration team is required to create jobs or workflows in Jenkins. After a Jenkins administrator has performed an initial setup, API developers can control Jenkins jobs on their own.
* Staging support - Even if the API developer experience should be simple and automated, in many enterprise companies approval or quality gates must be established. It must also be possible to trace back to a certain version of an API exposed.
* Segregation of responsibilities - Developers implementing an API should not be able to see or maintain configuration data for other stages, such as pre-production or production.

### Process overview

If you have a number of API developers working on different or even the same projects and repositories, they should be able to control the deployment pipeline of their APIs independently, at least in the development stage before the API package for higher stages is released.

The following diagram shows the process for the development without staging concept in a simplified way.

![Jenkins pipeline overview](/Images/api_mgmt_overview/jenkins-workflow.png)

To learn more, see [Jenkins Integration with GitHub & Bitbucket](https://github.com/Axway-API-Management-Plus/apimanager-swagger-promote/wiki/9.-Jenkins-Integration-with-GitHub-&-Bitbucket).

### Staging

It is best practice to create packages, artifacts, or some kind of releases for software projects. These artifacts are managed by repositories (for example, Nexus, JFrog Artifactory) and used to share assets between teams, and these artifacts are deployed to the target system. This provides several benefits, such as version auditing, segregation of concerns, rollbacks to a previous version, and the ability to establish CI/CD gates. It is best to use the same approach for promoting and deploying APIs and policies into all different API management stages.

The API developer or service provider expects the flexibility to code, deploy, and test frequently until they are satisfied with their code and are ready to create a release package. The following diagram shows an example process for the API developer.

![Developer process](/Images/api_mgmt_overview/dev-to-prod-process.png)  

{{< alert title="Tip" color="primary" >}}In this process it might be more efficient to use the API Manager CLI [`apim-cli`](/docs/api_mgmt_overview/api_mgmt_components/tools/#api-manager-cli).{{< /alert >}}

### Promoting to all other stages

When the process above is complete, the API release package is created ready for the next stage, which might be production or test. From this point on, the API release package is not changed, but deployed into the different stages with different configurations.  

The process in all remaining stages is different to the development stage and is illustrated as follows.

![Non-Development stages](/Images/api_mgmt_overview/prod-process.png)  

Watch this video to get an overview of how the process works.

{{< youtube 4VWxdIkCvlE >}}
