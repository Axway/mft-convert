{
"title": "Deployment and promotion tasks",
"linkTitle": "Deployment and promotion tasks",
"weight":"20",
"date": "2019-11-19",
"description": "Understand the main tasks involved in developing, promoting, and deploying API Gateway configuration."
}

This topic explains the breakdown of tasks performed by a policy developer in a development environment, and the tasks performed by an API Gateway administrator in an upstream environment (for example, testing or production).

## Deploy in a development environment

In a development environment, the policy developer works in a continuous cycle of iterative development, deployment, and testing. In this environment, it makes sense to keep all API Gateway configuration in a single package. This enables the policy developer to deploy the API Gateway configuration directly from Policy Studio in a single *deployment package*. The following diagram shows an example environment topology.

![Deploying in a development environment](/Images/docbook/images/promotion/deploy_dev_env.png)

The deployment package contains the entire API Gateway configuration, and is implemented as a `.fed` file.

## Environmentalize configuration

When development is complete, the policy developer must prepare the configuration for promotion to upstream environments. This involves *environmentalizing*
the configuration that will require environment-specific settings in upstream environments. The policy developer performs the following tasks in Policy Studio:

* Selects the policy, listener, and external connection configuration settings that are environment specific.
* Enters values for these environment-specific settings to ensure that the configuration remains deployable in the Development environment. These environment-specific settings are contained in the environment settings in the deployment package. If you have already entered values for these settings, these are used so you do not have to manually re-enter them.
* Exports a *policy package* (`.pol` file) on disk for promotion. For example, this enables you to transfer the file using FTP to the upstream environments, or to load the file into a CM repository.

The following diagram shows an example environment topology.

![Externalizing configuration](/Images/docbook/images/promotion/extern_config.png)

The policy package that is exported for promotion is implemented as a `.pol` file. This file should remain unchanged when it is promoted to upstream environments.

## Promote upstream (first cycle)

A *first cycle* promotion refers to promoting to an upstream environment in which no previous promotions have been performed. This means that the upstream environment is still running the default factory configuration that is installed with API Gateway. In this case, there is no existing upstream *environment package* (`.env`) to load into Configuration Studio at promotion time.

### Create an environment package

In an upstream environment (for example, testing), the API Gateway administrator uses Configuration Studio to create an environment package that is specific to their environment. Because this is the first promotion cycle, the administrator opens the policy package (`.pol`) received from the development environment, and performs the following tasks:

* Specifies values for the environment-specific settings selected in the development environment (for example, policy, listener, and external connections).
* Imports or creates certificates and keys.
* Defines users and user groups.
* Exports the environment package to a file on disk. The environment package is implemented as an `.env` file. For version history and rollback, you could also load the file into a CM repository.

### Deploy the policy and environment packages

When the environment package has been created, the administrator can use API Gateway Manager or scripts to deploy both the policy package from the development environment and the newly created environment package. Each environment will have its own version of the `.env` file containing environment-specific settings, certificates, users, and so on. This constitutes a full deployable configuration when combined with the unmodified `.pol` file from the development environment.

Alternatively, the administrator can save a deployment package (`.fed`) from Configuration Studio, which merges the policy and environment package data. If you are not concerned with moving an unmodified policy package from the development environment to all upstream environments, you can save a single `.fed` file, and deploy this using API Gateway Manager or scripts (for example, if you want a single file for convenience).

The following diagram shows an example environment topology.

![Upstream administration (first phase)](/Images/docbook/images/promotion/upstream_phase1.png)

{{< alert title="Note" color="primary" >}}The environment settings in the environment package (`.env`) override the environment settings in the policy package (`.pol`). The environment settings in the policy package indicate settings for which you need to specify environment-specific values. {{< /alert >}}

## Promote upstream (subsequent cycles) {#promote-upstream}

A *subsequent cycle* promotion refers to promoting to an upstream environment that has already had configuration promoted to it (any number of times). In this case, there is an existing version of the upstream environment package (`.env`) to load into Configuration Studio at promotion time.

### Create the environment package

In the upstream environment, the API Gateway administrator uses the Configuration Studio to create an environment package specific to their environment that contains all the environment-specific settings, certificates, and so on. This is required for the new policy package received from the development environment. Because this is not the first promotion cycle, there is already an environment package deployed in this environment. The administrator must merge this with the new policy package from the development environment, which enables reuse of environment-specific settings already entered.

The administrator opens the new policy package from the development environment, and the environment package currently deployed in their environment. Opening these `.pol` and `.env` files displays a merged view of the environment settings. The administrator then performs the following tasks:

* Specifies values for new environment-specific settings required by the new policy package from the development environment.
* Updates values for environment-specific settings that previously existed (if necessary).
* Adds or removes certificates and keys.
* Adds or removes users and user groups.
* Exports the environment package to a file on disk. Alternatively, for version history and rollback, you could load the file into a CM repository.

