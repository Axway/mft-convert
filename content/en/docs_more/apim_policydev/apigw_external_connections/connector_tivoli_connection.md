{
"title": "Configure Tivoli connections",
"linkTitle": "Configure Tivoli connections",
"weight": 12,
"date": "2019-10-17",
"description": "Configure API Gateway to connect to a Tivoli server."
}

Tivoli Access Manager for e-business is a commonly used product for securing web resources. Tivoli message filters allows the API Gateway to leverage existing Access Manager policies, thus avoiding the need to maintain duplicate policies in both products. At runtime, the Tivoli filters can delegate authentication and authorization decisions to Access Manager, and can also retrieve user attributes.

Tivoli connections determine how a particular API Gateway instance connects to an instance of a Tivoli server.

{{< alert title="Note" color="primary" >}}Each API Gateway instance can connect to a single Tivoli server. {{< /alert >}}

## Prerequisites

Before you can configure the **Tivoli Authorization** filter, you must configure API Gateway for integration with TAM. For more details, see the [API Gateway Authentication and Authorization Integration Guide](/docs/apigtw_auth_auth/).

## Configuration

To configure Tivoli connection, in the node tree, select **Environment Configuration > Server Settings > Security > Tivoli**. Alternatively, you can a click **Environment Configuration > External Connections > Tivoli Connection**, and select **Add a Tivoli Connection**. The newly added connection can then be assigned to a particular API Gateway instance.

In both cases, the **Tivoli Configuration** dialog is used to add the connection details required for the API Gateway to connect to the Tivoli server. The connection details are stored in the Tivoli configuration files. This dialog allows you to upload these files to the API Gateway.

Configure the following fields on the **Tivoli Configuration** dialog:

**Name**:

Enter a name onfor the connection, or select a previously configured Tivoli connection on which to base the new configuration.

**Select configuration file specification method**:

You can specify how to upload the Tivoli configurations files:

* **Upload Tivoli configuration files**: You can automatically upload all configuration files to a specified API Gateway instance. API Gateway overwrites these files each time at startup or refresh (for example, when configuration updates are deployed). This means that any changes to the main configuration file must be made using the Policy Studio and not directly to the file on disk.
* **Set file location for main Tivoli Configuration file**: You can manually copy the configuration files to the file system of a API Gateway instance and point API Gateway to pick up the configuration files from that location. API Gateway does not overwrite the files at startup or refresh time. You can edit the main configuration file directly using an editor of your choice.

**Connection enabled**:

Select to immediately enable the connection, deselect to disable the connection. You can edit this selection later to disable or enable the connection as needed by toggling this check box.

**Tivoli configuration files**:

If you selected to upload the Tivoli configuration files, use the use the **Load File** and **Next** buttons in the **Upload Tivoli configuration files** windows to upload all configuration files to the specified API Gateway instance. The files are displayed in the text area, and depending on the file, you may be able to edit the contents of the file.

**Server-side Tivoli configuration file**: Set the location of the main Tivoli configuration file.
