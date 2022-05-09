{
"title": "managedomain command reference",
  "linkTitle": "managedomain command reference",
  "weight": "30",
  "date": "2019-10-14",
  "description": "Run the `managedomain` command in different modes."
}
This topic describes how to run `managedomain` in the following modes:

* **Command interpreter mode**: Enter commands from a list using tab completion (default mode).
* **Interactive mode**: Follow instructions at the command prompt.
* **Command mode**: Enter specific commands and parameters.

The `managedomain` command is available in the following directory:

```
INSTALL_DIR/apigateway/posix/bin
```

For an overview of the `managedomain` command, see [Configure an API Gateway domain](/docs/apim_administration/apigtw_admin/makegateway).

## Command interpreter mode

To run in default command interpreter mode, enter `managedomain`, and press Tab to view and select options. For example:

```bash
Axway-7.7/apigateway/posix/bin>managedomain
Running in command interpreter mode. Enter 'quit' to exit.
Enter 'help' to view help topics. Enter 'help topic' to view help for a topic.
Press TAB to view and complete commands.
>

add                        add_service_only           add_tag
change_domain_passphrase   change_passphrase          create_archive
create_instance            delete_group               delete_host
delete_instance            delete_tag                 deploy
download_archive           edit_group                 edit_host
edit_instance              help                       initialize
list_deployments           print_topology             q
quit                       regen_certs                remove_service
reset                      sign_csr                   submit_cert
topology_check             topology_compare           topology_merge
topology_synch             version
```

You must first run `initialize` to register the first host in the domain in order to create and run the gateways.

### View help for a command

You can view detailed help for each command and its parameters by entering `help` followed by the command name. The following example shows the help for the `initialize` command:

```bash
help initialize

Register the first Node Manager and host in a new domain. This Node Manager will
always be an Admin Node Manager.

*** Usage ***
initialize
initialize nm_name NodeManagerOne
initialize host host1.axway.com port 8090
initialize domain_name DomainOne domain_passphrase 12345678 sign_alg sha256
initialize sign_with_user_provided ca ca.p12 domain_passphrase password
initialize sign_with_external_ca
initialize subj_alt_name "DNS.1=host1.com" subj_alt_name "DNS.2=host2.com"

*** Command Parameters ***
nm_name : User-friendly name of new Node Manager. Defaults to 'Node Manager
on host name'.

host : Host name. Default host name for the localhost will be used if not
provided.

port : Port for new Node Manager. Default is env.PORT.MANAGEMENT from
envSettings.props (i.e., 8090).

sign_with_generated : Use a system-generated self-signed domain certificate
to sign the SSL certificates for Node Managers and API Gateways. When the
--initialize command is run, the domain private key and certificate will be
generated. This is the default cert management option.

sign_with_user_provided : Use a user-provided domain key and certificate to
sign SSL certificates for Node Managers and API Gateways.

sign_with_external_ca : Use an external CA to sign SSL certificates for Node
Managers and API Gateways. managedomain will output CSRs which must be sent to
the external CA. Use --submit_cert command to submit the certificate when returned
from the external CA.

domain_passphrase : Passphrase for the domain private key, use with
--sign_with_generated and --sign_with_user_provided for non-default passphrase.
Defaults to no passphrase.

sign_alg : Optional signing algorithm for domain certificates (e.g., 'sha1',
'sha256', 'sha384' or 'sha512'). 'sha256' by default.

ca : Name of PKCS12 file with user-provided domain key and certificate, use with
--sign_with_user_provided.

key_passphrase : Used to encrypt temporary key files. Defaults to no passphrase. This
is optional, but recommended especially for --sign_with_external_ca when temporary
sensitive files may reside on disk for considerable time.

domain_name : Name of the domain, this will be used as the dname in the domain
certificate if it is system-generated, use with --sign_with_generated. Maximum length
is 64 characters.

subj_alt_name : Subject alternative names for Node Manager SSL certificates, can
specify multiple names. Default subject alternative names will be added if not
provided.

add_service : create Linux service for the new server (Node Manager or API Gateway).

service_user : User to run service as.

nm_entitystore_passphrase : The entity store password

metrics_enabled : Controls whether metrics data collection is enabled or not.

metrics_dburl : Metrics database URL e.g. jdbc:mysql://127.0.0.1:3306/DefaultDB or
jdbc:mysql://127.0.0.1:3306/reports

metrics_dbuser : The database username e.g. root or metrics db user.

metrics_dbpass : The plaintext password associated with the database username.

debug : enable debugging.
```

