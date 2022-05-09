{
"title": "Promotion and deployment scripts",
"linkTitle": "Promotion and deployment scripts",
"weight":"89",
"date": "2019-11-19",
"description": "Use sample scripts to automate environmentalizing and promoting API Gateway configuration."
}

The API Gateway provides sample scripts to automate various promotion and deployment tasks. These scripts are based on the Jython Java scripting interpreter (see [http://www.jython.org](http://www.jython.org/)). You can extend these scripts to suit your needs by using the Jython language syntax. All Jython sample scripts are found in the following directory in your API Gateway installation:

```
INSTALL_DIR/samples/scripts
```

For details on packaging and deployment tools, see [Packaging and deployment tools](/docs/apigtw_devops/deploy_package_tools).

{{< alert title="Note" color="primary" >}}All scripts and tools mentioned in this page pertain only to `.fed` or XML based projects, and are not applicable to YAML. For YAML-specific details, see [Package and deploy a YAML configuration](/docs/apim_yamles/yamles_packaging_deployment/).{{< /alert >}}

## Run the sample scripts

To run a sample script, call the `run` shell in the `/samples/scripts` directory, and specify the script to run. For example:

```
sh run.sh config/getEnvSettings.py
```

## Scripts for environmentalizing configuration

You can use the following scripts in the `/sample/scripts/environmentalize` directory to environmentalize API Gateway configuration:

| **Script name**        | **Description**                           |
|------------------------|-----------------------------------------|
| `addEnvSettings.py`    | Downloads a deployment package (`.fed`) from an API Gateway. Marks the **Traffic HTTP Interface** port field to be environmentalized. Creates an environment settings entry for the port, and sets it to `7878`.                                                                                                         |
| `getEnvSettings.py`    | Connects to an API Gateway and lists all the fields that have been marked for environmentalization. The associated values in environment settings are output.                                                         |
| `mergeEnvSettings.py`  | Offline script that does not connect to an API Gateway. Merges a policy package (`.pol`) from downstream with an environment package (`.env`) from upstream, and merges them to create a deployment package (`.fed`). |
| `removeEnvSettings.py` | Downloads a deployment package (`.fed`) from an API Gateway. Removes the **Traffic HTTP Interface** port field from being environmentalized (opposite of the `addEnvSettings.py`script).        |

## Scripts for promoting configuration

You can use the following scripts in the `/sample/scripts/migrate` directory to promote API Gateway configuration:

| **Script name**              | **Description**     |
|------------------------------|-----------------------------------------------|
| `archive.py`                 | Downloads the current API Gateway deployment package (`.fed`), policy package (`.pol`), and environment package (`.env`) files from the Node Manager. |
| `createDeploymentPackage.py` | Creates a deployment package (`.fed`) from policy (`.pol`) and environment (`.env`) packages. Where the policy and environment packages are obtained from is out of scope for this script. For example, they could have been obtained from a running server (see `archive.py`), a source code repository (CVS, Git, SVN, and so on), or an FTP or USB connection. |
| `envMigrate.py`              | Demonstrates the promotion of configuration from a development environment to a staging environment.   |
