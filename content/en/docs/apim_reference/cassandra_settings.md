{
"title": "Cassandra settings in Policy Studio",
"linkTitle": "Cassandra settings",
"weight":"50",
"date": "2019-10-14",
"description": "Configure settings for the external Apache Cassandra database in Policy Studio."
}

The Cassandra settings in Policy Studio enable you to configure settings for the external Apache Cassandra database used to store API Gateway and API Manager data. For example, you can use a Cassandra database to store data used by the following components:

* API Manager: Client registry, API catalog, and quota management
* Key Property Store: Custom table definitions and data
* OAuth token store
* Client registry: API key and OAuth solutions using API Gateway only
* Throttling: Smooth rate limiting

{{< alert title="Note" color="primary" >}}You do not need to configure Cassandra settings if you do not use Apache Cassandra for any of these features.{{< /alert >}}

To configure Cassandra settings, select the **Server Settings > Cassandra** node in the Policy Studio tree. To apply updates to these settings, click **Apply changes** at the bottom right of each page.

For more details on installing and configuring an Apache Cassandra database cluster, see [Install an Apache Cassandra database](/docs/apim_installation/apigtw_install/cassandra_install/).

## Cassandra Keyspace settings

Configure the following in the **Keyspace** settings:

**Keyspace name**:

Specifies the Cassandra keyspace used to store tables and data.

The keyspace name must be unique and less than 48 permitted characters. These include underscore (`_`) and alphanumeric characters (`a-z`,`A-Z`, `0-9`).

This is a unique namespace used to define data replication on cluster nodes. Defaults to `x${DOMAIN_ID}_${GROUP_ID}`. You can enter text, environment variables, and the following API Gateway-specific variables:

* `${DOMAINID}`: Topology ID of the API Gateway system
* `${GROUPID}`: API Gateway group ID
* `${GROUPNAME}`: API Gateway group name

**Initial replication strategy**:

Specifies the initial Cassandra replication strategy used to determine the nodes where replicas are placed:

* **Simple Strategy**: Use this setting for a single data center and one rack only. This is the default setting. If you ever intend more than one datacenter, use the Network Topology Strategy instead.
* **Network Topology Strategy**: This setting is recommended for most deployment because it is much easier to expand to multiple datacenters when required by future expansion.

**Initial replication**:

Specifies the initial Cassandra replication factor used to create the keyspace only. Defaults to `1`. Then to modify the replication factor after the keyspace is created, use the `cqlsh` command in `CASSANDRA_INSTALL_DIR/cassandra/bin`.

On startup, if the keyspace does not already exist, the keyspace is updated with this value. This is the only time this value is used. If you need to update the replication factor, you must use `cqlsh` (located in `cassandra/bin`). When this value is changed, you must run the `node repair` command on all nodes in the Cassandra cluster. For more details, see your Cassandra documentation.

## Cassandra Hosts settings

To add a Cassandra server host in the **Hosts** settings, click **Add** at the bottom right, and configure the following required settings in the dialog:

* **Name**: Enter a user-friendly name for the Cassandra server host. Defaults to `Local Cassandra server`.
* **Host**: Enter the host name or IP address of the Cassandra server host. Defaults to `localhost`.
* **Port**: Enter the port on which the API Gateway client listens on the Cassandra native protocol. Defaults to `9042`.

## Cassandra Authentication settings

If Cassandra authentication is enabled, configure the following in the **Authentication** settings:

* **Username**: Enter the user name to used to establish a connection with Cassandra. This field can be environmentalized. The user name can be formatted as follows:
    * Unquoted string containing only alphanumeric characters
    * Single-quoted string containing any printable character (including non-alphanumeric)
* **Password**: Enter the password to establish a connection with Cassandra. This field can be environmentalized.