### Run a command

You can run a command using tab completion to specify parameters. The following example shows the available parameters for the `create_instance` command:

```
create_instance <press TAB>
```

```bash
name                       group                      host
instance_management_port   instance_services_port     passphrase
yaml                       sign_with_generated        sign_with_user_provided    
sign_with_external_ca      domain_passphrase          sign_alg                   
ca                         key_passphrase             add_service                
service_user               debug                      anm_host                   
anm_port                   username                   password
truststore_file            truststore_password
```

The following example creates a new gateway instance with a specific name and group:

```
create_instance name APIServer1 group Group1
```

```bash
Requesting CSR from Admin Node Manager...
CSR received from Admin Node Manager.
Requesting signed certificate from Admin Node Manager...
Signed certificate received from Admin Node Manager.
Requesting Admin Node Manager to create new API Gateway...
New API Gateway SSL certificate details:
    dname:CN=instance-1,OU=group-2,DC=host-1
    expires:Fri Apr 27 16:36:37 BST 2114
    thumbprint:E6:C5:B5:6D:52:1D:74:3E:CD:3E:8B:B5:82:24:3A:78:1B:C9:27:F9
    issuer dname:CN=Domain
    issuer thumbprint:B5:68:73:7A:7B:ED:6D:B0:04:40:CF:E3:BC:36:6D:84:F7:49:29:12

The new API Gateway 'APIServer1' in group 'Group1' has been successfully created and installed

Start the new API Gateway by executing the following command:
Axway-7.7/apigateway/posix/bin/startinstance -g "Group1 " -n "APIServer1"

You can alternatively add Axway-7.7/apigateway/posix/bin/ to your path and use
"startinstance -g "Group1" -n "APIServer1"".

You can test the connection by visiting the URL: http://roadrunner:8080/healthcheck
```

Tab completion is also available for some parameter values (instance names, group names and host names). The following example shows available instances for the `delete_instance` command:

```bash
delete_instance name <press TAB>

APIServer1   APIServer2

delete_instance name APIServer
```

## Interactive mode

To run in interactive mode, enter `managedomain --menu`, and follow the instructions at the command prompt. The following options are available:

### Host management

The `managedomain --menu` options for host management are as follows:

Option `1`: `Register host`

Add a new host that runs an API Gateway to a domain topology. This is equivalent to the `initialize` command. You must ensure that the host is registered in order to create and run API Gateways. For example, you can specify the following:

* If the host is an Admin Node Manager
* Host name
* Node Manager name
* Node Manager port
* Node Manager passphrase
* Linux service for Node Manager
* Trust store details

When registering a Node Manager (Admin or not) in an existing domain, you must specify host details of a running Admin Node Manager in the domain. This is not required when registering the first Admin Node Manager in a new domain because this is always an Admin Node Manager.

Option `2`: `Edit a host`

Edit the details for a host registered in a domain topology (used occasionally). You can update the following:

* Host name
* Node Manager name
* Node Manager port
* Node Manager passphrase
* Linux service for Node Manager
* Change admin capabilities
* Enable metrics

Changing admin capabilities enables you to change a Node Manager to Admin Node Manager, or an Admin Node Manager to Node Manager. You can only change admin capabilities of the Node Manager running on the same machine. You cannot remove admin capabilities of the last Admin Node Manager in a domain, or from an Admin Node Manager that has the domain key and certificate used to sign CSRs.

When you get a license for an evaluation mode API Gateway, you must use this option to change the host from `127.0.0.1` to a network reachable address or host name. You must also restart the Node Manager to pick up any changes.

Option `3`: `Delete a host`

