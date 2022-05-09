{
"title": "Manage users",
"linkTitle": "Manage users",
"weight":"120",
"date": "2019-10-22",
"description": "API Gateway *users* provide access to the messages and services protected by API Gateway, while *admin users* provide access to the API Gateway configuration management features available in Policy Studio, Configuration Studio, and API Gateway Manager."
}

## Manage API Gateway users

By default, the API Gateway user store contains the configuration data for managing API Gateway user information. This user store is typically used in a development environment, and is useful for demonstration purposes.

In a production environment, user information may be stored in existing user Identity Management repositories such as Microsoft Active Directory, Oracle Access Manager, CA SiteMinder, and so on.

API Gateway users specify the user identity in the API Gateway user store. This includes details such as the user name, password, and X.509 certificate. API Gateway users must be a member of at least one user group. In addition, users can specify optional attributes, and inherit attributes at the group level.

To view all existing users, select the **Environment Configuration > Users and Groups > Users** node in the tree. The users are listed in the table on the main panel. You can find a specific user by entering a search string in the **Filter** field.

### Add users

You can create gateway users on the **Users** page. Click the **Add** button on the right.

To specify the new user details, complete the following fields on the **General** tab:

* **User Name**: Enter a name for the new user.
* **Password**: Enter a password for the new user.
* **Confirm Password**: Re-enter the user's password to confirm.
* **Signing Key**: Click to load the user certificate from the **Certificate Store**. For details on how to create and import certificates, see [Manage certificates and keys](/docs/apim_administration/apigtw_admin/general_certificates/).

You can also specify optional user attributes on the **Attributes** tab, which is explained in the next section.

### User attributes

You can specify attributes at the user level and at the group level on the **Attributes** tab. Attributes specify user configuration data (for example, attributes used to generate SAML attribute assertions).

The **Attributes** tab enables you to configure user attributes as simple name-value pairs. The following are examples of user attributes:

* `role=admin`
* `email=steve@axway.com`
* `dept=eng`
* `company=axway`

You can add user attributes by clicking the **Add** button. Enter the attribute name, type, and value in the fields provided. The `Encrypted` type refers to a string value that is encrypted using a well-known encryption algorithm or cipher.

### User groups

API Gateway user groups are containers that encapsulate one or more users. You can specify attributes at the group level, which are inherited by all group members. If a user is a member of more than one group, that user inherits attributes from all groups (the superset of attributes across the groups of which the user is a member).

To view all existing groups, select the **Environment Configuration > Users and Groups > Groups** node in the tree. The user groups are listed in the table on the main panel. You can find a specific group by entering a search string the **Filter** field.

### Add user groups

You can create user groups on the **Groups** page. Click the **Add** button on the right to view the **Add Group** dialog.

To specify the new group details, complete the following fields on the **General** tab:

**Group Name**:

Enter a name for the new group.

**Members**:

Click the **Add** button to display the **Add Group Member** dialog, and select the members to add to the group.

