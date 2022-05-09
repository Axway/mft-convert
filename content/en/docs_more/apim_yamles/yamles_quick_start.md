{
"title": "Get started with YAML configuration",
"linkTitle": "Get started",
"weight":"20",
"date": "2020-09-24",
"description": "Convert API Gateway configuration from XML to a YAML human-friendly format."
}

## Prerequisites

* Install or upgrade your API Gateway to its [latest version](/docs/apim_relnotes/).
* Upgrade your XML federated configuration using [upgradeconfig](/docs/apim_installation/apigw_upgrade/upgrade_analytics/#upgradeconfig-options) or [projupgrade](/docs/apim_reference/devopstools_ref/) before converting it.

{{< alert title="Note">}}
If you are using Git for your YAML configurations on a Windows operating system, ensure to set Git configuration setting `core.longpaths` to avoid *filename too long* error on checkout.

```
git config core.longpaths true
```

{{< /alert >}}

## Convert XML federated configuration to YAML

You must start with an existing XML federated configuration. If you do not have an existing XML federated configuration you can create a new one in Policy Studio using one of the [factory templates](/docs/apim_policydev/apigw_poldev/gs_project/#new-project-from-a-template-configuration).

If your project is called, for example, `test`, then the XML files for it will exist in `~/apiprojects/test`. The federated URL is `federated:file:/home/user/apiprojects/test/configs.xml`.

{{< alert title="Note">}}
Ensure that you have upgraded any old Policy Studio installation and your existing XML federated configuration to the latest version before converting it to YAML.
{{< /alert >}}

If the XML federated configuration is encrypted with a passphrase, you do not need to decrypt it before or after the conversion to YAML. The converted YAML configuration will remain encrypted as all values are copied from one configuration to the other.

To convert your XML federated configuration to YAML:

1. Open a terminal and change directory to the API Gateway installation directory `apigateway/posix/bin`.
2. Convert the XML to YAML:

    ```
    ./yamles fed2yaml -s /home/user/apiprojects/test -o /path/to/yamles
    ```

3. Validate the converted YAML configuration:

    ```
    ./yamles validate -s /path/to/yamles
    ```

For deployment to an API Gateway runtime, see [Packaging and deployment](/docs/apim_yamles/yamles_packaging_deployment/).

For troubleshooting any errors, and more details on options and commands, see [CLI Tool](/docs/apim_yamles/apim_yamles_cli).
