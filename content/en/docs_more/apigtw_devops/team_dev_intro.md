{
"title": "Team development overview",
"linkTitle": "Team development overview",
"weight":"60",
"date": "2019-11-19",
"description": "Understand team development concepts and features."
}

 This feature enables a team of API Gateway policy developers to work in parallel developing APIs and policies to be deployed as a single API Gateway configuration using a Source Code Management (SCM) system.

In addition, team development enables continuous integration (CI) and continuous delivery (CD) practices to be used in an API Gateway system. This enables the API Gateway to use best practices for development, deployment, and promotion, and to support the increasing use of DevOps tooling.

The following diagram shows the different stages of delivery, from development through to deployment.

![DevOps delivery pipeline](/Images/docbook/images/promotion/devops_delivery.png)

The following details the various tasks that you might perform at each stage.

**Develop**:

* Team of developers develop API and common projects in parallel on local development environments.
* Run unit tests on projects prior to SCM check in

**Version**:

* Manage and version projects in SCM
* Diff projects to identify conflicts
* Audit who changed what when
* Control developer access to projects
* Rollback changes

**Build**:

* Build script (`projpack`) builds API Gateway configuration by merging API and common projects
* Creates the policy package (`.pol`) containing all the projects

**Test**:

* Deployment script (`projdeploy`) deploys configuration to test environment to execute system tests
* Deploys policy and environment packages for test environment
* Run full test suites on entire configuration to detect regressions

**Release**:

* Manage and version configuration in package management repository
* Policy and environment packages by version and environment

**Deploy**:

* Deploy configuration from package management repository to target environments
* Extract and deploy policy and environment packages for specific configuration version and environment

To enable team development project templates and project dependencies in API Gateway, you must turn on the team development option in Policy Studio preferences. In Policy Studio select **Window > Preferences > Team Development** and select **Enable Team Development**.

## API Gateway projects

API Gateway provides a project-based approach to developing APIs, policies, and associated resources. API Gateway configuration is split into independent projects:

* API projects – Contain resources that are specific to an API and not shared with other APIs. Your configuration can contain multiple API projects.
* Common project – Contains shared resources used by multiple APIs. Your configuration can contain only one common project with server settings.

Policy developers develop and test individual API projects and a common project in a local development environment. These projects must be managed and versioned in a SCM system.

![Team development using projects](/Images/docbook/images/promotion/team_dev_projects.png)

### API project resources

An API developed on the API Gateway consists of a set of resources developed in Policy Studio:

* Listener configuration (for example, HTTP URL path, JMS queue, and so on), which is the entry point to the API
* Policies that implement the API and integrate with back-end systems and services
* Other resources (for example, XSLTs, scripts, and so on) that the policies use to implement the API

All these resources are specific to the API, and not shared with other APIs. Therefore each API can be packaged as a standalone API project with no dependencies on other API projects.

### Common project resources

Some API Gateway deployments might have resources that are common to and used by multiple APIs. For example, a corporate security policy that must be applied to all APIs. These common resources can be packaged in a *dependent project* that can be used by multiple APIs.

For example, if an API Gateway deployment consists of 100 APIs, and a common set of corporate security policies, the configuration will be organized as 100 API projects, and a dependent project containing the common security policies.

### Project versioning

Use of a SCM system is required as the system of record for managing and versioning projects. Projects are the granular unit of configuration that is managed and versioned in the SCM. This enables a version history of APIs to be maintained in the SCM, and allows policy developers to roll back to previous versions of an API.

The SCM also maintains an audit trail of changes to individual projects, allowing organizations to determine who changed what and when. In addition, the SCM can be used to control what level of access individual policy developers have to specific projects.

## Policy developer workflow

When team development is enabled, the primary experience of using Policy Studio is project-centric and file-system oriented. Policy developers use Policy Studio to open projects from the file system, edit policies and other resources in the project, and save the project including their changes back to the file system.

If the project being worked on has a dependent project, the dependent project can be opened read-only in Policy Studio. Then in Policy Studio, the policy developer can deploy the project being worked on and the dependent project to an API Gateway to test their changes.

The integration between Policy Studio and the SCM system is through the file system. Policy Studio reads and writes projects from and to the file system, and policy developers check projects in and out of the SCM system using the SCM tools. This approach provides the maximum flexibility and support for many SCM solutions.

To work on an API, the policy developer checks out the API project and any dependent project from the SCM, edits the resources in the API project, deploys the API and dependent project on the API Gateway to test, saves the API project to the file system, and checks the updated API project back into the SCM.

## Continuous Integration: build and test

Continuous Integration (CI) techniques are used to automate the building and testing of API Gateway configuration from all projects in the SCM. The entire build and test process can be automated with CI tools and triggered by a developer checking in an updated project. This approach ensures that API Gateway configuration is always deployable.