You can also specify optional attributes at the group level on the **Attributes** tab. For more details, see [User attributes](#user-attributes).

### Update users or groups

To edit details for a specific user or group, select it in the list, and click the **Edit** button on the right. Enter the updated details in the **Edit User** or **Edit Group** dialog.

To delete a specific user or group, select it in the list, and click the **Remove** button on the right. Alternatively, to delete all users or Groups, click the **Remove All** button. You are prompted to confirm all deletions.

## Manage admin users

When logging into the Policy Studio or API Gateway Manager, you must enter the user credentials stored in the local admin user store to connect to the API Gateway server instance. Admin users are responsible for managing API Gateway instances using the API Gateway management APIs. To manage admin users, click the **Settings** > **Admin Users**
tab in the API Gateway Manager.

### Admin user privileges

After installation, a single admin user is defined in the gateway manager with a user name of `admin`. Admin user rights in the system include the following:

* Add another admin user
* Delete another admin user
* Update an admin user
* Reset admin user passwords

{{< alert title="Note" color="primary" >}}An admin user *cannot* delete itself.{{< /alert >}}

### Remove the default admin user

If you need to remove the default admin user, perform the following steps:

1. Add another admin user.
2. Log in as the new admin user.
3. Delete the default admin user.

The **Admin Users** tab displays all existing admin users. You can use this tab to add, update, and delete admin users. These tasks are explained in the sections that follow.

### Admin user roles

The gateway uses Role-Based Access Control (RBAC) to restrict access to authorized users based on their assigned roles in a domain. Using this model, permissions to perform specific system operations are assigned to specific roles only. This simplifies system administration because users do not need to be assigned permissions directly, but instead acquire them through their assigned roles.

For example, the default admin user (`admin`) has the following user roles:

* `Policy Developer`
* `API Server Administrator`
* `KPS Administrator`

### API Gateway user roles and privileges

User roles have specific tools and privileges assigned to them. These define who can use which tools to perform what tasks. The user roles provided with the gateway assign the following privileges to admin users with these roles:

| Role                       | Tool                | Privileges                                                                                    |
|----------------------------|---------------------|-----------------------------------------------------------------------------------------------|
| `API Server Administrator` | API Gateway Manager | Read/write access to API Gateway Manager.                                                     |
| `API Server Operator`      | API Gateway Manager | Read-only access to API Gateway Manager.                                                      |
| `Deployer`                 | Deployment scripts  | Deploy a new configuration.                                                                   |
| `KPS Administrator`        | API Gateway Manager | Perform create, read, update, delete (CRUD) operations on data in a Key Property Store (KPS). |
| `Policy Developer`         | Policy Studio       | Download, edit, deploy, version, and tag a configuration.                                     |

A single admin user typically has multiple roles. For example, in a development environment, a policy developer admin user would typically have the following roles:

* `Policy Developer`
* `API Server Administrator`

### Add a new admin user

Complete the following steps to add a new admin user to the system:

1. Click the **Settings** > **Admin Users** tab in API Gateway Manager.
2. Click the **Create** button.
3. In the **Create New Admin User** dialog, enter a name for the user in the **Username** field.
4. Enter a user password in the **Password** field.
5. Re-enter the user password in the **Confirm Password** field.
6. Select roles for the user from the list of available roles (for example, `Policy Developer` and `API Server Administrator`).
7. Click **Create**.

### Remove an admin user

To remove an admin user, select it in the **Username** list, and click **Delete**. The admin user is removed from the list and from the local admin user store.

### Reset an admin user password

You can reset an admin user password as follows:

1. Select the admin user in the **Username** list.
2. Click the **Edit** button.
3. Enter and confirm the new password in the **Password** and **Confirm Password** fields.
4. Click **OK**.

### Manage admin user roles

You can manage the roles that are assigned to specific admin users as follows:

1. Select the admin user in the **Username** list.
2. Click the **Edit** button.
3. Select the user roles to enable for this admin user in the dialog (for example, `Policy Developer` or `API Server Administrator`).
4. Click **OK**.

**Edit user roles**:

To add or delete specific roles, you must edit the available roles in the `adminUsers.json` and `acl.json` files in the `conf` directory of your gateway installation.

### Configure a password policy for admin users

To configure the password policy that applies to admin user passwords, perform the following steps:

1. Click the **Settings** > **Admin Users** tab in API Gateway Manager.
2. Select **Password Policy enabled** to enable the password policy rules on this page. This is not selected by default.
3. Configure the following in **PASSWORD RULES**:
    * **Password must not be equal to the account name**: The password cannot be identical to the admin user name. This is selected by default.
    * **Password must not be the reverse of the account name**: The password cannot be the reverse of the admin user name. This is selected by default.
    * **Password must not contain the account name**: The password cannot contain the admin user name. This is selected by default.
    * **Minimum password length**: The password must be the specified minimum length. Defaults to 4 characters. If no value is specified, this rule is disabled.
    * **Password history length**: Enter the number of previous passwords to be compared. Leave this field empty to disable this rule.
    * **Minimum character differences from last password**: Enter the minimum number of different characters from the last password. Leave this field empty to disable this rule.
    * **Password lifetime (days)**: Enter how long the password is valid for in days. Leave this field empty to disable password expiry.
4. Configure the following in **PASSWORD COMPOSITION RULES**:
    * **Minimum uppercase characters**: Defaults to 1 uppercase alphabetic character.
    * **Minimum lowercase characters**: Defaults to 1 lowercase alphabetic character.
    * **Minimum numeric characters**: Defaults to 1 numeric character.
    * **Minimum special characters**: Defaults to 1 special character (`~!@#$%^&*()-_=+\[{}];:"",< >/?`).
    * If no value is specified in these fields, these rules are disabled.
5. Click **Apply** when finished.

## Configure a passphrase policy for node managers and API Gateway groups

You can configure a [passphrase policy](https://docs.axway.com/bundle/apim-security-guide/page/security_best_practices.html#Securitybestpractices-Passphrasepolicy) that applies to the passphrases of node managers and API Gateway groups to prevent users to create extremely weak passphrases, such as `password` or `1234`.

The passphrase policy is disabled by default. To enable it, perform the following steps as an API Server Administrator:

1. Call the `GET /topology/passphrasepolicy` method of the [API Gateway API v1.0 Topology API](http://apidocs.axway.com/swagger-ui/index.html?productname=apigateway&productversion=7.7.0&filename=api-gateway-swagger.json#!/Topology_API/get_topology_passphrasepolicy) to get the current passphrase policy.
2. Update the configuration returned from the API call to meet your specifications.
3. Paste the new configuration into the body of the [PUT /topology/passphrasepolicy](http://apidocs.axway.com/swagger-ui/index.html?productname=apigateway&productversion=7.7.0&filename=api-gateway-swagger.json#!/Topology_API/put_topology_passphrasepolicy) method to enable your passphrase policy.

The following is a sample body, including all available configurations for updating the passphrase policy:

```json
{
  "enabled" : true,
  "assertions" : [ {
    "description" : "general",
    "matchCount" : "*",
    "enabled" : true,
    "assertion" : [ {
      "enabled" : true,
      "resourceID" : "PASSWORD_NOT_NULL",
      "name" : "Passphrase cannot be empty"
    }, {
      "enabled" : true,
      "resourceID" : "PASSWORD_MIN_LENGTH",
      "minLength" : "4",
      "name" : "Passphrase must be longer than N characters"
    }, {
      "enabled" : true,
      "resourceID" : "PASSWORD_NOT_EQUAL_TO_ACC_NAME",
      "name" : "Passphrase cannot be the same as the domain/node manager/group name"
    }, {
      "enabled" : true,
      "resourceID" : "PASSWORD_NOT_EQUAL_TO_REV_ACC_NAME",
      "name" : "Passphrase cannot be the same as the reverse of domain/node manager/group name"
    }, {
      "enabled" : false,
      "resourceID" : "PASSWORD_NOT_CONTAINING_ACC_NAME",
      "name" : "Passphrase cannot contain domain/node manager/group name"
    }, {
      "enabled" : false,
      "resourceID" : "PASSWORD_DISTANCE",
      "name" : "Passphrase Distance"
    }, {
      "enabled" : false,
      "resourceID" : "PASSWORD_LIFETIME",
      "name" : "Passphrase Lifetime"
    }, {
      "enabled" : false,
      "resourceID" : "PASSWORD_NOT_IN_HISTORY",
      "name" : "Passphrase not in history"
    } ]
  }, {
    "description" : "composition",
    "matchCount" : "*",
    "enabled" : true,
    "assertion" : [ {
      "enabled" : true,
      "resourceID" : "MUST_HAVE_UPPER_CASE",
      "name" : "Must contain an upper case character",
      "count" : "1"
    }, {
      "enabled" : true,
      "resourceID" : "MUST_HAVE_LOWER_CASE",
      "name" : "Must contain a lower case character",
      "count" : "1"
    }, {
      "enabled" : true,
      "resourceID" : "MUST_CONTAIN_DIGIT",
      "name" : "Must contain a number",
      "count" : "1"
    }, {
      "enabled" : true,
      "characters" : "~!@#$%^&*()-_=+\\|[{}];:'\",<.>/ ?",
      "resourceID" : "MUST_CONTAIN_SPECIAL_CHARACTERS",
      "name" : "Must contain a special character",
      "count" : "1"
    } ]
  } ]
}
```

{{% alert title="Note" %}}Configuring a passphrase policy is currently not possible by way of API Manager UI.{{% /alert %}}