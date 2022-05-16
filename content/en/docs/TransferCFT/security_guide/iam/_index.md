---

    title: Identity and access management
    linkTitle: Identity and access management
    weight: 60

---
<span id="__RefHeading___Toc473905768"></span>

## RBAC model

This product implements an RBAC model to manage identity and access control. This model is based on the concept that there are few roles within an organization, and instead of assigning access to each individual resource, the permission can be assigned to roles, and then those roles assigned to users. This process makes it much easier to manage permissions. It is not necessary to review the complete list of users each time a new resource is added, but only to review the list of roles. Similarly, it is not necessary to review the complete list of resources each time a new user is added, but only to review the list of roles.

The following diagram illustrates an RBAC model.

![](/Images/TransferCFT/acceptall_3.png)

The following describes how the RBAC model works:

- A role is a list of permissions (or privileges).
- A permission is the right to execute one or multiple actions on one resource.
- When using a permission, it becomes an operation.
- Roles can be grouped to create hierarchical roles.
- Roles belong to an organization.
- When assigning a role to a subject (or user), you can create a User/Role constraint to limit the scope of the role for this subject.
- To define these constraints, attributes can be associated with Resources.
- When the subject starts using a role, a session is created. When creating the session, it is possible to limit the scope of the role for this particular session through a role activation constraint.

For example:

- A resource is a file.
- Actions on this resource are: Create, Read, Update, or Delete.
- A permission (privilege) on this resource is Read File.
- An attribute of this resource is Directory Name.
- The User/Role constraint is “If directory name=XYZ.”
- The role RRR is created including this permission.
- The constraint is added when assigning this role to a subject (user).

<span id="__RefHeading___Toc473905769"></span><span id="_Ref1887409479"></span>

## Available resources and actions

The following table lists available resources in Transfer CFT with associated actions, attributes, and descriptions for each object.

### Generic resources

<span id="__RefHeading___Toc473905770"></span>Generic resources


| Resource | Description | Resource actions |
| --- | --- | --- |
| SERVICE:CATALOG | Manage the catalog file | PURGE |
| SERVICE:LOG | Manage the log file | SWITCH |
| SERVICE:ACCOUNT | Manage account files | SWITCH |
| SERVICE:CFTSRV | Manage the Transfer CFT server | STARTUP, SHUTDOWN, EDIT, VIEW |
| SERVICE:COM | Manage the COM file | DELETE, VIEW |
| COMMAND:EXTRACT | Extract Transfer CFT parameters | EXECUTE |
| COMMAND:MQUERY | Execute the Mquery command | EXECUTE |
| COMMAND:TURN | Execute the Turn command | EXECUTE |
| COMMAND:CFTSUPPORT | Allow the cft_support command | EXECUTE |


### Permission checks

Permission checks are based on the user name and are applied to both a resource and an action.
