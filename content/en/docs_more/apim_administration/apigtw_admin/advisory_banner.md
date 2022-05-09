{
"title": "Configure an advisory banner",
"linkTitle": "Configure an advisory banner",
"weight":"75",
"date": "2019-10-14",
"description": "Display an advisory warning message about unauthorized use of API Gateway when establishing a successful user session from Policy Studio or API Gateway Manager."
}

To enable an advisory banner, perform the following steps:

1. Select **Settings > Advisory Banner** in the API Gateway Manager web console.
2. Configure the following settings:

    * **Advisory banner enabled**:
    Select whether the banner is enabled. The default is disabled.
    * **Advisory banner text**:
    Enter the text to display on the advisory banner. The default text is:

    ```
    Warning - unauthorized use of this tool is strictly prohibited and subject to audit, investigation, and potential prosecution.
    ```

3. Click **Apply** to save the changes.

When the banner is enabled, it is displayed on the API Gateway Manager login dialog:

![Advisory banner in API Gateway Manager](/Images/APIGateway/advisory_banner_gwmgr.png)

The advisory banner is also displayed when you log in to Policy Studio:

![Advisory banner in Policy Studio](/Images/APIGateway/advisory_banner_ps.png)
