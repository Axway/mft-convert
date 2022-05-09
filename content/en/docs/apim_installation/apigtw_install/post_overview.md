{
"title": "Post-installation",
"linkTitle": "Post-installation",
"weight":"28",
"date": "2019-10-07",
"description": "Tasks to perform after installing API Gateway."
}

This section describes various tasks that you might perform after installing API Gateway. This includes how to check if an installation has been successful, any initial configuration needed before you can start API Gateway, what you should do to secure API Gateway, and so on.

## Verify the installation

To verify your installation, follow these guidelines.

### Check the installation log

You can examine the installation log in the root directory of the installation (for example, `Axway-installLog.log`).

### Start API Gateway components and tools

* To start the API Gateway server and Admin Node Manager and to log in to the API Gateway Manager web-based administration tool, see [Start API Gateway](/docs/apim_installation/apigtw_install/install_gateway/#start-api-gateway).
* To start the Policy Studio desktop tool, see [Start Policy Studio](/docs/apim_installation/apigtw_install/install_policy_studio/#start_policy_studio).
* To log in to the API Manager web-based tool, see [Start API Manager](/docs/apim_installation/apigtw_install/install_api_mgmt/#start-api-manager).
* To start the Configuration Studio desktop tool, see [Start Configuration Studio](/docs/apim_installation/apigtw_install/install_config_studio/#start-configuration-studio).
* To set up and start the API Gateway Analytics server, and to log in to the API Gateway Analytics web-based tool, see the [API Gateway Analytics User Guide](/docs/apimanager_analytics/).

## Initial configuration

Depending on the installation options you selected, the following tasks might need to be completed before you can start API Gateway.

### Create a new domain

If you did not install the QuickStart tutorial, you must use the `managedomain` script to create a new managed domain that includes an API Gateway instance. You can run the script from the following directory

```
INSTALL_DIR/apigateway/posix/bin
```

For more details on running `managedomain`, see [Configure an API Gateway domain](/docs/apim_administration/apigtw_admin/makegateway/).

### Run API Gateway on privileged ports

API Gateway is run as a non-root user to prevent any potential security issues with running as the `root` user. To enable API Gateway to listen on privileged ports when running as non-root, you must perform the steps in [Run API Gateway on privileged ports](/docs/apim_administration/apigtw_admin/admin_non_root/). If you do not perform these steps, the following error is reported during API Gateway startup:

```
ERROR   ...  failed to listen on address 0.0.0.0/80: Permission denied. can't bind socket to address
```

### Set up a metrics database for API Manager or API Gateway Analytics

If you installed API Manager, see [Install and configure a metrics databases](/docs/apim_installation/apigtw_install/metrics_db_install/).

If you installed API Gateway Analytics, see the [API Gateway Analytics User Guide](/docs/apimanager_analytics/).

## Secure API Gateway

Perform the following tasks after installation to secure your API Gateway system and protect the API Gateway environment from internal or external threats.

### Change default passwords

If you did not set an administrator user name and password during installation, you should change the default administrator user name and password now. For details, see
[Manage administrator users](/docs/apim_administration/apigtw_admin/manage_user_access/#manage-admin-users).

### Change default certificates

The default certificates used to secure API Gateway components are self-signed. You can replace these self-signed certificates with certificates issued by a Certificate Authority (CA) For details, see [Manage certificates and keys](/docs/apim_administration/apigtw_admin/general_certificates/).

### Encrypt API Gateway configuration

By default, API Gateway configuration is unencrypted. You can specify a passphrase to encrypt API Gateway instance configuration as detailed in [Configure an API Gateway encryption passphrase](/docs/apim_administration/apigtw_admin/general_passphrase/).

### Change default session timeout for API Gateway Manager

The default idle session timeout for the API Gateway Manager web UI is 12 hours. It is recommended that you change this timeout to 120 minutes or less:

1. Open the file `INSTALL_DIR/apigateway/conf/envSettings.props`.
2. Edit the property `env.WEBMANAGER.SESSION.TIMEOUT`. The property value is in milliseconds. The default value is 43200000 (12 hours).
3. Restart API Gateway for the updates to be applied.

### Where to go next

For additional procedures you can perform to secure your API Gateway, see the [API Gateway Administrator Guide](/docs/apim_administration/apigtw_admin/).

For more information on the security features of API Gateway and best practices for strengthening the security of API Gateway, see the [API Management Security Guide](https://docs.axway.com/bundle/apim-security-guide/page/api_management_security_guide.html).

## Set up services

This section explains how to run various components as services.

### API Gateway

You can run Node Managers and API Gateway instances as services using the `managedomain` script. To register a Node Manager or an API Gateway instance as a service
on Linux, you must run the `managedomain` command as `root`. For example:

* Node Manager: Enter `managedomain --menu`, and choose option 2, `Edit a host`.
* API Gateway instance: Enter `managedomain --menu`, and choose option 10, `Add script or service for existing local API Gateway`.

Alternatively, you can run `managedomain` in command mode with the `--add_service` option to create a service for a Node Manager or API Gateway instance.

For more details on `managedomain`, see [managedomain command reference](/docs/apim_reference/managedomain_ref/).

### API Gateway Analytics

You can also run the API Gateway Analytics server as a service by creating a script. A sample script and *ReadMe* is provided in the following directory:

```
INSTALL_DIR/analytics/posix/samples/etc/init.d/
```

### Apache Cassandra

For details on running Apache Cassandra as a service, see [Install an Apache Cassandra database](/docs/apim_installation/apigtw_install/cassandra_install).

## Set up clustering

To set up API Gateway for high availability, you need to configure an external Apache Cassandra database for clustering. For more information, see the following topics:

* [Configure a Cassandra HA cluster](/docs/cass_admin/cassandra_config/)
* [Configure API Management in multiple datacenters](/docs/apimgmt_multi_dc/)
