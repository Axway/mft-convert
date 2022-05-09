{
"title": "Manage project dependencies",
"linkTitle": "Manage project dependencies",
"weight":"80",
"date": "2019-11-19",
"description": "Manage dependencies on common resources."
}

API Gateway deployments can have common resources used by multiple APIs. For example, this includes corporate security policies to be applied to all APIs. You can package such common resources in a *dependent project* used by multiple APIs.

To manage dependencies between API Gateway projects in Policy Studio, select **File** > **Manage Dependencies** from the main menu. Alternatively, right click **Project Dependencies** in the navigation tree on the left. This opens the **Manage Dependencies** dialog where you can add and remove project dependencies:

![Manage project dependencies](/Images/docbook/images/promotion/team_dev_dependencies.png)

## Add project dependencies

In the **Manage Dependencies** dialog, click **Add** to add a new project as a dependent project of the current project. This enables you to use the common resources (policies, resources, external connections, and so on) of the dependent project in your current project. A dialog enables you to browse to the directory of the project to add as a project dependency (for example, `C:\Users\jbloggs\apiprojects\MyTestAPIGateway`). If there are no conflicts between the projects, the dependent project is merged with the current project.

{{< alert title="Note" color="primary" >}}The encryption passphrase of the dependent project must be the same as the passphrase of the current project. If it is not, an error message is displayed and the dependency is not added.{{< /alert >}}

### Resolve project conflicts

If Policy Studio finds conflicts between the projects, this displays the **Project Dependencies Conflicts** dialog showing details of the conflicts. Policy Studio will not allow you to add the project as a dependency until the conflicts are resolved.

To resolve conflicts, open the project that you want to add as a dependency, and remove or resolve the conflicts.

The newly added project dependency is added to the navigation tree on the left. You can navigate the tree but its nodes are read-only and are only for informational purposes. There are no right-click options available and when you click on a node it does not open up an associated right hand pane (with the exception of **Show All References**).

## Remove project dependencies

In the **Manage Dependencies** dialog, click **Remove** to remove a project dependency. If there are references to the project dependency in your current project, you cannot remove the project dependency until all references are removed. In this case, a warning message displays. When you click **OK**, the **Show All References** panel displays, and the entities that reference the dependent project are shown.

## Identify project dependencies

When developing configuration in the current project, you can refer to and reuse entities from dependent projects. For example, a policy shortcut defined in a policy in the current project can refer to a common policy from a dependent project.

To help to distinguish entities from the current project and from a dependent project, reusable entities are displayed in an *italic* font. The name of the project to which a reusable entity belongs to is added as a suffix in square brackets (`[]`) to the entity name.

## Show broken references

When you open a project or import a configuration fragment, Policy Studio checks for broken references in the configuration. This is especially important when dealing with project dependencies when there are references in one project to another project. If there are any, Policy Studio displays the list of broken references on the right. Each item in the list displays a name and a link to the broken reference. You can click the link and go to the policy with the broken reference to fix it. You can also access this feature by selecting **Window > Show View > Broken References View** in the main menu.
