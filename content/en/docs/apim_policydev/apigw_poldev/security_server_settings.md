{
"title": "Configure Oracle SSM and Kerberos settings",
"linkTitle": "Configure Oracle SSM and Kerberos settings",
"weight": 50,
"date": "2019-10-17",
"description": "Configure Oracle SSM and Kerberos settings."
}

## Configure Oracle Security Service Module settings (10g)

An Oracle Security Service Module (SSM) integrates a secured application (in this case, the API Gateway) with an Oracle Entitlements Server (OES) 10g so that security administration (for example, roles, resources, and policies) is delegated to the Oracle Entitlements Server 10g. An SSM must be installed on the machine hosting the application to be secured by the Oracle Entitlements Server 10g. The SSM runs in-process with the secured application, which improves performance and on-the-wire security.

In Policy Studio, select the **Environment Configuration** > **Server Settings**
node in the tree, and select **Security**> **Security Service Module**
in the right pane. The **Security Service Module**
settings enable you to configure the API Gateway to act as a Java SSM. For more details on Oracle Entitlements Server 10g and SSMs, see the [Oracle Entitlements Server](http://www.oracle.com/technetwork/middleware/oes/overview/index.html)
website.

{{< alert title="Note" color="primary" >}}Oracle SSM is required only for integration with Oracle OES 10g. Oracle SSM is not required for integration with Oracle OES 11g. OES 10g was previously known as BEA AquaLogic Enterprise Security (ALES). Some items, such as schema objects, paths, and so on, may still use the ALES name. {{< /alert >}}

### Prerequisites

Before configuring the settings on the **Security Service Module**
tab, you must perform the following prerequisite tasks.

#### Test the SSM installation

Because the API Gateway is running a Java SSM internally, it is recommended that the example Java SSM client that ships with the OES installation is set up and configured. This example can be found in the following directory:

```
/ales32-ssm/java-ssm/examples/JavaAPIExample
```

Follow the instructions in the README file in this directory to test the installation. When the testing of the `JavaAPIExample`
is complete, all the configuration files for an SSM instance are located in the `/ales32-ssm/java-ssm/SSM-Name`
directory, where `SSM-Name`
is the name of the SSM setup when testing the example.

#### Configure the API Gateway classpath

The API Gateway classpath must be updated to include the JARs and configuration files for the SSM instance. The `jvm.xml`
file must be updated so that various environment variables and the `SSM-Name`
are updated to reflect the installation of the Java SSM. At minimum, the following must be updated in `jvm.xml`:

```
<Environment name="BEA_HOME" value="/opt/apps/bea" >
<Environment name="INSTANCE_NAME" value="SSM-Name" >
```

For example, to modify the classpath, place the following `jvm.xml`
in the `conf` directory of the API Gateway installation:

```xml
<!--Additional JVM settings to run with Oracle Entitlements Server BEA_HOME must be set to the location 
    where the SSM is installed -->
<ConfigurationFragment>
    <!-- Environment variables -->
    <!-- change these to match location where SSM has been installed and configured -->
    <Environment name="BEA_HOME" value="/opt/apps/bea" />
    <Environment name="ALES_SHARED_HOME" value="$BEA_HOME/ales32-shared" />
    <!-- Name of the SSM running in the API Gateway, replace the "SSM-Name" with the name of the SSM for 
    the API Gateway -->
    <Environment name="INSTANCE_NAME" value="SSM-Name" />
    <Environment name="INSTANCE_HOME" value="$BEA_HOME/ales32-ssm/java-ssm/instance/
$INSTANCE_NAME" />
    <Environment name="PDP_PROXY" value="$INSTANCE_HOME/pdpproxy" />
    <!-- Location of the Java SSM libraries -->
    <!-- <ClassDir name="$BEA_HOME" /> -->
    <ClassDir name="$BEA_HOME/ales32-ssm/java-ssm/lib" />
    <ClassDir name="$BEA_HOME/ales32-ssm/java-ssm/lib/providers/ales" />
    <!-- Add location of the SSM configuration to classpath -->
    <ClassPath name="$INSTANCE_HOME/config/" />
    <!-- Additional JVM parameters based on the %JAVA-OPTIONS% of set-env script in SSM instance running 
    in API Gateway $BEA_HOME/ales32-ssm/java-ssm/instance/ssm-name/config -->
    <VMArg name="-Dwles.scm.port=7005" />
    <VMArg name="-Dwles.arme.port=8000" />
    <VMArg name="-Dwles.config.signer=Oracle Entitlements Serverdemo.oracle.com" />
    <VMArg name="-Dlog4j.configuration=file:$INSTANCE_HOME/config/log4j.properties" />
    <VMArg name="-Dlog4j.ignoreTCL=true" />
    <VMArg name="-Dwles.ssl.passwordFile=$ALES_SHARED_HOME/keys/password.xml" />
    <VMArg name="-Dwles.ssl.passwordKeyFile=$ALES_SHARED_HOME/keys/password.key" />
    <VMArg name="-Dwles.ssl.identityKeyStore=$ALES_SHARED_HOME/keys/identity.jceks" />
    <VMArg name="-Dwles.ssl.identityKeyAlias=wles-ssm" />
    <VMArg name="-Dwles.ssl.identityKeyPasswordAlias=wles-ssm" />
    <VMArg name="-Dwles.ssl.trustedCAKeyStore=$ALES_SHARED_HOME/keys/trust.jks" />
    <VMArg name="-Dwles.ssl.trustedPeerKeyStore=$ALES_SHARED_HOME/keys/peer.jks" />
    <VMArg name="-Djava.io.tmpdir=$INSTANCE_HOME/work/jar_temp" />
    <VMArg name="-Darme.configuration=$INSTANCE_HOME/config/WLESarme.properties" />
    <VMArg name="-Dales.blm.home=$INSTANCE_HOME" />
    <VMArg name="-Dkodo.Log=log4j" />
    <VMArg name="-Dwles.scm.useSSL=true" />
    <VMArg name="-Dwles.providers.dir=$BEA_HOME/ales32-ssm/java-ssm/lib/providers" />
    <VMArg name="-Dpdp.configuration.properties.location=$PDP_PROXY/
PDPProxyConfiguration.properties" />
</ConfigurationFragment>
```

#### Centralize all trace output

Oracle’s Java SSM uses log4j to output any diagnostics. You can also add these messages to the API Gateway trace output by adding the log4j that ships with the API Gateway to the following file:

```
/ales32-ssm/java-ssm/SSM-NAME/conf/log4j.properties
```

Then the `log4j.rootCategory=WARN, A1, ASIlogFile` line includes a new appender called `VordelTrace` as follows:

```
log4j.rootCategory=WARN, A1, ASIlogFile, VordelTrace
```

Add the configuration for this new appender by adding the following line to the file:

```
log4j.appender.VordelTrace=com.vordel.trace.VordelTraceAppender
```

You can now start the API Gateway so that it runs with the Java SSM classpath and the centralized trace output.

#### Further information

For more details on configuring and testing SSMs, see the *Oracle SSM Installation and Configuration Guide*.

### Settings

On the **Security Service Module**
settings window, configure the following fields on the **Settings**
tab:

**Enable Oracle Security Service Module**:
Select whether to enable the API Gateway instance to act as an SSM. This setting is disabled by default.

**Application Configuration Name**:
Enter the Application Configuration name for the SSM instance. This is the name of your runtime application used by OES (for example, for monitoring purposes).

**Configuration Name**:
Enter the OES Configuration name for the SSM instance to be stored in the OES Configuration Repository. Configuration names share the same name as their Policy Domain names.

**Application Configuration Properties**:
Click **Add**
to specify optional configuration properties as name-value pairs. Enter a **Name**
and **Value**
in the **Properties**
dialog. Repeat to specify multiple properties.

**Policy Domain Name**:
Enter the OES Policy Domain name for the SSM instance. Policy Domains contain policy definitions (target resource, permission set, and policy). Policy Domain names share the same name as their Configuration names.

### Name authority definition settings

Configure the following field on the **Name Authority Definition**
tab:

**Name Authority Definition File**:
Click the **Browse**
button at the bottom right to configure the Name Authority Definition file for the SSM. This is an XML file that specifies the naming authority definition required for the API Gateway. For example, a specified XML file named `apigatewayNameAuthorityDefinition.xml`
file should contain the following settings:

```
<AuthorityConfig>
    <AuthorityDefinition name="apigatewayResource" delimiters="/\">
    <Attribute name="protocol" type="MULTI_TOKEN" authority="URLBASE" />
    </AuthorityDefinition>
    <AuthorityDefinition name="apigatewayAction" delimiters="/">
    <Attribute name="action" type="SINGLE_VALUE_TERMINAL" />
    </AuthorityDefinition>
</AuthorityConfig>
```

## Configure Kerberos settings

The **Kerberos Configuration** under **Server settings > Security > Kerberos** in the node tree enables you to configure instance-wide Kerberos settings on API Gateway and to upload a Kerberos configuration file to API Gateway. This configuration file contains information on the location of the Kerberos Key Distribution Center (KDC), as well as which encryption algorithms, encryption keys, and domain realms to use.

You can also configure trace options for the various APIs used by the Kerberos system, such as the Generic Security Services (GSS) and Simple and Protected GSS-API Negotiation (SPNEGO) APIs.

Linux platforms ship with a native implementation of the GSS library, which API Gateway can leverage. You can specify the location of the GSS library in this configuration window.

For more details on different Kerberos setups with API Gateway, see the [API Gateway Kerberos Integration Guide](/docs/apigtw_kerberos/).

### Kerberos configuration file `krb5.conf`

The Kerberos configuration file (`krb5.conf`) defines the location of the Kerberos KDC, supported encryption algorithms, and default realms in the Kerberos system. Both Kerberos clients and Kerberos services that are configured for API Gateway use this file.

Kerberos clients need to know the location of the KDC so that they can obtain a Ticket Granting Ticket (TGT). They also need to know what encryption algorithms to use and what realm they belong to. Kerberos services do not need to call the KDC to request a TGT, but they still require the information on supported encryption algorithms and default realms contained in the `krb5.conf`
file.

A Kerberos client or service identifies the realm it belongs to because the realm is appended to its Kerberos principal name after the `@`
symbol. Alternatively, if the realm is not specified in the principal name, the Kerberos client or service assumes the realm to be the `default_realm`
specified in the `krb5.conf`
file. The file specifies only one `default_realm`, but you can specify a number of additional named realms. The `default_realm`
setting is in the `[libdefaults]`
section of the `krb5.conf`
file. It points to a realm in the `[realms]`
section. This setting is not required.

The text input field in the Kerberos configuration window displays a default configuration for `krb5.conf`. You can type and modify the configuration as needed, and then click **OK** to upload it to your API Gateway configuration. Alternatively, if you have an existing `krb5.conf`
file that you want to use, select **Load File** and open to the configuration file. The contents of the file are displayed in the text area, and you can edit and upload it to API Gateway.

Refer to your Kerberos documentation for more information on the settings that can be configured in the `krb5.conf`
file.

### Advanced settings

You can configure various tracing options for the underlying Kerberos API using the check boxes on the **Advanced settings** tab. Trace output is always written to the `/trace`
directory of your API Gateway installation.

* **Kerberos Debug Trace**– Enables extra tracing from the Kerberos API layer.
* **SPNEGO Debug Trace** – Switches on extra tracing from the SPNEGO API layer.
* **Extra Debug at Login**– Provides extra tracing information during login to the Kerberos KDC.

### Native GSS library

The Generic Security Services API (GSS-API) is an API for accessing security services, including Kerberos. Implementations of the GSS-API ship with the Linux platforms and can be leveraged by API Gateway when it is installed on these platforms. The fields on this tab allow you to configure various aspects of the GSS-API implementation for your target platform.

**Use Native GSS Library**:
Select this to use the operating system's native GSS implementation. This option only applies to API Gateway installations on the Linux platforms.

{{< alert title="Note" color="primary" >}}These are instance-wide settings. If you select **Use Native GSS Library**, it is used for all Kerberos operations, and all Kerberos clients and services must be configured to load their credentials natively.\
If the native library is used, the following features are not supported:\

* The SPNEGO mechanism
* The WS-Trust for SPNEGO standard (requires the SPNEGO mechanism)
* The SPNEGO over HTTP standard (requires the SPNEGO mechanism)
* Signing and encrypting using the Kerberos session keys

>It is possible to use the KERBEROS mechanism with the SPNEGO over HTTP standard, but this would be non-standard.
{{< /alert >}}

**Native GSS Library Location**:
If you have opted to use the native GSS library, enter the location of the GSS library in the field provided, for example, `/usr/lib/libgssapi.so`. On Linux, the library is called `libgssapi.so`. .

{{< alert title="Note" color="primary" >}}This setting is only required when this library is in a non-default location.{{< /alert >}}

**Native GSS Trace**:
Use this option to enable debug tracing for the native GSS library.