Delete a registered host from a domain topology (used occasionally). You must first stop and delete all API Gateways running on the host. You can use this option to delete an Admin Node Manager or Node Manager. The Admin Node Manager that services this request is not allowed to delete itself from the domain, ensuring the domain always has at least one Admin Node Manager.

Option `4`: `Change Admin Node Manager and/or credentials, currently connecting as:user admin with truststore None`

By default, you connect to an Node Manager using `managedomain` with the credentials specified at installation time. You can override these at startup by passing the `--username --password` command line parameters, or reset while running `managedomain` with this option. This username/password refers to an `admin` user configured in Policy Studio.

You can also use this option to select which Admin Node Manager `managedomain` connects to. `managedomain` must talk to an Admin Node Manager, which can be local or remote. By default, `managedomain` connects to the local running Admin Node Manager, otherwise it searches the topology and uses the first running Admin Node Manager that it finds.

### API Gateway management

The `managedomain --menu` options for API Gateway management are as follows:

Option `5`: `Create API Gateway instance`

Create a new API Gateway instance. You can also do this in Policy Studio and API Gateway Manager. You can create API Gateway instances locally or on any host configured in the topology. You can choose to initialize the first API Gateway instance in a group with a YAML configuration.

Option `6`: `Edit API Gateway (rename, change management port)`

Rename the API Gateway instance, or change the management port. This functionality is not available in Policy Studio and API Gateway Manager.

Option `7`: `Delete API Gateway instance`

Delete an API Gateway instance from the topology, and optionally delete the files on disk. You can also do this in Policy Studio and API Gateway Manager. You must ensure that the API Gateway instance has stopped.

Option `8`: `Add a tag to API Gateway`

Add a name-value tag to the API Gateway. The **Topology** view on the API Gateway Manager **Dashboard** displays tags and enables you to filter for API Gateway instances by tag.

Option `9`: `Delete a tag from API Gateway`

Delete a name-value tag from the API Gateway. The tag will no longer be displayed in the API Gateway Manager **Dashboard**.

Option `10`: `Add or remove a Linux service for existing local API Gateway`

Must be run by a user with permission to create a service on the host operating system (`root` on Linux). When run, adds an `init.d` script.

### Group management

The `managedomain --menu` options for group management are as follows:

Option `11`: `Edit group (rename it)`

Rename an API Gateway group. This functionality is not available in Policy Studio and API Gateway Manager.

Option `12`: `Delete a group`

Delete all API Gateways in the group and the group itself. You must ensure that all API Gateways in the group have been stopped first.

### Topology management

The `managedomain --menu` options for topology management are as follows:

Option `13`: `Print topology`

Output the contents of the deployed domain topology. This includes the following:

* Topology version
* Hosts
* Admin Node Managers
* Node Managers
* Groups
* API Gateway instances (tags)

Option `14`: `Check topologies are in sync`

For advanced users. Check that all Node Managers are running the same topology version. Useful only in multi-host environment. Topologies should be in sync if everything is running correctly.

Option `15`: `Check the Admin Node Manager topology against another topology`

For advanced users. Compare the two topologies and highlights differences. There should be no differences if everything is running correctly.

Option `16`: `Sync all topologies`

For advanced users. Forces a synchronization of all topologies.

Option `17`: `Reset the local topology`

For advanced users. Delete the contents of the `apigateway/groups` directory. This means that you must re-register the host and recreate a local API Gateway instance. Alternatively, you can manually delete the contents of this directory to prevent issues if the host has been registered with other node managers.

### Deployment

The `managedomain --menu` options for deployment are as follows:

Option `18`: `Deploy to a group`

Deploy a configuration (`.fed` file) to API Gateways. This functionality is also available in Policy Studio and API Gateway Manager.

Option `19`: `List deployment information`

List the deployment information for all API Gateways in a topology. This functionality is also available in Policy Studio and API Gateway Manager.

Option `20`: `Create deployment archive`

Create a deployment archive from a directory that contains a federated API Gateway configuration.

Option `21`: `Download deployment archive`

Download the `.fed` file deployed to an API Gateway. This functionality is also available in Policy Studio.

Option `22`: `Update deployment archive properties`

Update the manifest properties relating to the deployed configuration only. This functionality is also available in Policy Studio. Enables you to update the properties without performing a new deployment.

