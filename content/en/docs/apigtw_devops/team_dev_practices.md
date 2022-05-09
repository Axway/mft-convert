{
"title": "Team development best practices",
"linkTitle": "Team development best practices",
"weight":"70",
"date": "2019-11-19",
"description": "Recommended best practices for creating API Gateway team development projects."
}
This includes creating a common project and API projects, adding the projects to an SCM system, and setting up project dependencies.

## When to use team development

You should use team development if you have a large team of policy developers who need to work in parallel developing APIs and policies to be deployed as a single large API Gateway configuration.

When using team development you must:

* Use a Source Code Management (SCM) system
* Enable team development project templates and dependencies in Policy Studio
* Follow the workflow detailed in [Policy developer workflow](/docs/apigtw_devops/team_dev_intro#policy-developer-workflow)

The team development workflow requires you to split projects into separate pieces of configuration so that teams can work on them independently. If you do not have a requirement for multiple policy developers to work in parallel, you should not use a team development workflow. Instead you should use the previous workflows in Policy Studio where policy developers typically connect to a running API Gateway instance, download the configuration, make desired changes, and then deploy the configuration back to the API Gateway. This approach encourages use of API Gateway as a design-time repository for policies, in addition to as the runtime for executing policies. This workflow still requires you to create a project in Policy Studio, but there is no requirement to split the project into separate pieces of configuration, as in the team development workflow.

{{< alert title="Note" color="primary" >}}Team Development functionality is not available when using [YAML-based projects](/docs/apim_yamles/).{{< /alert >}}

## Increase Policy Studio memory

If you have a large API Gateway configuration, you might need to increase the memory allocation in Policy Studio. You can do this by increasing the value of the `Xmx` setting in the following file:

```
INSTALL_DIR/policystudio/policystudio.ini
```

The default value is `-Xmx768m`.

## Create projects and dependencies for new or existing deployments

This section describes recommended best practices for creating API Gateway team development projects and project dependencies in new or existing API Gateway deployments.

### Upgrade existing deployment to API Gateway version 7.7

If you have an existing deployment from an earlier version of API Gateway, you must upgrade your API Gateway installation to version 7.7. Additionally, after you upgrade your API Gateway installation, you must upgrade the configuration in your development environment to version 7.7.

### Remove redundant configuration in existing deployment

If you have an existing deployment, you should reduce the size of the API Gateway configuration in Policy Studio by deleting redundant, duplicate, or older versions of the following API Gateway configuration:

* Web services
* Policies
* WSDL document bundles
* Listener paths

### Enable team development in Policy Studio

To enable team development project templates and project dependencies in API Gateway, you must turn on the team development option in Policy Studio preferences. In Policy Studio select **Window > Preferences > Team Development** and select **Enable Team Development**.

{{< alert title="Note" color="primary" >}}An existing team development-enabled project is automatically detected by Policy Studio, and you do not need to enable team development in this case.{{< /alert >}}

### Break up existing configuration into a common project and API projects

If you have an existing deployment, you must break up your existing monolithic API Gateway configuration as follows:

* One common project (containing global configuration that applies across all other projects)
* Multiple projects per API

The following sections describe how you can do this.

### Create a common project

Create a common project that contains shared resources used by multiple APIs. For example, your common project might contain a corporate security policy that must be applied to all APIs. Your configuration can contain only one common project with server settings.

To create a common project, perform the following steps:

1. If you have an existing configuration, open the configuration in Policy Studio as a project, and use the export feature to create configuration fragments for a separate common project with common configuration. For example, right-click the relevant policy in the Policy Studio tree, and select **Export Policy**.
2. Create a new common project in Policy Studio. Select **New Project > From a template configuration > Team Development - Common Project (with Server Settings)**.

    ![New common project with server settings](/Images/docbook/images/promotion/new_common_project_server_settings.png)

3. In this common project, create any configuration that you wish to reuse. For example:

    * All **Authentication** and **Authorization** policies should be in the common project and the other API projects should call out using **Policy Shortcut** filters to these policies.
    * Other common configuration includes global policies such as **Throttling**, XML/JSON schema validation, and XML to JSON transformation, or global settings such as **Alerts**.

    **Listeners** are not common configuration due to the link between **Paths** and **Ports**.

4. To import common configuration that you exported previously, select **Import Configuration Fragment**. You can put any configuration that you wish to reuse in the common project.

The following example shows a common project containing Authentication and Authorization policies that were exported from an existing configuration and imported into the common project.

![Common project with imported configuration](/Images/docbook/images/promotion/common_project_imported_config.png)

### Create separate API projects

Create API projects that contain resources that are specific to an API and not shared with other APIs. Your configuration can contain multiple API projects.

To create API projects, perform the following steps:

1. If you have an existing configuration, use the export feature in Policy Studio to create configuration fragments for the separate API projects with API configuration only. For example, right-click the relevant policy in the Policy Studio tree, and select **Export Policy**.
2. To create an API project for each separate API or group of APIs that need to be managed together, select **File** > **New Project** > **From a template configuration** > **Team Development - API Project (without Server Settings**).

    ![New API project from template](/Images/docbook/images/promotion/template_api_project.png)

3. In this API project, create the configuration for each separate API or group of APIs that need to be managed together.
4. To import API configuration that you exported previously, select **Import Configuration Fragment**.

### Add projects to SCM

You now have one common project, and separate API projects for each API, all stored locally. You must add your API Gateway project files to your SCM system before you start adding project dependencies in Policy Studio.

For example, using Git, an administrator creates a master repository for API projects. Policy developers can clone this master repository to their own local machine. They then create projects and commit to their local repository. When appropriate, they can push to the master repository. They must always perform a pull before a push request to ensure they get the latest from master and resolve any conflicts.

### Create project dependencies

To create project dependencies between a common project and an API project, perform the following steps:

1. Open one of the API projects in Policy Studio.
2. To create a project dependency between this API project and the common project, right-click **Project Dependencies** in the Policy Studio tree, select **Manage Project Dependencies**, and click **Add**.

    The common project configuration displayed in the **Project Dependencies** section of the API project are read-only.
3. Use a common configuration item (for example, a policy) from the common project in the API project (for example, using a policy shortcut).

    ![Example of a project dependency](/Images/docbook/images/promotion/project_dependancy_example.png)

4. Push the changes made to the API project to your SCM. For example, you are using Git and have changed an API project named Orders API in `/home/jdoe/apiprojects/ordersapi/`. If you changed the message in a **Set Message** filter from `OK` to `Not OK`, typically, the `PrimaryStore.xml` shows as modified when you run the `git status` command.

For more details on managing project dependencies, see [Manage project dependencies](/docs/apigtw_devops/team_dev_dependencies/).
