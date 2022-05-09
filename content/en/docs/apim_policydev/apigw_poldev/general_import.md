{
"title": "Import and export API Gateway configuration",
"linkTitle": "Import and export API Gateway configuration",
"weight": 12,
"date": "2019-10-17",
"description": "Import and export configuration data to share it in a development environment and to manage differences and references."
}

The ability to import and export XML or YAML-based configuration data is useful in a development environment if you wish to share and test configuration with other developers. By exporting configuration data from one API Gateway installation, and importing into another API Gateway installation, you can effectively share your API Gateway configuration in a development environment. This also enables you to manage differences and references between configuration components.

## Import API Gateway configuration fragment

You can import XML or YAML-based configuration data into your API Gateway configuration (for example, policies, certificates, and users).

{{< alert title="Note" color="primary" >}}The recommended way to export configuration between different environments is to use configuration packages. Select **File** > **Export** from the main menu. For more details, see [Manage projects](/docs/apim_policydev/apigw_poldev/general_project).{{< /alert >}}

### Import configuration fragment

To import previously-exported API Gateway configuration data, perform the following steps:

1. Click the **Import Configuration Fragment**
    button in the Policy Studio toolbar.
2. Browse to the location of the XML file or YAML configuration that contains the previously exported fragment that you wish to import.
3. Select the XML file/YAML configuration, and click **Open**.
4. If a passphrase was set on the configuration from which the data was previously exported, enter it in the **Enter Passphrase**
    dialog, and click **OK**.
5. In the **Import Configuration**
    dialog, all configuration items are selected for import by default. If you do not wish to import specific items, deselect them in the tree.
6. Click **OK**
    to import the selected configuration items.
7. The selected configuration items are imported into your API Gateway configuration and displayed in the Policy Studio tree. For example, any imported policies and containers are displayed under the **Policies** node.

{{< alert title="Note" color="primary" >}}Be careful when deselecting configuration nodes for import. Deselecting certain nodes might make the imported configuration inconsistent by removing supporting configuration. {{< /alert >}}

### View differences

The **Import Configuration**
dialog displays the differences between the existing stored configuration data (destination) and the configuration data to be imported (source). Differences are displayed in the tree as follows:

* **Addition**: Exists in the source Configuration being imported but not in the destination Configuration. Displayed as a green plus icon.
* **Deletion**: Exists in the destination Configuration but not in the source Configuration being imported. Displayed as a red minus icon.
* **Conflict**: Exists in both Configurations but is not the same. Displayed as a yellow warning icon.

If you select a particular node in the **Import Configuration**
tree, the **Differences Details**
panel at the bottom of the screen shows details for this Configuration entity (for example, added or removed fields). In the case of conflicts, changed fields are highlighted.

Some Configuration entities also contain references to other entities. In this case, an icon is displayed for the field in the **Difference Details**
panel. If you double-click a row with an icon, you can drill down to view further **Difference Details**
dialogs for those entities.

### What is imported

When configuration data is imported, some configuration items are imported in their entirety. For example, if the contents of a particular policy are different, the entire policy is replaced (new filters are added, missing filters are removed, and conflicting filters are overwritten). In addition, if a complex filter differs in its children, child items are removed and added as required (for example, WS Filter, Web service, User, and so on).

Other imports are additive only. For example, importing a single certificate does not remove the certificates already in the destination Certificate store. All references to other policies are also maintained during import.

{{< alert title="Note" color="primary" >}}Although importing some configuration items removes child items by default, you can deselect child nodes to keep existing child items. However, you should take care to avoid inconsistencies. The default selection applies in most cases.{{< /alert >}}

### Upgrade configuration from an earlier version

When you import configuration created using an earlier version of API Gateway, the configuration is automatically upgraded to the current API Gateway version configuration. This results in the migration of the configuration entities present in the `.xml`
file or YAML configuration that is being imported.

The **Migration Report**
trace console at the bottom of the window displays the migration report output that is generated when the configuration is upgraded. For example:

![Migration Report Console](/Images/docbook/images/general/import_migrate_report.png)

The **Migration Report**
console also displays links that navigate to the appropriate upgraded configuration entity. For example, the following window is displayed when the `MyFirstDirectoryScanner`
link is clicked:

![Migration Report Navigation](/Images/docbook/images/general/import_migrate_link.png)

## Export API Gateway configuration

You can export API Gateway configuration data by right-clicking a Policy Studio tree node (for example, policy or policy container), and selecting the relevant export menu option (for example, **Export Policy**). The configuration is exported to an XML file or YAML archive, which you can then import into a different API Gateway configuration.

{{< alert title="Note" color="primary" >}}For details on migrating API Gateway configuration between development, testing, and production environments, see the [API Gateway DevOps Deployment Guide](/docs/apigtw_devops/).{{< /alert >}}

### What is exported

You can export API Gateway configuration items by right-clicking a node in the Policy Studio tree. For example, this includes the following types of Policy Studio tree nodes:

* Policies
* Policy containers
* Schemas
* Alerts
* Caches
* Regular expressions (White list)
* Attacks (Black list)
* Users
* Certificates and Keys
* Relative paths
* Remote hosts
* Database Connections
* Server Settings

In addition, you can also export configuration items that are associated with the selected tree node. For example, this includes referenced policies, MIME types, regular expressions, schemas, and remote hosts. For details on exporting additional configuration items, see the next section.

### Export configuration items

To export API Gateway configuration items, perform the following steps:

1. Right-click a Policy Studio tree node (for example, policy or policy container), and select the relevant menu option (for example, **Export Policy**).
2. The first window in the export wizard is a read-only window that displays the configuration items to be exported. The **Exporting**
    tree displays the selected tree node (in this case, policy), which is exported by default. **The following configuration items will also be exported**
    tree includes additional referenced items that are also exported by default along with the policy (for example, MIME types, regular expressions, and schemas).
3. Click **Finish**
    if this selection suits your requirements. Otherwise, click **Next**
    to refine the selection.
4. In the next window, you can select optional configuration items for export. The **Additional configuration items that may be exported** tree on the left includes dependent items that are not exported by default. For example, these include the following:
    * Outbound references: Configuration items directly referenced out from the export set to other configuration stores (for example, certificates, users, or external connections).
    * Inbound references: Configuration items in other configuration stores that directly reference items in the export set.
    * Associated configuration directly related to the export set (for example, remote hosts or relative paths).
5. To add an item for export, select it in the **Additional configuration that may be exported** tree on the left, and click **Add**.
6. To remove an item for export, select it in the **Additional configuration that will be exported** tree on the right, and click **Remove**.

    The original set of items in the **Additional configuration that will be exported** tree cannot be removed. Only items added from the **Additional configuration that may be exported** tree can be removed.

7. By default, items displayed in the **Additional configuration that may be exported** tree are scoped to direct references to the export set (inbound, outbound, and associated). You can select **Display additional configuration that depends on items to be exported** to recursively add references to this tree when additional configuration items are added to the export set.
8. Click **OK** to export the selected configuration.

#### Referenced policies

When exporting a policy or policy container, by default, any policies referenced by the policy are included for export and displayed in the **Additional configuration that will be exported** list.
