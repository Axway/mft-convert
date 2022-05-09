{
"title": "Example promotion and deployment",
"linkTitle": "Example promotion and deployment",
"weight":"30",
"date": "2019-11-19",
"description": "Use case example on how to promote configuration from a development environment to a testing environment."
}

## Example topology

This example assumes the following simple environment topology:

* A domain is configured in the development environment with a group of API Gateways named **Dev Payment API Group**.
* A domain is configured in the testing environment with a group of API Gateways named **Testing Payment API Group**. The configuration developed in the development environment must be promoted to the servers in this group.

![Example environment topology](/Images/docbook/images/promotion/example_topology.png)

## Edit configuration and deploy in development environment {#edit-configuration}

The policy developer in the development environment uses Policy Studio to create policies, users, certificates, listeners, and so on as required for the business solution they are developing. The policy developer will edit and deploy the configuration to the **Dev Payment API Group** repeatedly until they are finished with the configuration.

### Deploy in Policy Studio

The policy developer deploys the configuration in Policy Studio by clicking the **Deploy**
button in the toolbar when editing the configuration. This displays the following window:

![Deploy in Policy Studio](/Images/docbook/images/promotion/ps_deploy.png)

Select the **Group** and API Gateway instances to which to deploy the configuration, and click **Deploy**. This uploads the configuration to the Admin Node Manager for the group, and then deploys it to the API Gateway instances on the hosts.

{{< alert title="Note" color="primary" >}}This simple example shows a group with a single API Gateway instance. Groups will typically have multiple API Gateway instances. If some Node Managers in the group are not running, do not select the API Gateways on those hosts, and you can still deploy to the other hosts in the group. {{< /alert >}}

## Environmentalize the environment-specific settings

When the policy developer is developing policies in an iterative manner as described in Step 1, they might choose not to consider what settings are environment-specific yet, or they might choose to environmentalize these settings as they go along. Either way, before promotion can occur, all settings that are environment-specific must be environmentalized to prepare the configuration for promotion to upstream environments.

### Display environmentalized configuration

You must first enable the display of configuration settings that are assigned for environmentalization in Policy Studio. Select **Window** > **Preferences** > **Environmentalization** in the main menu, and select **Allow environmentalization of fields**.

### Environmentalize configuration settings

For example, the developer chooses to environmentalize the following settings in the configuration:

* **URL**, **User Name**, and **Password** fields in a **Default Database Connection**
* **URL** field in a **Connect to URL** filter in a policy named **GetProducts**
* **X.509 Certificate** field in an HTTPS interface named **OAuth 2.0 Interface**
* **URL**, **User Name**, **Password**, and **Signing Key** fields in a **Sample Active Directory Connection**

The policy developer edits the database connection, **Connect to URL** filter, HTTPS interface, and LDAP connection. You can click the **Environmentalize** icon (![Environmentalize icon](/Images/docbook/images/deploy/env_off.png), a globe icon on the right side of the fields) as shown in the following examples. Alternatively, press **Ctrl-E** to environmentalize a selected field.

{{< alert title="Tip" color="primary" >}}You must select the field for focus before the **Environmentalize** icon (![Environmentalize](/Images/docbook/images/deploy/env_off.png)) is displayed.{{< /alert >}}

For example, to environmentalize the database connection, select **Environment Configuration** > **External Connections** > **Database Connections** > **Default Database Connection** > **Edit**, and click **Environmentalize** next to the appropriate fields:

![Example Database Connection](/Images/docbook/images/promotion/environmentalize_db.png)

To environmentalize the **Connect to URL** filter, Select **Policies** > **QuickStart** > **Virtualized Services** > **REST** > **GetProducts** > **Connect to Heroes' REST Service**, and click **Environmentalize** next to the **URL** field:

![Example Connect to URL Filter](/Images/docbook/images/promotion/environmentalize_connect_url.png)

To environmentalize the HTTPS interface, select **Environment Configuration** > **Listeners** > **API Gateway** > **OAuth 2.0 Services** > **Ports** > **OAuth 2.0 Interface**, and click **Environmentalize** next to the **X.509 Certificate** field:

![Example HTTPS Interface](/Images/docbook/images/promotion/environmentalize_https.png)

To environmentalize the LDAP connection, select **External Connections** > **LDAP Connections** > **Sample Active Directory Connection** > **Edit**, and click **Environmentalize** next to the appropriate fields:

![Example Database Connection](/Images/docbook/images/promotion/environmentalize_ldap.png)

When configuration settings have been environmentalized, the corresponding node in the Policy Studio tree is displayed with a globe icon and bold text.

### View environment settings

After environmentalizing the fields, the following nodes are available under the **Environment Configuration** > **Environment Settings** node in Policy Studio:

![Viewing Environment Settings](/Images/docbook/images/promotion/environment_settings.png)

For YAML-based projects, the following nodes are available under the **Environment Configuration** > **Yaml Values Settings** node in Policy Studio:

![Viewing Yaml Values Settings](/Images/docbook/images/promotion/yaml_values_settings.png)

### Update environment settings

Assuming the policy developer has already entered values for the fields that they have selected to be environmentalized, these values are automatically specified under the **Environment Configuration** > **Environment Settings** node. To update the setting values for the development environment, you must use the **Environment Settings** or **Yaml Values Settings** tree node.

For example, using the example environmentalized settings, the following window is displayed when you select **Environment Settings** > **External Connections** > **Database Connections** > **Default Database Connection**:

![Edit Database Connection in Environment Settings](/Images/docbook/images/promotion/edit_db_env_setting.png)

The YAML equivalent defaults to **Yaml Values Settings** > **External_Connections** > **Database_Connections** > **Default_Database_Connection**.

### Deselect environment settings

To disable environmentalization for a setting, you can right-click its node in the **Environment Configuration** > **Environment Settings** tree, and select **Remove**. This also deselects the field in the window used to edit the configuration setting (for example, database connection). The value configured before environmentalization is displayed again.

Alternatively, you can click the **Jump to configuration** link, and return to the window used to edit the configuration setting, and deselect the Environmentalize icon on this field, or press **Ctrl-E**. This also removes the field as a setting to be configured under the **Environment Settings** tree. The value configured before environmentalization is displayed again.

{{< alert title="Note" color="primary" >}}YAML supports referencing the same value across multiple entities and fields. As a result **Jump to configuration** is more complex and not yet supported. While you may insert the current value back into an entity by toggling **Environmentalization** via the configuration dialog on the original entity, the value from your `values.yaml` file is not automatically deleted to avoid breaking other possible references.{{< /alert >}}

### Deploy the configuration

After all environment-specific fields have been selected, and appropriate values set for the development environment, the policy developer should deploy and test the updated configuration. The deployment package (`.fed`) deployed to the **Dev Payment API Group** will contain the entries in the environment settings store, and the associated values suitable for the development environment.

### Environmentalize reference fields

Configuration fields that point to other fields are known as *reference fields*. For example, in an HTTPS Interface or **XML Signature** filter, you environmentalize a reference to an X.509 certificate. You can also environmentalize references to complex types such as authentication repositories. If a reference to an authentication repository is environmentalized, you could set the repository to the local user store in the development environment, and to an LDAP repository in the testing environment.

The standard way to environmentalize a certificate at group level is to click **Environmentalize** on its configuration window. Environmentalizing a certificate, or any other reference field, is the same as all other fields. For example, when you environmentalize the signing certificate in an **XML Signature** filter, the **Environment Configuration** > **Environment Settings** tree where you enter environment-specific values displays a node for the **XML Signature**  filter. The window on the right includes a **Signing Key** button to display a list of available certificates. You must select one of these certificates in Configuration Studio or Policy Studio. This field will most likely be prepopulated in Policy Studio if you already selected a certificate before clicking **Environmentalize**.

Alternatively, you can environmentalize a certificate using an alias. For example, in the development environment, the **XML Signature** filter could use a certificate named `MySigningCert`. The policy package (`.pol`) created from the development environment must be merged with an environment package (`.env`) that contains a certificate with the same alias.