### Deploy policy and environment packages

When the environment package has been created, the API Gateway administrator can then deploy both the policy package received from the development environment, and the new environment package using API Gateway Manager, or using scripts.

The following diagram shows an example environment topology.

![Subsequent upstream administration](/Images/docbook/images/promotion/upstream_phase2.png)

## Roll back configuration

You must ensure that you maintain a copy of previous configuration versions (policy and environment packages) in case you need to roll back and deploy an earlier configuration version. For example, you could use a Configuration Management (CM) repository to manage and roll back configuration package versions.

## Multi-site HA environments

Some environments might require different environment values for connections, certificates, and so on (for example, a remote High Availability (HA) site for a production environment in an active/passive configuration). In this scenario, the primary site is actively processing requests. The remote site is the backup passive configuration, deployed but not processing requests, and only becomes active if the primary site goes down. The same API Gateway configuration is deployed in both sites. Each site could be a separate domain, or one domain with different groups for each site. Specific environment values could be different for each site, for example, the remote site might connect to a different backup authentication server.

When the administrator receives the policy package (`.pol`) from the downstream environment, they can use Configuration Studio to create separate environment packages (`.env`) for the primary site and the remote site. The only difference between both environment packages is in the environment values required. In the primary site, the administrator deploys the policy package and the primary site environment package. In the remote HA site, the administrator deploys the same policy archive and the remote site environment package.

## Passphrase-protected configuration

When you promote and deploy passphrase-protected configuration between environments (for example, from testing to production), the passphrase for the target configuration (production) can be different from the passphrase in the source configuration (testing).

If you are using a different passphrase in each environment, when the deployment takes place, you can specify the correct passphrase for the target configuration in Policy Studio.

## Externalize an instance configuration

When API Gateway configuration is deployed to a group, the configuration package settings are applied to all API Gateway instances in the group. You can also specify API Gateway configuration values on a per-API Gateway instance basis using environment variables in the `envSettings.props` file. For example, you can specify different values for the port on which the API Gateway listens for HTTP traffic, depending on the environment in which the API Gateway is deployed.

The environment variable settings in the `envSettings.props` file are external to the API Gateway core configuration. The API Gateway runtime settings are determined by a combination of external environment variable settings and core configuration. This mechanism provides a simple and powerful approach to configuring specific API Gateway instances in the context of API Gateway group configuration defined in policy and environment packages.

{{< alert title="Note" color="primary" >}}Environment variables in the `envSettings.props` file apply to the gateway instance only. Configuration packages (`.fed`, `.pol`, and `.env`
files) apply to the gateway group.{{< /alert >}}

The `envSettings.props` file is located in the `conf` directory of your API Gateway installation, and is read each time the API Gateway starts up. Environment variable values specified in the `envSettings.props` file are displayed as environment variable selectors in the Policy Studio (for example, `${env.PORT.TRAFFIC}`).

### Configure environment variables

The `envSettings.props` file enables you to externalize configuration values and set them on a per-server environment basis. This section shows the configuration syntax used, and shows some example values in this file.

#### Environment variable syntax

If the API Gateway configuration contains a selector with a format of `${env.X}`, where *X* is any string (for example, `MyCustomSetting`), the `envSettings.props` file must contain an equivalent name-value pair with the following format:

```
env.MyCustomSetting=MyCustomValue
```

When the API Gateway starts up, every occurrence of the `${env.MyCustomSetting}` selector is expanded to the value of `MyCustomValue`. For example, by default, the HTTP port in the server configuration is set to `${env.PORT.TRAFFIC}`. Specifying a name-value pair of `env.PORT.TRAFFIC=8080` in the `envSettings.props` file results in the server opening up port 8080 at startup.

#### Example settings

The following simple example shows some environment variables set in the `envSettings.props` file:

```
# default port API Gateway listens on for HTTP traffic
env.PORT.TRAFFIC=8080
# default port API Gateway listens on for management & configuration HTTP traffic
env.PORT.MANAGEMENT=8090
# path to sample directory in API Gateway instance directory
env.SAMPLE.PATH=${environment.VINSTDIR}/sampleDir
```

The following example shows the corresponding `${env.PORT.TRAFFIC}` selector displayed in the **Configure HTTP Interface** dialog. At runtime, this is expanded to the value of the `env.PORT.TRAFFIC` environment variable specified in the `envSettings.props` file:

![Environment variable in Policy Studio](/Images/docbook/images/promotion/env_variable.png)

All entries in the `envSettings.props` file use the `env.` prefix, and the corresponding selectors specified in Policy Studio use the `${env.*}` syntax. If you update the `envSettings.props` file, you must restart or deploy the API Gateway for updates to be applied to the currently running API Gateway configuration.
