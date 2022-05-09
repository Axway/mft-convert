{
"title": "Configure Kerberos services",
"linkTitle": "Configure Kerberos services",
"weight": 5,
"date": "2019-10-17",
"description": "Configure API Gateway to act as a Kerberos service."
}

API Gateway can act as a Kerberos service. A Kerberos client must obtain a Ticket Granting Ticket (TGT) to authenticate to the Kerberos service exposed by API Gateway. The Kerberos client must present the TGT to API Gateway to be successfully authenticated, and for their requests to be processed. The Kerberos service is responsible for consuming the TGT.

You can configure Kerberos services globally under the **Environment Configuration > External Connections**
in the node tree. To configure a Kerberos service, right-click **Kerberos Services**, and select **Add a Kerberos Service**. The following sections describe how to configure the various fields on the **Kerberos Service**
dialog.

You can select globally configured Kerberos services by name when configuring a **Kerberos Service**
filter. This filter is responsible for validating the TGTs consumed by the Kerberos service. To easily tell different Kerberos services apart, ensure you enter a descriptive name for the service in the **Name**
when configuring a Kerberos service.

Once finished, you can select the configured Kerberos service when configuring other Kerberos-related filters. Ensure the check box **Enabled**
at the bottom of the window is selected.

For more details on different Kerberos setups with API Gateway, see the [API Gateway Kerberos Integration Guide](/docs/apigtw_kerberos/).

## Kerberos endpoint settings

Configure the following fields on the **Kerberos Endpoint**
tab:

### Kerberos Principal

Select the name of the Kerberos principal you want to associate with API Gateway. A Kerberos client trying to authenticate to API Gateway *must*
present a TGT containing a matching Kerberos principal name to API Gateway.

To select which Kerberos principal to use, click the **...** button, and select a previously configured principal from the list. To add a Kerberos principal, right-click **Kerberos Principals**, and select **Add Kerberos Principal**. You can also add Kerberos principals under **Environment Configuration > External Connections** in the node tree. For more details, see [Configure Kerberos principals](/docs/apim_policydev/apigw_external_connections/common_client_credentials/#configure-kerberos-principals).

### Secret Key

Select this to specify the location of the Kerberos service's secret key that is used to decrypt TGTs received from Kerberos clients.

**Enter Password**:
You can enter a password to generate the Kerberos service's secret key. This key is originally created for a specific principal on the KDC.

**Wildcard Password**:
Enter a wildcard password.

**Keytab**:
Instead of generating a secret key for the Kerberos service in this configuration dialog, adding the Kerberos service to the KDC usually generates a keytab
file containing a mapping between the Kerberos principal name and that principal's secret key. You can load the keytab file to API Gateway configuration using the fields in this section.

To load the principal-to-key mappings into the table in the dialog, select **Load Keytab**
and browse to the existing keytab file. To add a new keytab entry, select **Add Principal**. To delete a keytab entry, select the entry in the table and click **Delete Entry**. To export the entire contents of the keytab table in the dialog, click **Export Keytab**.

{{< alert title="Note" color="primary" >}}By default, the contents of the keytab table – derived from a keytab file or manually entered – stored in clear-text form in the underlying configuration data in API Gateway. If necessary, the contents of the keytab table can be encrypted using a passphrase.{{< /alert >}}

When API Gateway starts, it writes the stored keytab contents to the `/conf/plugin/kerberos/keytabs/`
directory in your API Gateway installation. It is recommended to configure directory-based or file-based access control for this directory and its contents.

For more details on configuring the **Keytab Entry**
dialog, see the [Kerberos Keytab concepts](/docs/apim_policydev/apigw_external_connections/common_client_credentials/#kerberos-keytab-concepts).

**Load via Native GSS Library**:
If you have configured API Gateway to use Native GSS Library
on the instance-level **Kerberos Configuration**
settings, you must choose to load the Kerberos service's secret key from the preferred location. The native GSS library expects the Kerberos service's secret key to be in the default system keytab file. The location of this keytab file is specified in the `default_keytab_name`
setting in the `krb5.conf`
file that the native GSS library reads using the `KRB5_CONFIG`
environment variable. This keytab can contain keys for multiple Kerberos services.

## Advanced settings

Configure the following fields on the **Advanced**
tab:

**Mechanism**:
Select the mechanism used to establish a context between the Kerberos service and the Kerberos client. The Kerberos client must use the same mechanism.

**Extract Delegated Credentials**:
A Kerberos client can set an attribute on the context established with the Kerberos service to indicate the Kerberos service can act on behalf of the Kerberos client in subsequent communications. This enables a Kerberos service (like API Gateway) to assume the identity of the Kerberos client when communicating with a back-end Kerberos service. The Kerberos client's credentials are propagated to the back-end Kerberos service, not API Gateway's credentials. This is called *credential delegation*.

If you have configured a Kerberos client to enable delegating its credentials to a Kerberos service, select **Extract Delegated Credentials** to set the Kerberos service to extract the delegated credentials from the context it establishes with the Kerberos client. The credentials are stored in the `gss.delegated.credentials`
and `gss.delegated.credentials.client.name`
message attributes.

To forward the extracted delegated credentials to the back-end Kerberos service on behalf of the Kerberos client, configure the Kerberos settings on the **Kerberos Client**
filter, or use the **Connection**
filter from the Routing category. If using the **Connection** filter, make sure to select **Extract from delegated credentials**
on the **Kerberos Endpoint**
tab to retrieve the TGT from the extracted delegated credentials for the **Kerberos Client**
on the **Kerberos Authentication**
tab of the **Connection**
filter.

For more details, see [Configure Kerberos clients](/docs/apim_policydev/apigw_external_connections/common_client_credentials/#configure-kerberos-clients)