{{< alert title="Note" color="primary" >}}You can also environmentalize certificates using an alias at the API Gateway instance level as described in [Externalize an instance configuration](/docs/apigtw_devops/promotion_arch/#externalize-an-instance-configuration). However, certificates are normally environmentalized at the API Gateway group level as described in this topic.{{< /alert >}}

## Save policy package in Policy Studio for promotion

The policy developer finishes editing and environmentalizing the configuration that they are running with, and deploys it to the API Gateway. They must then save the policy package in Policy Studio to enable promotion to the testing environment. To save the policy package, perform the following steps:

1. When the active configuration is loaded, select **File > Save > Policy Package**.

    Before creating the policy package, Policy Studio automatically detects any unenvironmentalized certificate references, and enables you to automatically environmentalize these settings before proceeding.
2. Browse to the directory in which to save the package, and enter its filename (for example, `c:\temp\payment.pol`).
3. Click **Save**.

A policy package (`.pol`) file is created on disk. The policy developer must transfer this file to the testing environment using some external mechanism (for example, FTP or email).

{{< alert title="Note" color="primary" >}}The steps described so far are the same for first and subsequent cycle promotions. For the first cycle, the policy developer will most likely use the default factory configuration as their starting point for editing the configuration. In subsequent cycles, the starting point will most likely be the existing configuration currently deployed to the **Dev Payment API Group**.{{< /alert >}}

## Create testing environment package in Configuration Studio

This step depends on whether this is a first cycle promotion or a subsequent cycle promotion.

### First cycle promotion: Open the policy package

If this is a first cycle promotion, the testing API Gateway administrator uses Configuration Studio to open the policy package created in the development environment by the policy developer in Step 3. The administrator does not need to open an environment package for a first cycle promotion. To open the policy package, perform the following steps:

1. Open a command prompt, and change to your Configuration Studio installation directory (for example, `INSTALL_DIR\configurationstudio`).
2. Enter `configurationstudio` to start Configuration Studio.
3. Select **File > Open File**.
4. Enter or browse to the location of the **Policy Package** (for example, `c:\temp\payment.pol`).
5. Click **OK**.

{{< alert title="Note" color="primary" >}}The Configuration Studio opens policy packages and environment packages by opening files available on disk. The administrator must ensure that the required files are available to the application. {{< /alert >}}

### Subsequent cycle promotion: Open the policy and environment packages

If this is a subsequent cycle promotion, the testing API Gateway administrator uses the Configuration Studio to open the policy package created in the development environment by the policy developer in Step 3. You must also open the currently deployed environment package for the testing environment. If you do not open the currently deployed environment package at this point, you might need to re-enter certificates and settings that you entered for the previous promotion.

### Specify environment settings

The administrator must navigate the **Environment Configuration** > **Environment Settings** tree node in Configuration Studio, and enter values that are specific to the testing environment. Values from the development environment are not displayed. The **Environment Settings** tree displays a question mark next to any settings without values. For a first cycle promotion, initially all environment setting values will be empty. Assuming a first cycle promotion with the sample data from Step 1, the **Environment Settings** tree is displayed in Configuration Studio as follows:

![Environment settings](/Images/docbook/images/promotion/environment_settings_cs.png)

For subsequent cycle promotions, settings that are still required by the new policy package from the development environment, and that existed in previously promoted policy packages, will have values configured. Any certificates, keys, user, and user groups previously created will also be shown. Environment settings that existed in previously promoted configuration but are no longer required will be removed. New settings in the new policy package are listed with no value.

For example, assuming a first cycle promotion using the example environmentalized settings, the following window is displayed when you select **Environment Configuration** > **Environment Settings** > **External Connections** > **LDAP Connections** > **Sample Active Directory Connection**:

![Edit database connection in environment settings](/Images/docbook/images/promotion/edit_ldap_env_setting_cs.png)

{{< alert title="Note" color="primary" >}}You cannot add or delete environment settings using Configuration Studio. These are predetermined by the policy developer in the development environment. {{< /alert >}}

### Update certificates and users

The administrator can add, edit, or remove new certificates, keys, users, and user groups in Configuration Studio in the same way as in Policy Studio. For more details, see the following topics:

* [Manage certificates and keys](/docs/apim_administration/apigtw_admin/general_certificates/)
* [Manage users](/docs/apim_administration/apigtw_admin/manage_user_access/)

If a certificate reference has been environmentalized, such as in the **Sample Active Directory Connection**, you must create or import a testing environment certificate in Configuration Studio. This makes the certificate available for selection when the environmentalized settings are edited in the **Environment Settings** tree in Configuration Studio.

### Update package properties

At any time, the API Gateway administrator can edit the environment package properties by selecting the **Environment Configuration** > **Package Properties** > **Environment** tree node in Configuration Studio. For example:

![Environment package properties](/Images/docbook/images/promotion/env_archive_manifest_props.png)

If the API Gateway administrator selects the **Environment Configuration** > **Package Properties** > **Policy** node, this displays a read-only view of the policy package properties on a similar window. For example:

![Policy package properties](/Images/docbook/images/promotion/policy_archive_manifest_props.png)

{{< alert title="Note" color="primary" >}}You cannot edit the contents of the policy package file in Configuration Studio.{{< /alert >}}

### Save the environment package

When you have entered all the environment-specific settings for the testing environment, select **File > Save > Environment Package** in Configuration Studio. An environment package (`.env`) file is saved to disk.

## Deploy configuration to testing environment group

The testing API Gateway administrator takes the policy package unchanged from the development environment created in Step 3, and the environment package created using Configuration Studio for the testing environment created in Step 4, and deploys them to the **Testing Payment API Group** using API Gateway Manager or scripts.

For example, to deploy using API Gateway Manager, perform the following steps:

1. Enter the following URL in your browser to launch API Gateway Manager:

    ```
    https://127.0.0.1:8090/
    ```

2. On the **Dashboard** tab, select the API Gateway group in the **TOPOLOGY** section.
3. Click the **Edit** button on the right of the group, and select **Deploy Configuration**.
4. Select **I wish to deploy configuration contained in a Policy Package and Environment Package**, and browse to the `.pol`  and `.env` files.
5. Click **Deploy**.

## Further configuration updates in testing environment

This section describes how to update environment-specific settings using Configuration Studio, and if necessary, using Policy Studio.

### Update environment settings using Configuration Studio

If further updates are required to the environment-specific settings in the testing environment, the testing API Gateway administrator can open the policy package and environment package files in Configuration Studio at any time, and update the contents for the environment package file. The administrator can then deploy the policy package and updated environment package files to the **Testing Payment API Group** using API Gateway Manager or scripts.

### Update environment settings using Policy Studio

Normally the policy package will be promoted through to upstream environments without any updates. However, in some cases, a single policy package for all environments will not be possible. For example, you might wish to use different authorization filters in development and testing environments. But the policy developer might not have sufficient knowledge to create the necessary configuration for all upstream environments in the policy package. In this case, the API Gateway administrator in the upstream environment must use Policy Studio to make the required changes.

The administrator will open a policy package from the development environment and the current testing environment package (if one exists) in Policy Studio, before making the testing environment-specific updates to the configuration. The administrator can save a policy package (`.pol`) and an environment package (`.env`) from Policy Studio. They can deploy them as usual to the **Testing Payment API Group** using API Gateway Manager or scripts. Alternatively, they can save a single deployment package (`.fed`), and deploy this package.

## Further promotions to upstream environments

If further promotions to more upstream environments are required, repeat [Create testing environment package in Configuration Studio](#create-testing-environment-package-in-configuration-studio) and [Deploy configuration to testing environment group](#deploy-configuration-to-testing-environment-group) only.

{{< alert title="Note" color="primary" >}}
Some environments (for example, testing and production) might be exact copies of each other, which enables you to deploy the same environment package to both environments. In these cases, repeat [Deploy configuration to testing environment group](#deploy-configuration-to-testing-environment-group) only.
{{< /alert >}}
