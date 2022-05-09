{
"title": "Automate tasks with Jython scripts",
"linkTitle": "Automate tasks with Jython scripts",
"weight":"90",
"date": "2019-11-27",
"description": "Learn more about API Gateway Jython sample scripts that you can use to automate common administration tasks."
}

API Gateway contains several sample scripts that let you automate various common administration tasks. The scripts are based on the Java scripting interpreter, Jython ([http://www.jython.org](http://www.jython.org/)). Scripts can be extended to suit your needs by following the Jython language syntax. All Jython scripts can be found in the following location:

```
INSTALL_DIR/apigateway/samples/scripts
```

To run a sample script, you can call the `run` shell in this directory and point it to the script to be run. For example, to run the `changeTrace.py` sample script:

```
cd INSTALL_DIR/apigateway/samples/scripts
./run.sh config/changeTrace.py
```

## Jython scripts

The following summarizes the Jython scripts that are available in the API Gateway installation.

### `analyze`

This category contains scripts to:

* Print a list of all references in the API Gateway. For each reference it shows what store it originates from and to.
* Check if the API Gateway is locked down.
* Get a list of unresolved references between entities in the API Gateway.

### `apikeys`

This category contains scripts to:

* Send an API key and associated secret as query parameters.
* Send an API key only as a query parameter.
* Fetch a URL with HTTP basic authentication.
* Send a signed query string.
* Send an authenticated REST request.

### `cassandra`

This category contains scripts to convert all Cassandra-based data stores to file-based data stores and redeploy.

### `certs`

This category contains scripts to add a new certificate to the certificate store from different sources.

### `config`

This category contains scripts to:

* Remove the sample service listeners and policies from the API Gateway.
* Connect to a particular process and toggle tracing for the management port.
* Connect to the API Gateway and set an address to bind to on a given port of a given interface.
* Update the `maxInputLen`, `maxOuputLen`, and `maxRequestMemory` configuration in Node Managers and API Gateways.

### `environmentalize`

This category contains scripts to:

* Mark fields for environmentalization and create associated environment setting entries.
* Get the fields that have been marked for environmentalizing and output the associated value in environment settings.
* Remove fields marked as environmentalized.
* Create a deployment archive from a policy package and an environment package.

### `io`

This category contains scripts to import and export entities to and from an API Gateway configuration.

### `json`

This category contains scripts to generate a JSON schema for a fully qualified class name.

### `migrate`

This category contains scripts to:

* Download the current `.fed`, `.pol` and `.env` archives via Node Manager from an API Gateway.
* Create a deployment package from policy and environment packages. They can be obtained from a running API Gateway, from a source code repository (for example, Git or SVN), or via USB or FTP, and so on.
* Demonstrate a promotion of configuration from the development environment to the staging environment.

### `monitor`

This category contains scripts to:

* Print the filter details of each policy executed in a transaction.
* Print the success or failure status of each filter in a transaction.

### `oauth`

This category contains scripts to provide OAuth 2.0 support.

### `passport`

This category contains scripts to create an Axway PassPort CSD based on the API service configuration. An Axway Component Security Descriptor (CSD) file is used when registering with Axway PassPort.

### `publish`

This category contains scripts to add an entity type and any defined instances to an associated entity store.

### `unpublish`

This category contains scripts to remove an entity type and any defined instances from an associated entity store.

### `topology`

This category contains scripts to:

* Create API Gateway instances in a group.
* Get the domain topology from the Admin Node Manager.
* Get the domain topology and output the IDs of the API Gateway instances.

### `securityconstraints`

This category contains scripts to check a configuration for FIPS, SuiteB, or SuiteBTS compliance.

### `users`

This category contains scripts to connect to a particular process and add a new user to the user store.

### `ws`

This category contains scripts to:

* Register a WSDL in an API Gateway.
* List the web services in an API Gateway.
* Remove a registered service from the API Gateway.
