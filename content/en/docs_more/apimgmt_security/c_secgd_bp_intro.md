{
"title": "Security best practices",
"linkTitle": "Security best practices",
"weight": 90,
"date": "2019-11-25",
"description": "Recommended best practices for securing API Gateway, API Manager, and API Portal."
}

## Secure connections

All connections between internal components (API Gateways and Node Managers) are secured by mutual authentication.  Best practices would recommend securing all connections to external networks with mutual authentication, where that is supported by the back-end service.

Similarly, where API Gateway is receiving messages over various protocols, ensure these connections are mutually authenticated where possible.

## Sample certificates

Axway provides sample certificates with the product. These sample certificates and key pairs can be found in the API Gateway's trusted certificate store and should be used for test purposes only.

As soon as the product goes live, or as soon as real data is managed by the product, you must use your own certificates. Using sample certificates is a security risk as all API Gateway customers have the same certificates with the same private keys.

## Self-signed certificates

Using self-signed certificates may also be a security risk for many reasons, including:

* Anyone can generate his own certificate and you need a very secure process to receive/send these certificates to make sure they are coming from the right partner. When using CA-signed certificates, you can rely on the CA.
* If self-signed certificates are not securely stored, anyone can change them. CA-signed certificates also must be securely stored, but no one can change them.
* There is no way to revoke self-signed certificates.

## Privileged access user list

The API Gateway installer sets access rights on its files so that only the user that installed the image can read or modify files.  This means that only the API Gateway itself can access its log, trace, and configuration files.

You must limit the users that have privileged access to the machine on which API Gateway is running. At a minimum, maintain a list of administrators and a list of users with access to multiple functions.

## Internet access limitation

As much as possible, limit the number of Internet access points. Do not open useless Internet connections and limit interconnections with external networks as much as possible. This limits the product’s attack surface, reduces the risk of external attacks, and makes it easier to audit the product.

