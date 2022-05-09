{
"title": "Kerberos constrained delegation",
"linkTitle": "Kerberos constrained delegation",
"weight":"10",
"date": "2019-11-14",
"description": "Kerberos *constrained* delegation (KCD) enables API Gateway to act as a trusted Kerberos service principal, to acquire a Kerberos service ticket in the name of the requesting end user, and to authenticate to a constrained set of Kerberos back-end services as the end user."
}

In this scenario:

* **Client application**: Does not support Kerberos authentication, or cannot provide Kerberos credentials.
* **Back-end service**: Requires Kerberos authentication with end user's credentials. Multiple back-end services may exist.
* **API Gateway**: Authenticates the client application, then acts as a Kerberos client and authenticates to the back-end service as the end user.

The client application can only authenticate using a non-Kerberos authentication mechanism. The back-end service requires Kerberos authentication, and must authenticate the real user associated with the client application.

API Gateway authenticates the client using a non-Kerberos authentication mechanism. API Gateway has no access to the end user’s Kerberos secret key or keytab. API Gateway must map the incoming user credential to the Kerberos client principal name of the user to be impersonated. To do this, API Gateway uses a selector to generate the end user's Kerberos principal name.

As a trusted proxy, API Gateway impersonates the end user's credentials and authenticates to the back-end service as the end user. The back-end service sees the request originating directly from the original end user and can authenticate the end user with Kerberos credentials. API Gateway can request Kerberos service tickets on behalf of the client to more than one Kerberos service and authenticate to multiple back-end services as the end user in a single policy.

Even when using a client application that supports Kerberos authentication, an end user may not be able to provide the Kerberos credentials, for example, when working remotely on a browser and trying to access the back-end service from outside of the Kerberos realm. Configuring API Gateway for KCD helps solve this authentication problem as well.

{{< alert title="Note" color="primary" >}}Cross-domain authentication is not supported for Kerberos constrained delegation. However, you can configure a chain of policies with a separate Kerberos service account for each domain to overcome this.{{< /alert >}}

![Example flow of API Gateway acting as a Kerberos client when end user application does not support Kerberos authentication the back-end service requires.](/Images/IntegrationGuides/KerberosIntegration/KerberosConstrainedDelegation/APIgw_KCD_1.png)

KCD uses two Microsoft Kerberos extensions, Protocol Transition and Constrained Delegation:

* **Protocol Transition (S4U2Self)** – A Kerberos service can obtain a Kerberos service ticket to itself on behalf of a Kerberos principal (the end user) without requiring the end user to initially authenticate using Kerberos. The end user can authenticate using some other authentication mechanism.
* **Constrained Delegation (S4U2Proxy)** – A Kerberos service can request and obtain further Kerberos service tickets to other services on behalf of an end user after receiving the first Kerberos service ticket for that end user. The further Kerberos service tickets can only be requested to a constrained set of services configured in the KDC.

In API Gateway, Protocol Transition and Constrained Delegation must be used in combination. Constrained Delegation is not possible using a ticket obtained by API Gateway when authenticating the client using Kerberos. An API Gateway policy can enforce the authentication of the client to API Gateway to use Kerberos authentication. However, API Gateway does not support forcing this within Active Directory. A policy that forces the incoming authentication to use Kerberos authentication still does both Protocol Transition and Constrained Delegation.

In addition to constrained delegation, API Gateway also supports *unconstrained* or *open* credentials delegation. Constrained delegation is considered to be more secure than unconstrained delegation because the KDC administrator can constrain the set of back-end services that the trusted Kerberos service can request tickets for as the end user they are impersonating. Restricting the delegation reduces the number of potential targets for attacks, so that if one part of the system is compromised, the whole system is not. In unconstrained delegation, the Kerberos service ticket can be requested for any valid service. For more details, see [API Gateway in unconstrained credentials delegation](/docs/apigtw_kerberos/kerberos_use_case_ucd).

## Prerequisites

Before you start configuration, you must have API Gateway installed on any machine with access to the Windows Domain Controller. The machine does *not* have to be a Windows machine that is part of the Windows Domain.

## Example names

