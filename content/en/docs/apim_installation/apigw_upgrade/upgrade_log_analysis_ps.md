{
    "title": "Resolve upgrade issues in Policy Studio",
    "linkTitle": "Resolve upgrade issues in Policy Studio",
    "weight": 70,
    "date": "2019-10-07",
    "description": "Detect and analyze upgrade issues in Policy Studio."
}

Policy Studio provides graphical features to help you detect and analyze upgrade issues. You can use the **Tools > Upgrade Log Analysis** option to analyze an `upgrade.log` file. You can also use the tool to analyze upgrade issues when creating a new project from existing configuration, or when importing a configuration fragment. For more information, see [Upgrade log analysis](/docs/apim_policydev/apigw_poldev/upgrade_log_analysis_ps/).

You can also use Policy Studio to resolve configuration issues. If the upgrade process identifies issues with the configuration in your old installation, you can use Policy Studio to resolve the issues. There are two ways of doing this:

1. Resolve the issues in the upgraded configuration during the upgrade, and deploy the updated configuration to the new installation when the upgrade completes. See [Resolve issues during upgrade](#resolve-issues-during-upgrade).
2. Resolve the issues in the upgraded configuration after the upgrade completes, and deploy the changes. See [Resolve issues after upgrade](#resolve-issues-after-upgrade).

## Resolve issues during upgrade

During the upgrade, if the `upgrade` command identifies configuration issues that must be resolved in Policy Studio, open the problematic configuration in Policy Studio. For example, in Policy Studio version 7.7, create a new project using **From .fed file** or **From .pol and .env files**, and select the correct files under the following directory:

```
/opt/Axway-7.7/apigateway/upgrade/bin/out/upgrade/esgroups/groups/group-2/
```

Modify the problematic configuration in Policy Studio, and save the project for deployment after sysupgrade completes.

{{< alert title="Tip" color="primary" >}}If Policy Studio is not installed on the node that has the problematic configuration, copy the directory to a machine where Policy Studio is installed, and edit it there. {{< /alert >}}

## Resolve issues after upgrade

Alternatively, you can choose to complete the upgrade first and then resolve the configuration issues in Policy Studio. In this case, load the configuration from the running API Gateway after the upgrade completes. For example:

1. In Policy Studio version 7.7, click **New Project from an API Gateway instance**.
2. Select the API Gateway group that has the problematic configuration.
3. Make changes to the configuration in Policy Studio, and deploy them.
4. Repeat this process for each API Gateway group in the topology that the upgrade step identified as problematic.

{{< alert title="Note" color="primary" >}}You must take care to deploy the correct API Gateway project to the correct API Gateway group.{{< /alert >}}
