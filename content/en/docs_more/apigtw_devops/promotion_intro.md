{
"title": "Deployment and promotion overview",
"linkTitle": "Deployment and promotion overview",
"weight":"10",
"date": "2019-11-19",
"description": "Understand deployment and promotion concepts, including API Gateway configuration and configuration packages."
}

A typical enterprise-level customer will have several environments through which an API Gateway configuration will move from development to production. For example, this typically includes completely separate development, testing, and production domains. Promotion refers to the act of moving API Gateway configuration from one environment to another, and configuring environment-specific values so that the configuration can be deployed in each environment.

## Environment topology

In a typical environment topology, each environment is implemented as a completely separate API Gateway domain. The exact mapping of environments to domains is determined by how each environment is administered, and which users have access rights.

Environments are distinct administrative entities in which only certain users have the privileges to perform operations. For example, only Production Operations staff have access to the *production* environment. In the API Gateway architecture, a domain is a distinct administrative entity for managing groups of API Gateways. For example, the production environment is implemented as a distinct production domain to which only Production Operations staff have access.

In the following diagram, each environment is implemented as a distinct API Gateway domain. Developers work in their own *development* environments, and then promote their API Gateway configurations to a central Testing team that performs testing in a single *testing* environment. When testing is complete, the Testing team promotes the API Gateway configurations to the Production Operations team for deployment in the production environment. Development, Testing, and Production Operations teams have access to their respective environments only. Therefore, each environment should be implemented as a distinct domain.

![Environment topology](/Images/docbook/images/promotion/env_topology.png)

The API Gateway configuration is deployed to a group of API Gateways. Therefore, each domain consists of the required API Gateway groups to run the configurations. In the following diagram, the Development and Testing teams work in the same environment with common access to all API Gateway configurations. Therefore, in this case, there is a single domain for all the development and testing API Gateway groups.

![Shared environment topology](/Images/docbook/images/promotion/shared_env_topology.png)

{{< alert title="Note" color="primary" >}}The API Gateway does not mandate a specific environment-to-domain configuration, and is flexible enough to work with any architecture. However, you should manage your environments in an environment and domain topology. Implementing each environment as a distinct API Gateway domain is a good starting point.{{< /alert >}}

## Promotion and deployment

In a multienvironment topology, *promotion* refers to physically moving an API Gateway configuration between environments. For example, this might involve using FTP to transfer a configuration file, or loading and retrieving the file in a Configuration Management (CM) repository. In addition, promotion involves configuring environment-specific settings for the target environment (for example, users, certificates, and external connections to third-party systems). This is known as *environmentalization*.

Promotion typically involves two distinct tasks performed by different user types:

* In the downstream development environment, the policy developer prepares the configuration for promotion to upstream environments (for example, testing and production). This involves deciding what settings are environment-specific, and assumes expertise in policy development and configuration tools such as Policy Studio.
* The upstream user takes the configuration prepared by the policy developer, creates the environment-specific configuration, and deploys it. This is typically performed by an API Gateway administrator in upstream environments. The Configuration Studio tool used for this promotion step is designed for the skills of upstream administrators, and does not assume expertise in policy development and configuration.

*Deployment* refers to deploying configuration to an API Gateway group in a local domain. For example, you can deploy using the following tools:

* Policy Studio in a development environment
* API Gateway Manager in a testing environment
* Scripts in a production environment (for example, `managedomain` or a custom script)

## API Gateway configuration

API Gateway configuration consists of the following types of information:

![API Gateway configuration types](/Images/docbook/images/promotion/gw_config_types.png)

These component configuration types are described as follows:

| **Configuration type**  | **Description**                                                                                                                  | **Environment specific**           |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| **Policy**              | Policy rule definitions and configuration                                                                                        | Some might be environment specific |
| **Listener**            | Configuration for listeners used by policies (for example, for HTTP, JMS, or TIBCO)                                              | Some environment specific          |
| **External Connection** | Settings for external configuration (for example, database connections or authentication repositories)                           | Some environment specific          |
| **Certificate**         | Security certificates and keys used by policies                                                                                  | All environment specific           |
| **User**                | Local user name and password store for API client authentication                                                                 | All environment specific           |
| **Environment Setting** | Environment-specific settings for environmentalized configuration in the policy, listener, and external connection configuration | All environment specific           |
| **Package Property**    | Name-value pairs used to describe the contents of the configuration package (for example, `Version`, `ID`, or `Timestamp`).      | Some environment specific          |

