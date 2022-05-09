{
"title": "Package and deploy a YAML configuration",
"linkTitle": "Package and deploy",
"weight":"80",
"date": "2020-09-24",
"description": "Learn how to package and deploy a YAML configuration to the API Gateway runtime."
}

{{< alert title="Note">}}When a YAML configuration is deployed to an API Gateway group, it is still possible to switch it back and deploy an XML federated configuration to that same API Gateway group. When an API Gateway instance is first created, an XML federated or YAML configuration is deployed with it.{{< /alert>}}

## Create an API Gateway initialized with a YAML factory configuration

You can use the `managedomain` command to [create API Gateway instances](/docs/apim_administration/apigtw_admin/makegateway/#create-an-api-gateway-instance). The first API Gateway in a group is created by default with a XML federated configuration. To create an API Gateway instance with a YAML configuration, run the following command:

```
./managedomain --create_instance --yaml --group TestGroup -n MyGateway
```

You don't need to use the `--yaml` option to create subsequent API Gateways, as the existing configuration of the group will be used.

## Build the deployment package

After you create a YAML configuration, you must build a deployment package before deploying the YAML configuration to API Gateway.

The deployment package is a `.tar.gz` file. This is the equivalent of the `.fed` file for an XML federated configuration. You can use any standard tooling to build a `.tar.gz` file that contains the content of the directory of the YAML configuration.

The `.tar.gz` file must have the following structure inside:

![YAML deployment package structure](/Images/apim_yamles/yamles_package.png)

{{< alert title="Note">}}The root directory of the YAML configuration cannot be included in the `.tar.gz` file.{{< /alert>}}

### Use command line to build the YAML.tar.gz

To build a `yaml-config.tar.gz` package for the YAML configuration in a directory named `~/yamlconfig`, follow this example:

```
cd ~/yamlconfig && tar -zcvf ../yaml-config.tar.gz * && cd ~
```

After running the command, the `yaml-config.tar.gz` file is created in your home directory.

### Use maven to build the YAML.tar.gz

You can also use the `maven-assembly-plugin` to generate a `.tar.gz` file via `mvn clean install`.

The following is an example of how to configure a project to use maven to build the `.tar.gz`. The folder structure is as follows:

```
+---project
    +---src
        +---assembly
            +archive.xml
        +---main
             +---APIs
             +---Environment Configuration
             +---External Connections
             +---Libraries
             +---META-INF
             +---Policies
             +---Resources
             +---Server Settings
             +---System
             +_parent.yaml
    +pom.xml
```

The following is an example of the `pom.xml` file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <modelVersion>4.0.0</modelVersion>

  <parent>
    <groupId>com.axway.parent</groupId>
    <artifactId>parent</artifactId>
    <version>1.24.0</version>
  </parent>

  <artifactId>yaml-gateway-config-demo</artifactId>
  <packaging>pom</packaging>
  <version>7.7.0-SNAPSHOT</version>

  <build>
    <plugins>
      <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <version>2.5.4</version>
        <executions>
          <execution>
            <id>make-assembly</id>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
            <configuration>
              <tarLongFileMode>gnu</tarLongFileMode>
              <descriptors>
                <descriptor>src/assembly/archive.xml</descriptor>
              </descriptors>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
</project>
```

The following is an example of the `archive.xml` file for the maven plugin:

```xml
<assembly
xmlns="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0 http://maven.apache.org/xsd/assembly-1.1.0.xsd">
  <id>bundle</id>
  <formats>
    <format>tar.gz</format>
  </formats>
  <includeBaseDirectory>false</includeBaseDirectory>
  <baseDirectory>${project.basedir}</baseDirectory>
  <fileSets>
    <fileSet>
      <directory>src/main</directory>
      <outputDirectory>.</outputDirectory>
      <useDefaultExcludes>false</useDefaultExcludes>
      <includes>
        <include>**</include>
      </includes>
    </fileSet>
  </fileSets>
</assembly>
```

Running `mvn clean install` in the root directory of this project creates the `yaml-gateway-config-demo-7.7.0-SNAPSHOT-bundle.tar.gz` in the target directory.

## Deployment package Properties

The API Gateway `.fed`, `.pol`, and `.env` configuration package files include property files (`.mf`) that contain name-value pairs, also known as package properties, which describe the package contents.

When the XML federated configuration is converted to YAML, the `.mf` files are copied into the `META-INF` directory, in the YAML configuration directory structure. By default, the following files are created after the conversion:

* `manifest.mf`
* `manifest-policy.mf`
* `manifest-environment.mf`

You can modify any of these files before the deployment using an editor or via a script as part of your deployment process.

You can remove the `manifest-policy.mf` and `manifest-environment.mf` files that are created after conversion if you no longer require a separation between policy and environment variables.

The `manifest.mf` file is required. It contains two read-only properties, `timestamp` and `format`. If you delete this file, it will be recreated on the API Gateway side after deployment. You can delete the read-only properties and add custom properties. However, we recommended you to add any custom properties to either the `manifest-policy.mf` or `manifest-environment.mf`, or even to a new custom manifest file, for example `META_INF/custom.mf`.

## Deploy the deployment package

You can deploy YAML configuration by way of the following methods:

* Policy Studio
* API Gateway Manager UI
* A command line deployment tools, such as `managedomain` or `projdeploy`

### Deploy using Policy Studio

For information on how to deploy YAML configuration via Policy Studio, see [Deploy configuration in Policy Studio](/docs/apim_administration/apigtw_admin/deploy_get_started/#deploy-configuration-in-policy-studio).

### Deploy using API Gateway Manager UI

For information on how to deploy YAML configuration in the API Gateway Manager web UI, on port `8090`, see [Deploy a YAML deployment package](/docs/apim_administration/apigtw_admin/managetopology/#deploy-a-yaml-deployment-package).

### Deploy using managedomain

Before you use `managedomain` for deploying, ensure that the YAML `.tar.gz` is encrypted using the same passphrase that the API Gateway uses to decrypt its configuration. The configuration is unencrypted by default. If you converted an XML federated configuration that was encrypted, it is still encrypted by the same passphrase in the converted YAML format.

To deploy `~/archives/yaml-config.tar.gz` via `managedomain` to an API Gateway group, run the following command:

```
./managedomain --deploy --group TestGroup --archive_filename ~/archives/yaml-config.tar.gz --username admin --password changeme
```

### Deploy using projdeploy

You can encrypt, reencrypt, or decrypt the YAML `.tar.gz` file with the `projdeploy` tool before deploying to an API Gateway group.

See the following examples on how to deploy the `~/archives/yaml-config.tar.gz` file using `projdeploy` to an API Gateway group:

When the `.tar.gz` is unencrypted and the API Gateway group is using no passphrase for decryption.

```
./projdeploy --name yaml-config --dir archives --group TestGroup --type targz --passphrase-none --change-pass-to "fred" --deploy-to --host-name=localhost --port=8090 --user-name=admin --password=changeme
```

When the `.tar.gz` is unencrypted and the API Gateway group is using a passphrase of "changeme" for decryption.

```
./projdeploy --name yaml-config --dir archives --group TestGroup --type targz --passphrase-none --change-pass-to "changeme" --deploy-to --host-name=localhost --port=8090 --user-name=admin --password=changeme
```

When the `.tar.gz` is encrypted using `targz-passphrase` and the API Gateway group is using a passphrase of `group-passphrase` for decryption.

```
./projdeploy --name yaml-config --dir archives --group TestGroup --type targz --passphrase "targz-passphrase" --change-pass-to "group-passphrase" --deploy-to --host-name=localhost --port=8090 --user-name=admin --password=changeme
```

When the `.tar.gz` is encrypted using `targz-passphrase` and the API Gateway group is using no passphrase for decryption.

```
./projdeploy --name yaml-config --dir archives --group TestGroup --type targz --passphrase "targz-passphrase" --change-pass-to-none --deploy-to --host-name=localhost --port=8090 --user-name=admin --password=changeme
```

See [projdeploy](/docs/apigtw_devops/deploy_package_tools/#example-projdeploy-use-cases) for more details on other `projdeploy` options. The options `--apply-env`, `--polprop`, `--envprop` do not apply for YAML configurations.

## Manage your domain topology when YAML is deployed

The API Gateway Manager UI and `managedomain` script will work as normal for topology management when YAML is deployed, with some exceptions as described in this section.

For example, you can:

* Continue to use `managedomain` to change a group passphrase when YAML is deployed (`--change_passphrase` option).
* Download the currently deployed YAML `.tar.gz` (`–download_archive` option).
* Use API Gateway Manager UI to check if a group is consistent, and restart API Gateway instances.

Refer to [Manage domain topology in API Gateway Manager](/docs/apim_administration/apigtw_admin/managetopology/) and [managedomain command reference](/docs/apim_reference/managedomain_ref/) for more information on these tools.

### Update deployment archive properties

You can use option `22` of `managedomain` to update the deployment package properties without performing a deployment. This is not supported for YAML deployments. Instead, edit the contents of the manifest files in the `META_INF`, then redeploy it.