* Cassandra authentication is disabled by default. For details on enabling Cassandra authentication, see [Internal authentication documentation](https://docs.datastax.com/en/cassandra-oss/3.x/cassandra/configuration/secureInternalAuthenticationTOC.html?hl=internal%2Cauthentication).

## Cassandra Security settings

Configure the **Enable SSL** setting in the **Security** settings:

Select whether to use Secure Sockets Layer (SSL) to establish secured connections to the Cassandra server. This option is deselected by default. When **Enable SSL** is selected, you can configure the following:

* **Trusted certificates**:

Click to select the list of certificates or certificate authorities trusted when validating Cassandra server certificates. This is required when SSL is enabled.

* **Client certificate**:

Click to select the client certificate and key to use if client authentication is required by the Cassandra server (also known as SSL mutual authentication).

* **Accepted cipher suites**:

Select which cipher suites are only allowed when establishing SSL connections to the Cassandra server. This setting is optional. For details on the default cipher suites, see the [SunJSSE Provider documentation](http://docs.oracle.com/javase/8/docs/technotes/guides/security/SunProviders.html).

* **Do not use the SSLv3 protocol**:

Specifies not to use SSL v3 to avoid any weaknesses in this protocol. This option is selected by default (SSLv3 is not supported by Cassandra 2.x and above). This option is provided for backward compatibility when connecting to legacy Cassandra installations.

* **Do not use the TLSv1 protocol**:

Specifies not to use TLS v1 to avoid any weaknesses in this protocol. TLSv1 is supported by Cassandra 2.x.

* **Do not use the TLSv1.1 protocol**:

Specifies not to use TLS v1.1 to avoid any weaknesses in this protocol. TLSv1.1 is supported by Cassandra 2.x.

* **Do not use the TLSv1.2 protocol**:

Specifies not to use TLS v1.2 to avoid any weaknesses in this protocol. TLSv1.2 is supported by Cassandra 2.x. The default protocol is TLS v1.2.

## Throttling settings

Configure the following in the **Throttling** settings:

**Keyspace name**:

Specifies the Cassandra keyspace used to store tables for throttling and smooth rate limit data.

The keyspace name must be unique and less than 48 permitted characters. These include underscore (`_`) and alphanumeric characters (`a-z`,`A-Z`,`0-9`). In addition, one throttling keyspace per group is recommended in a multi-group domain that shares the same Cassandra cluster.

This is a unique namespace used to define data replication on cluster nodes. Defaults to `t${DOMAIN_ID}_throttling`. You can enter text, environment variables, and the following API Gateway-specific variables:

* `${DOMAINID}`: Topology ID of the API Gateway system
* `${GROUPID}`: API Gateway group ID
* `${GROUPNAME}`: API Gateway group name

**Initial replication strategy**:

Specifies the initial Cassandra replication strategy used to determine the nodes where replicas are placed:

* **Simple Strategy**: Use this setting for a single data center and one rack only. This is the default setting. If you ever intend more than one datacenter, use the Network Topology Strategy instead.
* **Network Topology Strategy**: This setting is recommended for most deployments because it is much easier to expand to multiple datacenters when required by future expansion.

**Initial replication**:

Specifies the initial Cassandra replication factor used to create the throttling keyspace only. Defaults to `1`. Then to modify the replication factor after the keyspace is created, use the `cqlsh` command in `CASSANDRA_INSTALL_DIR/cassandra/bin`.

* On startup, if the throttling keyspace does not already exist, the keyspace is updated with this value. This is the only time this value is used. If you need to update the replication factor, you must use `cqlsh` (located in `cassandra/bin`). When this value is changed, you must run the `node repair` command on all nodes in the Cassandra cluster. For more details, see your Cassandra documentation.

**Read consistency level**:

Select the consistency level for Cassandra read operations from the list. Defaults to `ONE`.

**Write consistency level**:

Select the consistency level for Cassandra write operations from the list. Defaults to `ONE`.

For more details on Cassandra consistency levels, see the [Consistency level documentation](https://docs.datastax.com/en/cassandra-oss/3.x/cassandra/dml/dmlConfigConsistency.html).
