{
"title": "API Gateway as both Kerberos client and service",
"linkTitle": "API Gateway as both Kerberos client and service",
"weight":"4",
"date": "2019-11-14",
"description": "Configure API Gateway to act both as Kerberos client and Kerberos service."
}

For demonstration purposes, or to test configuring Kerberos authentication, you can configure API Gateway to act both as Kerberos client (`DemoClient`) and Kerberos service (`DemoService`). This configuration is not suitable for production environment.

This is the most straight-forward setup to get started with Kerberos authentication in API Gateway. You configure API Gateway to act as a Kerberos client and authenticate to API Gateway that acts as a Kerberos service.

You can do this configuration using a single gateway instance, or two gateway instances in different groups. The example in this guide uses a single gateway instance.

The Kerberos client and service principals do not use selectors, so the same client principal (`DemoClient@AXWAY.COM`) always authenticates to the same service principal (`DemoService@AXWAY.COM`).

![Kerberos_use_case_demo](/Images/IntegrationGuides/KerberosIntegration/kerb_use_case_demo/Kerberos_use_case_demo.png)

## Prerequisites

Before you start configuration, you must have API Gateway installed on any machine with access to the Windows Domain Controller. The machine does *not* have to be a Windows machine that is part of the Windows Domain.

## Example names

In this example, the Kerberos client `DemoClient@AXWAY.COM` connects to the Kerberos service `DemoService@AXWAY.COM`.

The example Kerberos realm name `AXWAY.COM` is specific to the examples in this guide. Replace the example realm name with your own realm name.

The next sections describe how to configure the gateway as both Kerberos client and service.

## Configure Active Directory

This section describes how to configure a Kerberos client principal and Kerberos service principal in Active Directory acting as the Key Distribution Center (KDC). The principals in the KDC are used when configuring the Kerberos principals in Policy Studio.

### Configure a user account for the Kerberos client

Configure a user account for the Kerberos client principal. In this example, the client principal is `DemoClient@AXWAY.COM`.

