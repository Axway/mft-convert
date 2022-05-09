{
"title": "API Gateway configuration files",
  "linkTitle": "API Gateway configuration files",
  "weight": 130,
  "date": "2020-07-16",
  "description": "List of files that you can use to change API Gateway configuration."
}
List of API Gateway files that you can use to change the configuration of the product, so you can test your customizations after the upgrade is complete.

## Redaction files

Contains settings for redaction.

```
apigateway/system/conf/redaction.xml
```

## Node manager entity store

The following directory contains the entity store configuration for node manager.

```
apigateway/conf/fed
```

## API Gateway group entity store

The following directory contains the entity store configuration for the gateway.

```
apigateway/groups/group-id/conf
```

## Analytics entity store

Contains the entity store configuration for analytics.

```
analytics/conf/fed
```

## Custom OAuth

Files on disk that you can modify to change the OAuth configurations. There are also sample configs and scripts, it's possible they may be modified by users.

```
${environment.VDISTDIR}/samples/oauth/templates/login.html
${environment.VDISTDIR}/samples/oauth/templates/requestAccess.html
${environment.VDISTDIR}/samples/oauth/templates/showAccessCode.html
```

```
/opt/Axway-7.7.0/apigateway/samples/oauth/
/opt/Axway-7.7.0/apigateway/samples/scripts/oauth/
```

## Logging changes

Edit the logging configurations. We have changed the `log4j` configuration file from XML to YAML format.

```
apigateway/system/conf/log4j2.yaml
apigateway/system/conf/loggers/eventLog.yaml
apigateway/system/conf/loggers/openTrafficLog.yaml
apigateway/system/conf/loggers/topologyLog.yaml
```

## JVM changes

Configure JVM settings.

```
apigateway/system/conf/jvm.xml
```

The following directories can also be used to define the JVM.XML

```
apigateway/conf
apigateway/groups/group-2/instance-1/conf
```

For more information, see [JVM System Properties](/docs/apim_reference/system_props/)

## ext/libs entries

Directory for customer JARs. These can be third-party JARs or JARs with Java code that they have written for their own custom filters. Customers are warned that third-party JARs could conflict with those already installed with the API Gateway in the `system/lib` directory.

There is a system-wide `ext/lib` file, and another one for each API Gateway instance.

```
apigateway/ext/lib
apigateway/groups/group-id/instance-id/ext/lib
```

## ext/posix/bin

This directory contains customers custom scripts.

```
apigateway/ext/posix/bin
```

## ext/Linux.x86_64

Configure Java level properties and settings. Often used to turn legacy behavior `on/off` at system level.

```
apigateway/system/conf/jvm.xml
apigateway/conf/jvm.xml
apigateway/groups/group-id/instance-id/conf/jvm.xml
```

*Example:*

```
<?xml version="1.0" encoding="UTF-8"?>
<ConfigurationFragment>
   <VMArg name="-Ddont.expect.100.continue=true"/>
</ConfigurationFragment>
```

## SSO Configuration files

Contains configuration for Single sign-on.

```
service-provider.xml
sso.jks
idp.xml
```

## ACL and roles for node manager management APIs

Used to edit the Access Control List roles for node manager management APIs.

```
apigateway/conf/acl.json
```

## Admin users

Changes and additions to admin users.

```
apigateway/conf/adminUsers.json
```

## Domain audit log

A series of rules to control entry to the domain audit log.

```
apigateway/conf/apiaudit.xml
```

## envSettings.props for node manager

Environment settings for node manager, such as host and port information.

```
apigateway/conf/envSettings.props
```

## envSettings.props for API Gateway instances

Environment settings for API Gateway instances, such as host and port information.

```
apigateway/groups/group-id/instance-id/conf
```

## managedomain.props

Used to set username and password for `managedomain` in a file, rather than required on command line.

```
apigateway/conf/managedomain.props
```

## openssl.cnf

Used for creating the default certs in the node manager. Contains the list of predefined variables for the cert.

```
apigateway/conf/openssl.cnf
```

## passwordPolicy.json

Used for configuring rules around password requirements. You can edit this file through the UI, or using an editor of your choice.

```
apigateway/conf/passwordPolicy.json
```

## passphrasePolicy.json

Used for configuring rules around passphrase requirements for node managers and API Gateway groups. You can edit this file through an API call, or using an editor of your choice.

```
apigateway/conf/passphrasePolicy.json
```

## userconfig.xml for node manager

User configurations for node manager.

```
apigateway/conf/userconfig.xml
```

This file is pulled in via `system/conf/config.xml`, which is used in the `system/conf/nodemanager.xml`.

## API Manager custom properties

Custom properties files contain customizable variables and functions for API Manager and API Gateway web UIs:

* Javascript syntax must be used.
* Media type `application/javascript` is implied when they are accessed via browser.

The `vordel/apiportal/app/app.config` allows customers to add custom fields to the API Manager objects.

```
./apigateway/webapps/emc/vordel/manager/app/app.config
./apigateway/webapps/apiportal/vordel/apiportal/app/app.config
./apigateway/webapps/apiportal/vordel/apiportal/app-login/app.config
./apigateway/webapps/apiportal/vordel/apiportal/registry-login/app.config
```

## API Manager email templates

Email templates and images for API Manager user and application registration and password workflows.

```
/apigateway/system/conf/apiportal/email
/apigateway/system/conf/apiportal/email/images
```

## Customized API Manager landing page

Contains HTML files that you can edit to change the landing page for API manager.

```
/apigateway/webapps/apiportal/vordel/apiportal/custom-login
```

## Topology

This file is automatically changed when entries are made to the topology. It is possible to edit it manually, but this is not recommended.

```
apigateway/groups/topology.json
```

## KPS data

Key Property Store data generated when the file-based KPS is used. This is data, as opposed to configuration.

```
apigateway/groups/group-id/instance-id/conf/kps/file
```

## API Firewall (WAF)

API Gateway administrators can configure the embedded ModSecurity engine to protect API Gateway HTTP traffic against threats and monitor reported exceptions.

```
/apigateway/system/conf/threat-protection/apigw-manager/modsecurity.conf
/apigateway/system/conf/threat-protection/default/modsecurity.conf
/apigateway/groups/group-id/instance-id/conf/waf.xml
```

## Traffic Monitor data

Data configuration for the traffic monitor. This is data, as opposed to configuration.

```
apigateway/groups/group-id/instance-id/conf/opsdb.d
```
