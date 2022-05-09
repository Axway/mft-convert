{
    "title": "API management workflows",
    "linkTitle": "Workflows",
    "weight": "10",
    "date": "2019-04-15",
    "description": "Understand the concepts and workflows in API management."
}

## API consumer user registration workflow

The use cases for the API consumer user registration workflow are:

Case 1: Automatic approval, delegated user management
: An API consumer registers in API Manager. The user is not created, and is placed in a pending queue. The user receives a security email to prove they own the email address. When they click a link, the user is created, and they receive a `created` email.

Case 2: Automatic approval, no delegated user management
: Same behavior as case 1.

Case 3: No automatic approval, no delegated user management
: Same behavior as case 1 and 2, except when the user clicks the link in the security email, they receive a `pending` email, and remain in the pending queue. An email notification is sent to the API administrator email address specified in the API Manager settings, and the API administrator approves or rejects the registration. When approved, the user is created, and receives a `created` email.

Case 4: No automatic approval, delegated user management
: Same behavior as case 3, except that the email notification is sent to the contact email address for the organization, and the API administrator or organization administrator approves or rejects the registration.

The following table shows the difference between case 3 and 4 when the appropriate settings are selected in API Manager:

| Auto-approve user registration | Delegate user management | API Portal | Output |
|--------------------------------|--------------------------|------------|--------|
| Disabled                       | Disabled                 | Enabled    | Email sent to API admin email address for approval by API admin, and directed to API Manager. |
| Disabled                       | Enabled                  | Enabled    | Email sent to the organization email address for approval by the API admin or organization admin, and directed to API Portal. If API Portal is disabled, the admin is directed to API Manager in all cases. |

For more details, see [API Manager settings](/docs/apim_reference/api_mgmt_config_web/#api-manager-settings).

### Application creation workflow

The use cases for the application creation workflow are:

Case 1: Automatic approval, delegated application management
: A user creates a new application, requesting access to specific APIs. The application is automatically approved and created.

Case 2: Automatic approval, no delegated application management
: Same behavior as case 1.

Case 3: No automatic approval, no delegated application management
: Same behavior as case 1 and 2, except the application is not created, and enters a pending queue. An email notification is sent to the contact address for the organization, and the API administrator approves or rejects the registration. When approved, the user receives a `created` email.

Case 4: No automatic approval, delegated application management
: Same behavior as case 3, except that the API administrator or organization administrator approves or rejects the registration.

The following table shows the difference between case 3 and 4 when the appropriate settings are selected in API Manager:

| Auto-approve applications | Delegate application management | API Portal | Output |
|---------------------------|---------------------------------|------------|--------|
| Disabled                  | Disabled                        | Enabled    | Email sent to the organization email address for approval by API admin, and directed to API Manager. |
| Disabled                  | Enabled                         | Enabled    | Email sent to the organization email address for approval by API admin or organization admin, and directed to API Manager in all cases. |

For more details, see [API Manager settings](/docs/apim_reference/api_mgmt_config_web/#api-manager-settings).

### API access workflow

The use cases for the API access workflow are:

Case 1: Organization wants new API access
: Only the API administrator can assign API access to organizations.

Case 2: User wants new API access to an existing application, automatic approval
: The user adds a new API, and the API access is granted immediately.

Case 3: User wants new API access to an existing application, no automatic approval, no delegated application management
: The user adds a new API, and the API access request is placed in a `pending` queue. An email notification is sent to the contact address for the organization, and the API administrator approves or rejects the API access request. When approved, the access is granted.

Case 4: User wants new API Access to an existing application, no automatic approval, delegated application management
: Same behavior as case 3, except that the API administrator or organization administrator approves or rejects the API access request.

{{< alert title="Note" color="primary" >}}When an organization administrator adds a new front-end API, the API enters the `pending` queue, and the API administrator receives an email to approve or reject publishing the API.{{< /alert >}}