## API Gateway configuration packages

The API Gateway deployment and promotion tools bundle the API Gateway configuration into the following configuration package files:

| **Package**             | **Description**   | **Used when**        |
|-------------------------| ------------------| ---------------------|
| **Deployment package**  (`.fed`)                         | Contains all API Gateway component configuration (policy, listener, external connection, user, certificate, and environment setting). Implemented as a `.fed` file. This contains all of the data that would be contained in separate policy and environment packages combined.                                         | Used by the policy developer in Policy Studio during the iterative development cycle to deploy all configuration. For more details, see [Deploy in a development environment](/docs/apigtw_devops/promotion_arch/#deploy-in-a-development-environment).                                                       |
| **Policy package** (`.pol`)                        | Contains the policy, listener, external connection, and environment setting configuration. Implemented as a `.pol` file. The environment settings in the `.pol` file contain a list of what has been environmentalized in the policy, listener, and external connection configuration. It does not contain the environment-specific values.  | Used when promoting APIs and policy configuration to an upstream environment (for example, testing or production). Its contents remain unchanged in the upstream environment. For more details, see [Environmentalize configuration](/docs/apigtw_devops/promotion_arch/#environmentalize-configuration). |
| **Environment package** (`.env`)                    | Contains the user, certificate, and environment setting configuration. Implemented as an `.env` file. The environment settings in the `.env` file contain a list of what has been environmentalized in the policy, listener, and external connection configuration, along with the environment-specific values.           | Environment-specific settings used in upstream environments only (for example, testing or production). For more details, see [Promote upstream (first cycle)](/docs/apigtw_devops/promotion_arch/#promote-upstream-first-cycle).      |

The combined contents of the policy package and environment package are equivalent to the contents of the deployment package, which contains all API Gateway configuration.

![API Gateway configuration packages](/Images/docbook/images/promotion/gw_config_packages.png)

## Configure package properties

The API Gateway configuration package files include property files that contain name-value pairs describing the package contents, and which are known as *package properties*.

The API Gateway bundles its configuration in the following package formats:

* Deployment package (`.fed`)
* Policy package (`.pol`)
* Environment package (`.env`)

All three API Gateway configuration package formats (`.fed`, `.pol`, and `.env`) contain property name-value pairs, which you can use to describe the package contents. These package property values are stored in package property files (`.mf`). A deployment package (`.fed`) has two sets of package properties, one associated with the policy-related configuration, and one associated with the environment-related configuration. Policy packages (`.pol`) and environment packages (`.env`) have a single set of properties each.

### Default properties

The default set of package properties that can be edited includes the following:

| **Property**       | **Description**                                                                                                       |
|--------------------|-----------------------------------------------------------------------------------------------------------------------|
| **Name**           | Name associated with the configuration (for example, `Payment API Configuration`)                                     |
| **Description**    | Description associated with the configuration (for example, `API Gateway configuration settings for the Payment API`) |
| **Version**        | Configuration version (for example, `v3`)                                                                             |
| **VersionComment** | Comment relating to the configuration version (for example, `Added SSL`)                                              |

These fields are all free format text fields. You can set them to an empty value, or remove them completely, as required. The set of properties is completely customizable. You can add your own custom properties if required.

### Read-only properties

The package also includes the following read-only, system-controlled package properties:

| **Property**  | **Description**                       |
|---------------|---------------------------------------|
| **Id**        | A unique ID for the package           |
| **Timestamp** | The time that the package was written |

### Configure properties in Policy Studio

When editing an API Gateway configuration in Policy Studio, you can add, edit, or remove the policy properties and environment properties in the **Environment Configuration** > **Package Properties**
tree node. For example, the following window is displayed when you select **Policies**:

![Policy Studio package properties](/Images/docbook/images/promotion/ps_properties.png)

To add a new package property, click the add icon on the right of the window. Similarly, to delete a package property, click the delete icon to the right of the property.

### Configure properties in Configuration Studio

You can edit environment properties in Configuration Studio using a similar window. You can only view policy properties because these are read-only.

Package property values are deployed to an API Gateway along with the entire configuration in the relevant configuration package structure.
