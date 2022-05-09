{
"title": "Known limitations",
"linkTitle": "Known limitations",
"weight":"50",
"date": "2020-09-25",
"description": "List of known limitations related to the YAML configuration."
}

## Upgrade YAML configurations made with a Technical Preview release

YAML configuration upgrade is supported in a fully automated way. However, upgrade support to YAML configurations created using a technical preview release, can be performed following these criteria:

* Technical preview releases can be upgraded with manual steps, as described in following sections.
* Any YAML configuration created before [API Gateway March 21 update](/docs/apim_relnotes/20210330_apimgr_relnotes/) cannot be upgraded.
* The service pack update (`update_apigw.sh`) script does not support YAML configuration upgrade prior to [API Gateway May 21 update](/docs/apim_relnotes/20210530_apimgr_relnotes/#yaml-configuration-store-ga).

### Upgrade Technical Preview releases

To upgrade configurations made with a [Technical Preview release](/docs/apim_relnotes/20210330_apimgr_relnotes/), perform the following steps:

1. Change directory to your configuration root directory.
2. Create a file named `_version.yaml` in the `META-INF` directory.
3. Add the following content to your recently created `META-INF/_version.yaml` file:

  ```yaml
  ---
  version: 0.0.0

  ```

You are now ready to run the `yamles upgrade` command. For more information, see [Upgrade YAML configuration](/docs/apim_yamles/apim_yamles_cli/yamles_cli_upgrade).

## Policy Studio

There is no support for YAML configurations in Policy Studio.

## API Gateway Manager web UI

Deployment of YAML configurations via API Gateway Manager web UI is not supported.

## Team development

* Support of Team development using `yamles import` command, where you must import each project one at a time.
* No explicit support for project dependencies.
* No explicit support to look for merge conflicts. You must use `_fragment.yaml` directives to manage it.

For more information, see [Team development with YAML configuration](/docs/apim_yamles/apim_yamles_references/yamles_team_development).

## ES Explorer

Support of ES Explorer is limited to viewing and editing YAML configurations.

When an entity store is edited via ES Explorer or the entity store API, some fields in other entities might get reordered, creating more `diffs` than what was really modified.

It is not yet possible to import or export YAML configuration fragments in ES Explorer, this can only be done using the `yamles` CLI.

## API Manager

The YAML format supports API Manager. However, is not possible to run `setup-apimanager` on an API Gateway instance that has a YAML configuration deployed to it. To workaround this limitation:

1. Run `setup-apimanager` after deploying an XML federated configuration to your group.
2. When API Manager is setup, you can create a new project in Policy Studio by downloading the current configuration from the running API Gateway.
3. If the configuration is downloaded to `~/apiprojects/apimanager` you can convert this XML federated configuration to YAML and build a `.tar.gz` as follows:

    ```
    yamles fed2yaml federated:file:/home/user/apiprojects/configs.xml -o ~/yamlconfig --targz ~/yamlconfig.tar.gz
    ```

4. Deploy the `yamlconfig.tar.gz` to the API Manager enabled instance using `managedomain` or `projdeploy`.

{{< alert title="Note">}}
The format of API Manager data stored in Cassandra is the same regardless of whether a YAML configuration or an XML federated configuration is deployed.
{{< /alert >}}

## Node Manager

YAML configuration for Node manager is not supported.

## API Gateway Analytics

YAML configuration for Analytics is not supported.

## Deployment archive

You can update the deployment archive package properties by choosing `option 22` of the `managedomain` script. For more information, see [Updating Deployment Archive Properties](/docs/apim_yamles/yamles_packaging_deployment/#updating-deployment-archive-properties).
