{
"title": "Embedded ActiveMQ settings in Policy Studio",
"linkTitle": "Embedded ActiveMQ settings",
"weight":"90",
"date": "2019-10-14",
"description": "Configure settings for the Apache ActiveMQ messaging broker in Policy Studio."
}

The **Embedded ActiveMQ** settings enable you to configure settings for the Apache ActiveMQ messaging broker that is embedded in each API Gateway instance. You can also configure multiple embedded ActiveMQ brokers to work together as a network of brokers in a group of API Gateway instances. For more details, see [Apache ActiveMQ](http://activemq.apache.org/) documentation.

## Configure Embedded ActiveMQ settings

Apache ActiveMQ 5.14.3 restricts serializing object message types. To allow the serialization, you must add the impacted packages to the system property in the `jvm.xml` file.

To allow serialization globally on all API Gateway instances, add the following to `INSTALL_DIR/system/conf/jvm.xml`:

```
<VMArg name="-Dorg.apache.activemq.SERIALIZABLE\_PACKAGES=<list of packages>"/>
```

Use comma to separate the packages. For example:

```
<VMArg name="-Dorg.apache.activemq.SERIALIZABLE\_PACKAGES=myorg.data,myorg2.data2">
```

To allow serialization on a particularAPI Gateway instance, add the above code to `INSTALL_DIR/groups/<group name>/<instance name>/conf/jvm.xml.`

To configure embedded ActiveMQ settings, select the **Server Settings** node in the Policy Studio tree, and click **Messaging > Embedded ActiveMQ**. To apply updates to these settings, click **Save** at the bottom right of the window.

### General messaging settings

Configure the following ActiveMQ messaging settings:

**Enable Embedded ActiveMQ Broker**:

Specifies whether to enable starting up the ActiveMQ broker that is embedded in the API Gateway instance. This is not selected by default.

**Address**:

Specifies the IP address used to open a listening socket for incoming ActiveMQ connections. Defaults to `0.0.0.0`, which specifies that all interface addresses should be used.

**Port**:

Specifies the TCP port for incoming ActiveMQ connections. Defaults to `${env.BROKER.PORT}`, which enables the port number to be environmentalized. This means that the port number is specified in the `envSettings.props` file on a per-server basis. Alternatively, you can enter the port number directly in this field (for example, `61616`).

**Shared Directory**:

Specifies the location of the shared directory in your environment that is used by multiple embedded ActiveMQ brokers. This setting is required, and must be configured for high availability and failover. Defaults to `INSTALL_DIR/messaging-shared`.

When setting up a shared directory, do not use OCFS2, NFSv3, or other software where exclusive file locks do not work reliably. For details, see [Apache ActiveMQ Shared File System Master Slave](http://activemq.apache.org/shared-file-system-master-slave.html).

### SSL settings

Configure the following settings to secure the communication with JMS clients, and between multiple embedded ActiveMQ brokers:

**Enable SSL**:

Specifies whether to use Secure Sockets Layer (SSL) to secure the communication with JMS clients and between ActiveMQ brokers.

**Server Cert**:

When **Enable SSL** is selected, click to select the server certificate with a private key that is used for SSL communication between ActiveMQ brokers. For details on importing certificates into the certificate store, see [Manage X.509 certificates and keys](/docs/apim_administration/apigtw_admin/general_certificates).

**Accepted cipher suites**:

When **Enable SSL** is selected, select which cipher suites should be accepted by the JMS server when the SSL communication is being established. If no cipher suites are selected, the default cipher suites from the Java Security Socket Extension (JSSE) are used.

**Require Client Certificates**:

When **Enable SSL** is set, click to require client certificates for client SSL authentication. For example, for mutual (two-way) SSL communication, you must trust the issuer of the client certificate by importing the client certificate issuer into the certificate store. For details on importing certificates, see [Manage X.509 certificates and keys](/docs/apim_administration/apigtw_admin/general_certificates).

When **Require client certificates** is selected, you can then select the root certificate authorities that are trusted for mutual (two-way) SSL communication between ActiveMQ brokers. For details on importing certificates into the API Gateway certificate store, see [Manage X.509 certificates and keys](/docs/apim_administration/apigtw_admin/general_certificates).

**Enable Hostname Verification**:

When **Enable SSL** is set, click to execute hostname verification between all JMS clients and embedded ActiveMQ brokers. The X509 Certificate used must have the **Subject Alternative Names** (SANs) extension set with `localhost`, `127.0.0.1`, a fully qualified domain name and IP address for each gateway in the group.

Policy Studio does not enable certificate creation with SANs, but you can create certificates using third-party services or tools for example, OpenSSL, then import and deploy them with Policy Studio. For details on importing certificates, see [Manage X.509 certificates and keys](/docs/apim_administration/apigtw_admin/general_certificates).

{{% alert title="Caution" color="warning" %}}
Disabling hostname verification is a less secure option than using a certificate with SANs populated.
{{% /alert %}}

### Authentication settings

Configure the following to specify authentication settings between multiple embedded ActiveMQ brokers:

* The authentication settings are also used by features on the **Messaging** tab in the API Gateway Manager web console (for example, sending messages and managing durable topic subscriptions). For more details, see [Manage embedded ActiveMQ messaging](/docs/apim_administration/apigtw_admin/admin_messaging).

**Authenticate broker and client connections with the following policy**:

When user name and password credentials are provided for inter-broker communication, they are delegated to the selected policy for authentication. By default, no policy is selected. To select a policy, click the button on the right, and select a preconfigured policy in the dialog.

**Username**:

Specifies the user name credential when connecting to other ActiveMQ brokers.

**Password**:

Specifies the password credential when connecting to other ActiveMQ brokers.

**Communicate with brokers in the same group**:

Every API Gateway instance belongs to a group. This setting specifies whether to communicate only with ActiveMQ brokers in the same API Gateway group. This is the default setting.

**Or brokers outside the group registered with the following alias**:

Specifies an alias name used to communicate with other ActiveMQ brokers registered with the same alias. This setting enables communication with ActiveMQ brokers that belong to different API Gateway groups.

### Advanced settings

Configure the following advanced settings:

**Maximum UI queue browsing size**:

Enter the maximum number of messages loaded into memory and returned to the API Gateway Manager UI when listing messages contained in a queue. For more information on viewing the messages in a queue, see [Manage messages in a queue](/docs/apim_administration/apigtw_admin/admin_messaging#manage-messages-in-a-queue).

**Maximum memory usage**:

Enter a value and select a unit (MiB or GiB) to specify the maximum amount of memory to use for non-persisted or temporary messages. The default value is 1 GiB.

**Maximum disk store usage**:

Enter a value and select a unit (MiB or GiB) to specify the maximum amount of disk space to use for persisted messages. The default value is 100 GiB.

**Maximum temporary disk usage**:

Enter a value and select a unit (MiB or GiB) to specify the maximum amount of disk space to use for temporary messages. The default value is 50 GiB.

**Enable report of memory and disk usage (when exceeding 50%)**:

Select or deselect this option to enable or disable reporting of memory and disk usage when usage exceeds 50 percent of the specified maximums. When this option is selected, memory and disk usage information is written to the API Gateway trace log. For example:

```
INFO  [3d1b:000000000000000000000000] ActiveMQ memory usage reached 50%
INFO  [3d1b:000000000000000000000000] ActiveMQ memory usage reached 60%
ERROR [3d1b:000000000000000000000000] ActiveMQ memory usage reached 70%
ERROR [3d1b:000000000000000000000000] ActiveMQ memory usage reached 80%
ERROR [3d1b:000000000000000000000000] ActiveMQ memory usage reached 90%
FATAL [3d1b:000000000000000000000000] ActiveMQ memory usage reached 100%
ERROR [3d1b:000000000000000000000000] ActiveMQ memory usage went down to 90%
ERROR [3d1b:000000000000000000000000] ActiveMQ memory usage went down to 80%
ERROR [3d1b:000000000000000000000000] ActiveMQ memory usage went down to 70%
INFO  [3d1b:000000000000000000000000] ActiveMQ memory usage went down to 60%
INFO  [3d1b:000000000000000000000000] ActiveMQ memory usage went down to 50%
INFO  [3d1b:000000000000000000000000] ActiveMQ memory usage went down to 40%
```