For the example in this section, the trusted Kerberos principal `TrustedAPIGateway` can impersonate valid users in Active Directory and request service tickets in their name to the back-end service principals `HTTP/BackEndService.axway.com@AXWAY.COM` and `HOST/BackEndService.axway.com@AXWAY.COM`.

The example Kerberos realm name `AXWAY.COM` is specific to the examples in this guide. Replace the example realm name with your own realm name.

The next sections describe the steps to configure the gateway in constrained credentials delegation.

## Configure Active Directory

This section describes how to configure a trusted Kerberos client principal for API Gateway in Active Directory acting as the Key Distribution Centre (KDC) .

Before you configure the user account for the trusted Kerberos principal, you must have configured the user account and Service Principal Names (SPN) for the back-end services you want API Gateway to request service tickets for. For an example configuration, see [Configure a user account for the Kerberos service](/docs/apigtw_kerberos/kerberos_use_case_demo#configure-a-user-account-for-the-kerberos-service).

### Configure user account for the trusted Kerberos principal

1. On the Windows Domain Controller, click **Control panel > Administrative Tools > Active Directory User and Computers**
2. Right-click **Users**, and select **New > User**.
3. Enter a name for the Kerberos principal (`TrustedAPIGateway`) in the **First Name** and **User Logon Name** fields, select your Active Directory domain from the drop-down menu (`@axway.com`), and click **Next**.
4. Enter the password, and do the following:
    * **User must change password at next logon**: Deselect this.
    * **User cannot change password**: Select this.
    * **Password never expires**: Select this.

    This ensures that a working API Gateway configuration does not stop working when a user chooses, or is prompted to change their password. API Gateway does not track these actions.

    If these options are not suitable in your implementation and a user password changes in Active Directory, you must then update the password or keytab of the Kerberos client or service related to the user in Policy Studio, and redeploy the configuration to API Gateway.

    If you cannot deselect **User must change password at next logon**, ensure the user changes the password and that the new password or keytab is deployed to API Gateway *before* API Gateway attempts to connect as this user.

    You can store Kerberos passwords in a KPS table to update a changed password in runtime. For more details, see [Use KPS to store passwords for Kerberos authentication](/docs/apigtw_kerberos/kerberos_kps#use-kps-to-store-passwords-for-kerberos-authentication).

5. Click **Next > Finish**.
6. Open a command prompt on the Windows Domain Controller, and enter the following command to set the Service Principal Name (SPN):

    ```
    setspn -A <service class>/<host> <service name>
    ```

    For example:

    ```
    setspn -A HTTP/TrustedAPIGateway.axway.com TrustedAPIGateway
    ```

    This command creates the SPN, but does not create a keytab file.

7. Right-click on the new user, and select **Properties > Delegation**, and select **Trust this user for delegation to specified services only** and **Use any authentication protocol**. API Gateway does not support the option **Use Kerberos only**.
8. Add the back-end services (here `HTTP/BackEndService.axway.com` and `HOST/BackEndService.axway.com`), then click **OK**. The trusted Kerberos principal can request service tickets for these back-end services on behalf of the impersonated end users.

    ![Overview](/Images/IntegrationGuides/KerberosIntegration/KerberosConstrainedDelegation/Overview_4.png)

## Configure Kerberos principals

This section describes how to add Kerberos principals for the end user, trusted Kerberos principal, and back-end Kerberos service for KCD using Policy Studio.

1. In the node tree, click **Environment Configuration > External Connections > Kerberos Principals**.
2. Add a new Kerberos principal for the end user:
    * **Name**: `End User Principal to Impersonate in KCD`
    * **Principal Name**: `${authentication.subject.id}@AXWAY.COM`
    * **Principal Type**: `NT_USER_NAME`

    Using a selector here enables you to impersonate multiple end users.
3. Add a new Kerberos principal for the trusted Kerberos principal account as follows:
    * **Name**: `TrustedAPIGateway for KCD`
    * **Principal Name**: `TrustedAPIGateway@AXWAY.COM`
    * **Principal Type**: `NT_USER_NAME`

4. Add a new Kerberos principal for the back-end service account as follows:

    * **Name**: `<Back-end service name>` (for example, `Back-end Kerberos Service`)
    * **Principal Name**: `<Service Principal Name for the back-end service>` (for example, `HOST/BackEndService.axway.com@AXWAY.COM`)
    * **Principal Type**: `NT_USER_NAME`

## Configure API Gateway policy

This section describes how to configure API Gateway for the KCD using Policy Studio.

### Configure the Kerberos client

Although the trusted Kerberos principal can be referred to as a Kerberos service principal, it is acting on the client-side of the Kerberos authentication transaction, and needs a Kerberos client for KCD.

1. In the node tree, click **Environment Configuration > External Connections > Kerberos Clients**.
2. Click **Add a Kerberos Client**, and enter a name for your client (`Trusted Kerberos Client for KCD`).
3. On the **Kerberos Endpoint** tab, set the following:
    * **Load via JAAS Login**: Select this option and the **Request from KDC** option.
    * **Kerberos Principal**: `TrustedAPIGateway for KCD`.
    * **Enter Password**: Enter the password for `TrustedAPIGateway@AXWAY.COM`.
    * **Enabled**: Ensure this option is selected.

4. On the **Kerberos Constrained Delegation** tab, set the following:
    * **Kerberos Principal to Impersonate**: `End User Principal to Impersonate in KCD`
    * **Select Cache for Impersonated Subjects**: `Kerberos Constrained Delegation Impersonated Subject Cache`

5. On the **Advanced** tab, set the following:
    * **Mechanism**: `SPNEGO_MECHANISM`.
    * **Context Settings**: Select the following options:
        * **Mutual authentication**
        * **Integrity**
        * **Confidentiality**
        * **Anonymity**
        * **Replay Detection**
        * **Sequence Checking**
    * **Synchronize to Avoid Replay Errors at Service**: Deselect this option to improve performance.
    * **Refresh when remaining validity is `<value>` seconds**: Set to `300`.

### Configure a Kerberos profile for the Kerberos client

1. In the node tree, click **Environment Configuration > External Connections > Client Credentials > Kerberos**.
2. Add a Kerberos profile as follows:
    * **Profile Name**: `Authenticate to BackEndService using KCD`.
    * **Kerberos Client**: `Trusted Kerberos Client for KCD`.
    * **Kerberos Service Principal**: `<Back-end Kerberos Service>`.
    * **Send token with first request**: Select this option.

### Configure a Kerberos policy

The following section describes how to configure the Kerberos policy for KCD.

To start, add a new policy named, for example, `Kerberos KCD SPNEGO Client-Side`.

#### Configure the end user authentication method

1. Configure the authentication mechanism the end user application requires. The required filters and configuration details depend on the type of authentication. For an example configuration, see [Configure a KCD demo setup](/docs/apigtw_kerberos/kerberos_use_case_kcd#configure-a-kcd-demo-setup).
2. Right-click the first filter in your policy, and select **Set as Start**.

#### Configure connection to the back-end service

1. Open the **Routing** category in the filter palette, and drag a **Connect to URL** filter onto the policy canvas.
2. Enter the **URL** used to invoke the back-end service.
3. On the **Authentication** tab, select the Kerberos credential profile configured earlier (`Authenticate to BackEndService using KCD`), and click **Finish**.

#### Build the policy

1. Click the **Add Relative Path** icon to create a new relative path `/kcd` that links to this Kerberos client-side policy.
2. Connect the filters with a success path.

    ![Overview](/Images/IntegrationGuides/KerberosIntegration/KerberosConstrainedDelegation/Overview_6.png)

    The policy has the following flow:

    * API Gateway authenticates the end user using a non-Kerberos authentication mechanism, and sets the message attribute `authentication.subject.id` to the user name of the end user.
    * API Gateway generates a Kerberos principal name for the end user using the selector `${authentication.subject.id}@AXWAY.COM`.
    * API Gateway checks the cache of impersonated subjects for valid credentials for the end user Kerberos principal name.
    * If valid credentials are not found, API Gateway impersonates the end user, and sends a service ticket request in the name of the end user to the KDC.
    * API Gateway sends the Kerberos token containing the service ticket in the name of the end user to the back-end Kerberos service.
    * The back-end Kerberos service authenticates the end user and returns its response.

### Configure the Kerberos system settings

1. In the node tree, click **Environment Configuration > Server Settings > Security > Kerberos**, and click **Add Kerberos Configuration**.
2. On the **krb5.conf** tab, add the Kerberos settings as follows:

    ```
    [libdefaults]
    default_realm = AXWAY.COM
    renewable=true
    proxiable=true
    forwardable=true

    [realms]
    AXWAY.COM = {
    kdc = dc.axway.com
    }
    ```

    For KCD, the setting `forwardable` must be `true`.

    Replace the realm settings in the example with your Kerberos realm, and set the `kdc` setting to the host name of your Windows Domain Controller.

    For more details on the fields and options in this configuration window, see [Kerberos configuration](/docs/apim_policydev/apigw_poldev/security_server_settings/#configure-kerberos-settings).

3. Click the **Deploy** icon to deploy the configuration to API Gateway.

You have now configured and deployed a simple KCD policy for SPNEGO authentication where API Gateway acts as the trusted Kerberos principal for KCD. The end user application that invokes this policy in API Gateway must provide authentication credentials to satisfy the chosen non-Kerberos authentication mechanism.

For demonstration purposes, you can add API Gateway as the back-end service as well as sample users. See [Configure a KCD demo setup](#configure-a-kcd-demo-setup).

### Test the configuration

Use a client application to call the KCD policy in API Gateway.

The back-end Kerberos service should send a confirmation on a successful authentication.

The **Traffic Monitor** tab on the API Gateway Manager (`https://localhost:8090`) is an excellent place to view and troubleshoot the message flows. For more details, see [Monitor services in API Gateway Manager](/docs/apim_administration/apigtw_admin/monitor_service/#monitor-services-in-api-gateway-manager).

## Configure a KCD demo setup

Follow the next steps to configure a test back-end service and sample users to test or demonstrate the KCD.

### Configure a back-end service for testing

For demonstration purposes, you can use another API Gateway instance as the back-end Kerberos service. API Gateway is configured as the Kerberos service for the most part the same way for both KCD and standard Kerberos authentication in the client-side transaction. For more details, see [Configure API Gateway to act as the Kerberos service](/docs/apigtw_kerberos/kerberos_use_case_service/).

The difference between KCD and standard SPNEGO configuration is that for KCD, the back-end service must have a Service Principal Name (SPN). For more details, see [Map an SPN to the user account](/docs/apigtw_kerberos/kerberos_use_case_service/#map-an-spn-to-the-user-account).

### Configure sample authentication

For demonstration purposes, you can configure HTTP Basic authentication against a local user repository as the incoming authentication mechanism on API Gateway for the end user.

#### Configure sample users

You can quickly add some sample users to a local repository in Policy Studio.

The user identity in the local repository must be mappable to an end user Kerberos principal name, so that when the trusted Kerberos principal impersonates an end user, the original end user can be identified in Active Directory. The setup in this guide uses a selector expression `${authentication.subject.id}@AXWAY.COM` for the mapping. For more details, see [Configure Kerberos principals](#configure-kerberos-principals).

For example, if your end user Kerberos principal names were `Bob@AXWAY.COM`, and `Bill@AWAY.COM`, then add users named Bob and Bill to the local user repository.

1. In the node tree, click **Environment Configuration > Users and Groups > Users**.
2. Click **Add**, and fill in the details for your user. For example:

    | Bob                  | Bill                 |
    |----------------------|----------------------|
    | User Name: `Bob`     | User Name: `Bill`    |
    | Password: `changeme` | Password: `changeme` |

The passwords in the local user repository do *not* need to match these users' Kerberos passwords in the Key Distribution Center. Here, the user names and passwords configured in the local repository are passed to API Gateway over HTTP Basic.

#### Configure HTTP Basic authentication

In this example, API Gateway authenticates the end users using HTTP Basic.

1. Open the **Authentication** category, and drag a **HTTP Basic** filter onto the policy canvas.
2. Set **Credential Format** to **User Name**, and select **Allow client challenge**.
3. Set **Repository Name** to **Local User Store**, and click **Finish**.
4. Right-click the **HTTP Basic** filter, and select **Set as Start**.
5. Connect the filters with a success path.

    ![Demo policy](/Images/IntegrationGuides/KerberosIntegration/KerberosConstrainedDelegation/demo_policy.png)

To test the configuration, see [Test the policies](#test-the-configuration).