1. On the Windows Domain Controller, click **Control panel > Administrative Tools > Active Directory User and Computers**.
2. Right-click **Users**, and select **New > User**.
3. Enter a name (such as `DemoClient`) in the **First Name** and **User Logon Name** fields, ensure the Active Directory domain is set to your domain, and click **Next**.
4. Enter the password, and do the following:
    * **User must change password at next logon**: Deselect this.
    * **User cannot change password**: Select this.
    * **Password never expires**: Select this.

    This ensures that a working API Gateway configuration does not stop working when a user chooses, or is prompted to change their password. API Gateway does not track these actions.

    If these options are not suitable in your implementation and a user password changes in Active Directory, you must then update the password or keytab of the Kerberos client or service related to the user in Policy Studio, and redeploy the configuration to API Gateway.

    If you cannot deselect **User must change password at next logon**, ensure the user changes the password and that the new password or keytab is deployed to API Gateway *before* API Gateway attempts to connect as this user.

    You can store Kerberos passwords in a KPS table to update a changed password in runtime. For more details, see [Use KPS to store passwords for Kerberos authentication](/docs/apigtw_kerberos/kerberos_kps#use-kps-to-store-passwords-for-kerberos-authentication).

5. Click **Next > Finish**.

### Configure a user account for the Kerberos service

1. Configure a user account for the Kerberos service as in [Configure a user account for the Kerberos client](#configure-a-user-account-for-the-kerberos-client). In this example, the name of the service is `DemoService@AXWAY.COM`.
2. Map a Service Principal Name (SPN) to the user account. The Kerberos client uses the SPN to uniquely identify a service. To map the SPN, open a command prompt on the Windows Domain Controller, and enter the following command:

    ```
    ktpass -princ HTTP/<host>@<Kerberos realm> -mapuser <user> -pass password -out <user>.keytab -crypto rc4-hmac-nt -kvno 0
    ```

    The SPN is of the format `HTTP/<host>@<Kerberos realm>`, where `<host>` is the name of the host running the Kerberos service, DemoService in this case:

    ```
    ktpass -princ HTTP/DemoService.axway.com@AXWAY.COM -mapuser DemoService -pass Axway123 -out DemoService.keytab -crypto rc4-hmac-nt -kvno 0
    ```

    Replace the example realm name with your own domain name. Note that the realm name is uppercase.

The command creates an SPN `HTTP/DemoService.axway.com@AXWAY.COM`, which is mapped to the `DemoService` user account. The command also creates a keytab file for the account that you can use later when configuring a Kerberos service for your gateway in Policy Studio.

If you do not want to create a keytab file, you can use the following command:

```
setspn -A HTTP/
```

## Configure Kerberos principals

This section describes how to add Kerberos principals for the Kerberos client and Kerberos service using Policy Studio.

1. In the node tree, click **Environment Configuration > External Connections > Kerberos Principals**.
2. Add a new Kerberos principal for the Kerberos client account (`DemoClient@AXWAY.COM`) as follows:
    * **Name**: `DemoClient`
    * **Principal Name**: `DemoClient@AXWAY.COM`
    * **Principal Type**: `NT_USER_NAME`
3. Add a new Kerberos principal for the service account (`DemoService`) as follows:
    * **Name**: `DemoService`
    * **Principal Name**: `DemoService@AXWAY.COM`
    * **Principal Type**: `NT_USER_NAME`

## Configure API Gateway to act as the Kerberos client

This section describes how to configure API Gateway to act as a Kerberos client (`DemoClient@AXWAY.COM`) in Policy Studio.

### Configure a Kerberos client

1. In the node tree, click **Environment Configuration > External Connections > Kerberos Clients**.
2. Click **Add a Kerberos Client**, and enter a name for your client (`DemoClient Kerberos Client`).
3. On the **Kerberos Endpoint** tab, set the following:
    * **Load via JAAS Login**: Select this option and the **Request from KDC** option.
    * **Kerberos Principal**: `DemoClient`.
    * **Enter Password**: Enter the password.
    * **Enabled**: Make sure this option is selected.
4. On the **Advanced** tab, set the following:
    * **Mechanism**: `SPNEGO_MECHANISM`.
    * **Context Settings**: Select the following options:
        * **Mutual authentication**
        * **Integrity**
        * **Confidentiality**
        * **Anonymity**
        * **Replay Detection**
        * **Sequence Checking**
    * **Synchronize to Avoid Replay Errors at Service**: Deselect this option to improve performance.

### Configure a Kerberos profile for the Kerberos client

1. In the node tree, click **Environment Configuration > External Connections > Client Credentials > Kerberos**.
2. Add a Kerberos profile as follows:
    * **Profile Name**: `Authenticate to DemoService`.
    * **Kerberos Client**: `DemoClient Kerberos Client`.
    * **Kerberos Service Principal**: `DemoService`.
    * **Send token with first request**: Select this option.

### Configure a client-side policy

1. Add a new policy named, for example, `Kerberos Demo Client-Side`.
2. Open the **Routing** category in the filter palette, and drag a **Connect to URL** filter onto the policy canvas.
3. Enter the **URL** used to invoke the Kerberos service-side policy in the Kerberos service. In this example, `DemoClient@AXWAY.COM` calls out and back to the same gateway instance on `http://localhost:8080/service` to call `DemoService@AXWAY.COM`.
4. On the **Authentication** tab, choose the Kerberos credential profile configured earlier (`Authenticate to DemoService`), and click **Finish**.
5. Right-click the **Connect to URL** filter, and select **Set as Start**.
6. Click on the **Add Relative Path** icon to create a new relative path `/client` that links to this Kerberos client-side policy.

The policy looks like this:

![Policy with "Connect to URL" filter](/Images/IntegrationGuides/KerberosIntegration/kerb_use_case_demo/demo_kerb_client_paths.png)

The client-side policy has the following flow:

* Send a request with a SPNEGO Kerberos token to the Kerberos service on URL `http://localhost:8080/service`.
* Pass the response from Kerberos service back to the calling application.

### Configure Kerberos system settings

1. In the node tree, click **Environment Configuration > Server Settings > Security > Kerberos**, and click **Add Kerberos Configuration**.
2. On the **krb5.conf** tab, change the following settings:

    ```
    [libdefaults]
    default_realm = AXWAY.COM
    [realms]
    AXWAY.COM = {
    kdc = dc.axway.com
    }
    ```

    Replace the realm settings in the example code with your Kerberos realm, and set the `kdc` setting to the host name of your Windows Domain Controller.

3. Click the **Deploy** icon to deploy the configuration to API Gateway.

You have configured and deployed a simple client-side policy for Kerberos authentication using SPNEGO.

## Configure API Gateway to act as the Kerberos service

This section describes how to configure API Gateway to act as a Kerberos client (`DemoService@AXWAY.COM`) in Policy Studio.

### Configure a Kerberos service

1. In the node tree, click **Environment Configuration > External Connections > Kerberos Services**.
2. Click **Add a Kerberos Service**, and enter a name for your service (`DemoService Kerberos Service`).
3. On the **Kerberos Endpoint** tab, set the following:
    * **Kerberos Principal**:`DemoService@AXWAY.COM`.
    * **Enter Password**: Enter the password you configured for the user account in Active Directory.
    * **Enabled**: Select this option.
4. On the **Advanced** tab, set **Mechanism** to `SPNEGO_MECHANISM`, and click **OK**.

### Configure a service-side policy

1. Add a new Policy named, for example, `Kerberos Demo Service-Side`.
2. Open the **Authentication** category in the filter palette, and drag a **Kerberos Service** filter onto the policy canvas.
3. Set **Kerberos Service** to the Kerberos service you created (`DemoService Kerberos Service`), change **Kerberos Standard** to **SPNEGO Over HTTP**, and click **Finish**.\
4. Right-click the **Kerberos Service** filter, and select **Set as Start**.
5. Open the **Conversion** category in the palette, and drag a **Set Message** filter onto the policy canvas.
6. Set **Content type** as `text/xml`, copy the following code to **Message Body**, and click **Finish**:

    ```
    <Response>Kerberos client '${authentication.subject.id} authenticated' successfully.</Response>
    ```

7. Open the **Utility** category in the palette, and drag a **Reflect Message** filter onto the policy canvas.
8. Click **Add Relative Path**, and create a new relative path `/service` that links to this Kerberos service-side policy.
9. Connect the filters with success paths.

    ![Service-side policy with Kerberos Service, Set Message, and Reflect Message filters](/Images/IntegrationGuides/KerberosIntegration/kerb_use_case_demo/demo_kerb_svc_paths.png)

    The policy has the following flow:

    * Authenticate the client.
    * Return a response with a HTTP status `200` if the authentication passes.

10. Click the **Deploy** icon to deploy the configuration to your Kerberos service.

You have now configured a simple service-side policy for SPNEGO authentication. The Kerberos client invokes this policy on `http://localhost:8080/service`.

## Test the policies

Use a third-party client application, such as Postman, to call the client-side policy in API Gateway.

The response should be:

```
<Response>
Kerberos client 'DemoClient@AXWAY.COM authenticated' successfully.
</Response>
```

The **Traffic Monitor** tab on the API Gateway Manager (`https://localhost:8090`) is an excellent place to view and troubleshoot the message flows. For more details, see [Monitor services in API Gateway Manager](/docs/apim_administration/apigtw_admin/monitor_service/#monitor-services-in-api-gateway-manager).