Option `23`: `Change group configuration passphrase`

The default passphrase for the API Gateway configuration is `“”`. Use this option to set a more secure password. This functionality is also available in Policy Studio.

### Domain SSL certificates

The `managedomain --menu` options for group management are as follows:

Option `24`: `Regenerate SSL certificates on localhost`

Regenerate the SSL certificates used to secure API Gateway components in the domain (for example, Node Manager and the API Gateway instances that it manages). You must restart the Node Manager on the localhost after running this option. You must run this option on all hosts in the domain.

Option `25`: `Sign CSR`

Specify a Certificate Signing Request (CSR) to send to the Certificate Authority (CA) when applying for an SSL certificate for a Node Manager or API Gateway instance. You can use this option when `managedomain` acts as the CA, and is passed the CSR to create a signed certificate. You will most likely use an external CA in production. However, this option facilitates testing of certificates signed by an external CA. You can install the API Gateway on a locked-down host, and use this feature only (no license required). You would typically only do this when using a system-generated self-signed domain certificate, and do not wish to store the domain private key on an Admin Node Manager host, and do not wish to use an external CA.

Option `26`: `Submit externally signed certificate`

Specify an SSL certificate signed by an externally signed Certificate Authority (CA) to be used by a Node Manager or API Gateway instance. Use this option after registering a host or creating an API Gateway using a certificate signed by an external CA. Submitting the certificate with this option completes the host registration or API Gateway creation.

Note that API Gateway CSRs define three custom object identifiers (OID) that they expect the CA to issue in the certificate. The CA *must* issue the certificates with these OIDs, as well as the other key usage and extended key usage bits defined in the CSR, or the gateways will be unable to communicate. The custom OIDs are defined as follows:

```
ADMIN_NODE_MANAGER_OID = "1.3.6.1.4.1.17998.10.1.1.2.2"
NODE_MANAGER_OID = "1.3.6.1.4.1.17998.10.1.1.2.1"
GATEWAY_OID = "1.3.6.1.4.1.17998.10.1.1.2.3"
```

An Admin Node Manager's topology certificate will have both the `ADMIN_NODE_MANAGER_OID` and the `NODE_MANAGER_OID`, while a local Node Manager's topology certificate will have only the `NODE_MANAGER_OID` and a gateway instance's topology certificate will have only the `GATEWAY_OID` in its certificate.

## Command mode

You can also enter `managedomain` commands and parameters directly on the command line. For example, the following command creates an Admin Node Manager on the first host in the domain and signs with a user-provided domain key:

```
managedomain -i --sign_with_user_provided --ca=/home/keys/test.p12
```

{{< alert title="Note" color="primary" >}}You must run `managedomain -i` or `--initialize` to register the first host in the domain in order to create and run API Gateways.{{< /alert >}}

For details on all available commands, enter `managedomain --help`

For detailed examples of using `managedomain` in command mode, see the following:

* [Configure Admin Node Manager high availability](/docs/apim_administration/apigtw_admin/admin_node_mngr)
* [Find your installed version and list patches using managedomain](/docs/apim_administration/apigtw_admin/trblshoot_get_help#find-install-version)

## Provide credentials to the Admin Node Manager

You can use the following properties file to automatically provide admin user name and password credentials to authenticate to the Admin Node Manager:

```
INSTALL_DIR/apigateway/conf/managedomain.props
```

* You must ensure that the appropriate read and execute privileges for your operating system have been set for the `execute` file.
* You must also ensure that the `execute` and `managedomain.props` files are protected.

Perform the following steps:

1. Open the `managedomain.props` file in an editor.
2. Uncomment the `password_exec` property.
3. Ensure that the path to `../apigateway/conf/execute.sh` is correct.
4. Change the password echoed in `../apigateway/conf/execute.sh`.
5. Save your changes to the file.

Alternatively, you can provide credentials on the command line. The following example shows command mode:

```
managedomain.bat --print_toplogy --username <userName> --password <password>
```

The following example shows interactive mode:

```
managedomain --menu
Enter username: my_admin_user
Enter password: **********
```