![Continuous integration](/Images/docbook/images/promotion/team_dev_CI.png)

You can use the API Gateway policy package (`.pol`) and environment package (`.env`) mechanisms to promote and deploy API Gateway configuration in upstream environments.

API Gateway provides a build script (`projpack`) to build the unified API Gateway configuration by merging all the projects in the SCM to create the policy package. API Gateway also provides a deployment script (`projdeploy`) to deploy the policy and environment package to the API Gateway.

The CI process can be automated using DevOps tools such as Jenkins. For example, when an updated API project is checked into the SCM, a job is kicked off that builds the policy package, deploys the policy and environment packages to the test server, and runs a suite of tests.

Policy developers can use the API Gateway environmentalization features to enable customization of deployment artifacts for specific environments. The customization of environment configuration is performed using Configuration Studio. If a policy developer has added new environmentalized properties as part of changes they have made, the environment package for the CI environment must be updated before deploying and testing.

## Developer collaboration and resolving conflicts

This section explains how to resolve conflicts between and within API Gateway projects.

### Inter-project collaboration and conflicts

The team development approach enables multiple policy developers to collaborate on an API Gateway configuration that has multiple APIs. Each API is implemented as a different API Gateway project, and only one policy developer works on an API project at any one time. Multiple policy developers can work on different APIs in parallel without impacting each other.

Occasionally, there might be a conflict between projects due to a clash of resource naming, and this will be detected by the build script (`projpack`). If two policy developers working on different API projects create a resource of the same type with the same name in their respective projects, a conflict will occur.

For example, two policy developers, Joe and Fred, are developing two different API projects, and each creates a Database Connection called **TestDBConnection**. This creates a conflict when building the API Gateway configuration from the two projects. The `projpack` build script will attempt to merge the configuration from both projects, detect the conflict, and fail. If both DB Connections are intended to refer to different physical connections, you can resolve the conflict by keeping the DB Connections in both API projects, but adopting different names. If both DB Connections refer to the same physical connection, you can resolve the conflict by creating a single DB Connection in a common project referenced by both API projects.

{{< alert title="Note" color="primary" >}}The `projpack` script detects most conflicts between projects, however, it cannot detect conflicts between projects containing different REST APIs with the same name. For example, if `projectA` and `projectB` both have a REST API named `TestAPI`, and this REST API contains `method1` and `method2` in `projectA`, and in `projectB` it contains `method3` and `method4`, `projpack` succeeds in merging the projects with no conflicts, but the resulting `.fed` file is corrupt. You must ensure that you do not have two different REST APIs with the same name in different projects. {{< /alert >}}

For more details on `projpack`, see [Generate configuration packages from API Gateway projects](/docs/apigtw_devops/deploy_package_tools#generate-configuration-packages-from-api-gateway-projects).

### Intra-project collaboration and conflicts

Occasionally, multiple developers might need to make changes to the same API Gateway project in parallel. This will most likely occur when different policy developers are working on different API projects, but need to make changes to a common or shared dependent project.

For example, our two policy developers, Joe and Fred, both check out dependent project v1 to make changes unaware that the other has done the same. Joe makes his changes first and checks the dependent project back in as v2. Fred subsequently makes his changes, and then tries to check the dependent project back in as v2. The SCM system will detect this as a conflict and prevent Fred from checking the dependent project back in as v2.

Fred must now resolve this conflict by merging his changes with Joe’s, and checking the dependent project back in as v3. To perform this, Fred will check out Joe’s v2 from the SCM, and then use Policy Studio to compare and merge the two project versions to create the v3. Policy Studio enables you to compare two configurations to identify the differences between them.

## Continuous Delivery: promote and deploy

Similar to how a Source Code Management (SCM) system is required for managing the projects in the development environment, a package management repository is recommended for managing the policy and environment packages in upstream environments.

When stable, this version of the policy package can be promoted and deployed to upstream environments. The API Gateway administrator can configure environment packages (`.env`) for each upstream environment for this version of the policy package (`.pol`). For example, if the upstream environments consist of test, pre-production, and production, for each version of the policy package, the equivalent environment package for the test, pre-production, and production environments should be created.

The policy and environment packages are then stored in the package management repository. To deploy a specific version of the policy package to a specific environment, the correct versions of the policy package and environment package for that environment can be read from the package management repository and deployed (for example, deploy V2 to pre-production). Deployment of the policy and environment packages from the package management repository can be automated using the deployment script (`projdeploy`). For more details on `projdeploy`, see [Build and deploy API Gateway configurations](/docs/apigtw_devops/deploy_package_tools#build-and-deploy-api-gateway-configurations).

![Continuous delivery](/Images/docbook/images/promotion/team_dev_CD.png)