For a list of default ports that are opened by the API Gateway components, see [Default ports](/docs/apim_installation/apigtw_install/system_requirements/#default-ports).

## Session timeouts

The default idle session timeout for the API Gateway Manager web UI is 12 hours. It is recommended that you change this timeout to 120 minutes or less. For details, see [Change default session timeout for API Gateway Manager](/docs/apim_installation/apigtw_install/post_overview/#change-default-session-timeout-for-api-gateway-manager).

## Service packs and patches

In the event of a possible vulnerability discovered in the product, you must be able to apply the patch or new service pack as soon as possible. Make sure you have the correct procedure to complete the upgrade. Always use the latest version of the product, if possible, as it contains fixes to known vulnerabilities.

For more information on upgrade procedures, see the following sections:

* API Gateway and API Manager software:
    * [API Gateway Upgrade Guide](/docs/apim_installation/apigw_upgrade/)
    * [Update API Gateway](/docs/apim_installation/apigtw_install/install_service_packs/)
* API Gateway and API Manager container deployment:
    * [Upgrade a container deployment](/docs/apim_installation/apigw_containers/container_upgrade/)
    * [Apply a patch, update, or service pack](/docs/apim_installation/apigw_containers/container_patch_sp/)
* API Portal:
    * [Upgrade API Portal](/docs/apim_installation/apiportal_install/upgrade_automatic/)
    * [Upgrade API Portal docker deployment](/docs/apim_installation/apiportal_docker/upgrade_docker/)
    * [Update API Portal with a service pack or patch](/docs/apim_installation/apiportal_install/install_service_pack/)

## Generic or anonymous users

The term “generic users” means that the password is shared among multiple specific users. This makes it easier for an attacker to retrieve this password. In addition, the procedure to change shared passwords can be complicated and risky. In case of an incident, these generic or anonymous users make it impossible to determine who completed the erroneous action.

In cases where multiple administrator users are responsible for configuring policies (via Policy Studio) to run on API Gateway, it makes a lot of sense to create distinct users for each administrator user.

## Password policy

In line with security best practices, you can configure a password policy for administrator users in API Gateway Manager. Password policy refers to the size and complexity of the password, as well as to all the rules to manage the password.

It is also possible to take certain actions when a configurable number of invalid authentication attempts has occurred via HTTP basic, HTTP digest, and HTML form-based authentication.  For example, you can lock a user account or ban an IP address if a certain number of invalid passwords have been submitted to API Gateway. For details, see [Authentication filters](/docs/apim_policydev/apigw_polref/authn_common/).

For more information on setting the password policy for administrator users, see the
[Configure a password policy for admin users](/docs/apim_administration/apigtw_admin/manage_user_access/#configure-a-password-policy-for-admin-users).

You can also configure password policies for [API Manager](/docs/apim_administration/apimgr_admin/api_mgmt_admin/#enforce-password-changes) and [API Portal](/docs/apim_administration/apiportal_admin/customize_page_content/#enforce-password-policies) users.

## Passphrase policy

In line with security best practices, an API Server Administrator can configure a passphrase policy for node managers and API Gateway groups. Passphrase policy refers to the size and complexity of the passphrase, as well as all the rules to manage the passphrases.

For more information on setting the passphrase policy, see [Configure a passphrase policy for node managers and API Gateway groups](/docs/apim_administration/apigtw_admin/manage_user_access/#configure-a-passphrase-policy-for-node-managers-and-api-gateway-groups).

## Default authentication account

The default behavior of the software version of the product is to force you to set an administrator password during installation.

## Remote connections

You should limit your remote connections in the following ways:

* If no one needs to access the product remotely, make sure that the UI ports are closed in your firewall.
* If someone needs to connect remotely only occasionally, set a procedure to open the port only on demand.
* Do not turn off the secure-by-default measures already taken for the internal components of the product, for example, API Gateway, Admin Node Manager, and Policy Studio.

## Logging, audit, and alerts rules

An important aspect of security is to be notified when something wrong occurs, and to be able to investigate it. Therefore, it is important to define the right level of logging and audit, and to set the right alerts.

API Gateway can log audit and monitoring data to different locations, for example, Axway Sentinel, local text or XML file, local or remote syslog, and so on.

Events can be audited at different levels, for example, success, failure, or abort.  The audit trails written to the local text file, local XML file, and database can all be signed to ensure integrity.

The product can also send various types of alerts (for example, email messages, SNMP traps, Amazon SNS, and so on) under certain configurable error conditions.

See [Configure logging and events](/docs/apim_administration/apigtw_admin/logging/) and [Configure system alerts](/docs/apim_policydev/apigw_poldev/general_system_alerts/) for more information on configuring logging and alerting in API Gateway.

## Sensitive files and databases

In general, it is good practice to limit the number of administrators with access to the product.

Ensure the default protection mechanisms on sensitive files used by API Gateway and API Portal remain in place.  For example, the product’s files are installed with read, update, and delete privileges such that only the product can access them by default.  There should be no reason to change these privileges.

The following files are deemed sensitive from a security perspective, where `GROUP` and `INSTANCE` placeholders represent the identifiers (for example, `group-2`, `instance-1`, and so on) of the group in a multi-group and a multi-instance domain, respectively.

### Node Manager Entity Store

The files located in the following directory comprise the configuration for the Node Manager:

```
/conf/fed
```

### API Gateway Entity Store

The files in the following directory, where `<ID>` represents the policy package ID, make up the main configuration for API Gateway.

```
/groups/<GROUP>/conf/<ID>

```

As stated earlier, an encryption passphrase can be used to encrypt sensitive data in the Entity Store, including private keys, passwords, and tokens.

### Administrator passwords

The following file contains the hashed passwords of administrator users that have the ability to configure the product.

```
/conf/adminUsers.json
```

The passwords are stored as a salted hash derived from 102400 iterations of the PBKDF2 algorithm with HmacSHA-2.  Each password uses a different salt so that identical passwords result in different stored hashes.

### Node Manager and API Gateway Group passphrases

The following file contains the hashed passphrases for Node Managers and API Gateway groups.

```
/system/conf/groupSettings.json
```

The passphrases are stored as a salted hash derived from 102400 iterations of the PBKDF2 algorithm with HmacSHA-2. Each passphrase uses a different salt so that identical passphrases result in different stored hashes.

### Key Property Store

If you have configured the product to store data in a Key Property Store (KPS), the files located in the following directory must be protected:

```
/groups/<GROUP>/<INSTANCE>/Communion/kps
```

### Automated startup files

In cases where API Gateway must be started automatically without manual intervention, it is necessary to store the Entity Store encryption passphrase (when used) in certain configuration files, which are as follows:

```
/groups/<GROUP>/<INSTANCE>/conf/service.xml

/system/conf/nodemanager.xml

/skel/service.xml
```

If you configure a password executable file for use with `managedomain`, it is important to protect the following file:

```
/conf/managedomain.props
```

You should also protect the password executable file specified by the `password_exec` parameter in the `managedomain.props` file.

### API firewalling rule set

API Gateway embeds Apache ModSecurity — a toolkit for real-time HTTP traffic monitoring, logging, and access control — to help companies mitigating application-level threats on their APIs. The embedded ModSecurity engine looks for threat protection rules configuration in the `/system/conf/threat-protection/default` directory. This directory and the files within it must be protected.

### Audit, trace, and log files

In terms of audit trail data written out by the API Gateway, it is important to protect the following files and directories from modification or deletion:

```
/logs

/trace

/groups/<GROUP>/<INSTANCE>/logs

/groups/<GROUP>/<INSTANCE>/trace
```

You should also protect any custom logging files if a non-default location has been configured.

### API Portal files

The files located in the following directory contain configuration, log, and source files:

```
/opt/axway/apiportal/htdoc
```

The following directory contains the database user and password:

```
/opt/axway/apiportal/htdoc/configuration.php
```

### Files related to single sign on (SAML SSO)

SSO agent configuration file:

```
/groups/<GROUP>/<INSTANCE>/conf/service-provider.xml
```

SSO agent keystore (private key used to sign SAML messages) generated by the user in:

```
/groups/<GROUP>/<INSTANCE>/conf/
```

Identity Provider’s metadata files:

```
/groups/<GROUP>/<INSTANCE>/conf/idp.xml
```

It is a recommended practice to not change default security options:

```
secure-cookie = true

saml-allow-http-connection = false
```

Signature:

```
sign = true

saml-signature-algorithm = rsa-sha256

saml-allow-unsigned-assertion = false

saml-verify-metadata-signature = true
```

### File integrity

After downloading the product package from Axway Support at [https://support.axway.com](https://support.axway.com/), it is highly recommended to verify the file integrity. Use a third-party tool for your OS to compute the hash of the downloaded file and compare it with the hash that is displayed on the Details page for the product package. Both SHA-256 and MD5 hashes are provided but it is safer to use SHA-256.

It is also recommended to protect file integrity after the product has been installed using a file integrity monitoring tool. In order to be able to configure the monitoring tool, the following tables provide information about the files used by the product and which actions can modify those files.

#### Files modified when upgrading

Upgrading API Gateway, API Manager, or API Portal can modify any file in the system. Before you upgrade, it is recommended to switch off your monitoring tool.

#### Files modified on specific actions in API Gateway and API Manager

Outside upgrade, the following files in API Gateway and API Manager should not change, and it is recommended to monitor their integrity:

* `*.so` C++ files in `INSTALL_DIR/apigateway/Linux.x86_64` and all subdirectories.
* `gateway-oracle-em-plugin.jar` Oracle Java plugin in `INSTALL_DIR/apigateway/Linux.x86_64/system/conf/oracle-em`.       * `*.jar` Java files in the following directories and all subdirectories:
    * `INSTALL_DIR/apigateway/Linux.x86_64y/system/lib`
    * `INSTALL_DIR/apigateway/Linux.x86_64/jre/lib/`
    * `INSTALL_DIR/apigateway/Linux.x86_64/lib/`
    * `INSTALL_DIR/apigateway/Linux.x86_64/java/`
    * `INSTALL_DIR/apigateway/Linux.x86_64/java`
    * `INSTALL_DIR/apigateway/samples/WebServices/lib/`
    * `INSTALL_DIR/apigateway/upgrade/legacy/7.1.x/`
* `*.js` Javascript files in `INSTALL_DIR/apigateway/webapps` and all subdirectories.
* `*.*` executables in `INSTALL_DIR/apigateway/posix/bin`.

All other files are modified at runtime and cannot be verified using a monitoring tool.

#### Files modified on specific actions in API Portal

Outside upgrade, the following files in API Portal are modified by specific actions:

* `configuration.php` configuration file  in `INSTALL_DIR/`: The configuration is updated manually or through Joomla! Administration Interface (JAI).
* `default.php`, `contact.php`, `faq.php` static content files in:
    * `INSTALL_DIR/views/documentation/tmpl/`
    * `INSTALL_DIR/views/help/tmpl/`
    * `INSTALL_DIR/views/pricing/tmpl/`
    * `INSTALL_DIR/views/terms/tmpl/`
    * `INSTALL_DIR/views/home/tmpl/`

    These files contain static content that can be modified.
* `*.*` template files in `INSTALL_DIR/templates/purity_iii/local/`: New themes are created or the CSS files modified.
* `*.*` custom CSS files in `INSTALL_DIR/templates/purity_iii/css/`: The custom CSS file can be modified.

All other files are modified at runtime and cannot be verified using a monitoring tool.

## Unusual behavior and automated tools detection

The web UI and REST APIs available from the products make them susceptible to attacks on the Internet. Therefore, besides the protection described in this document, we strongly recommend that you also implement additional protection, such as `proxies` and `WAF`, to secure connections. These tools enable you to detect unusual behavior and to identify connections from an automated tool. Make sure to receive alerts when using these tools, and to secure involved users and their addresses.

At a minimum, you should perform a periodic review of the log and audit to deductively detect any issues.

## API Portal certificate verification

It is recommended that you explicitly configure the API Manager certificate in API Portal and enable API Portal to verify the certificate and host of API Manager. For more information, see [Connect API Portal to API Manager](/docs/apim_installation/apiportal_install/connect_to_apimgr/).

## API Portal Internet access restriction

In API Portal, it is recommended that you protect the Joomla! administration UI from direct Internet access. For more information, see [Secure API Portal](/docs/apim_installation/apiportal_install/secure_harden_portal/).

## API Portal administrator user name

API Portal forces you to change the administrator password on first login. For best practice, you should also change the administrator user name.
