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

### Use command line tooling to build the YAML .tar.gz

To build a `yaml-config.tar.gz` package for the YAML configuration in a directory named `~/yamlconfig`, follow this example:

```
cd ~/yamlconfig && tar -zcvf ../yaml-config.tar.gz * && cd ~
```

After running the command, the `yaml-config.tar.gz` file is created in your home directory.

### Use maven to build the YAML .tar.gz

You can also use the `maven-assembly-plugin` to generate a `.tar.gz` file via `mvn clean install`. The following is an example of how to configure a project to use maven to build the `.tar.gz`. The folder structure is as follows:

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

The following is the body of the `pom.xml`:

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

The `archive.xml` for the maven plugin:

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

The result of running `mvn clean install` in the root directory of this project is a `.tar.gz` file in the target directory, named `yaml-gateway-config-demo-7.7.0-SNAPSHOT-bundle.tar.gz`.

## Deployment Package Properties

The API Gateway `.fed`, `.pol`, and `.env` configuration package files include property files that contain name-value pairs describing the package contents and which are known as package properties. These package property values are stored in package property files (`.mf`). When the XML federated configuration is converted to YAML, the `.mf` files are copied as they are into the `META-INF` directory on the YAML configuration directory structure. By default, `manifest.mf`, `manifest-policy.mf`, and `manifest-environment.mf` files are created after the conversion. These files can be used as before to hold any user-defined name-value properties for the deployment package. You can edited the files in an IDE of your choice, or via a CI/CD job before deployment.

You can remove the existing `manifest-policy.mf` and `manifest-environment.mf` files that are created after conversion if the split no longer makes sense for your deployments. If you remove the `manifest.mf`, it will be recreated on the API Gateway side after deployment, and it contains read-only properties, `timestamp` and `format`.

You can add custom properties to the `manifest.mf` file, but do delete the read-only properties. We recommended you to add any custom properties to either the `manifest-policy.mf` or `manifest-environment.mf`, or even a new custom manifest file, for example `META_INF/custom.mf`.

## Deploy the deployment package

Deployment of YAML configuration via Policy Studio or the API Gateway Manager UI is not yet supported, you must use command line tools instead. For example, `managedomain`, or `projdeploy`.

### Use managedomain to deploy a YAML configuration

Before you use `managedomain` for deploying, ensure that the YAML `.tar.gz` is encrypted using the same passphrase that the API Gateway uses to decrypt its configuration. The configuration is unencrypted by default. If you converted an XML federated configuration that was encrypted, it is still encrypted by the same passphrase in the converted YAML format.

To deploy `~/archives/yaml-config.tar.gz` via `managedomain` to an API Gateway group, run the following command:

```
./managedomain --deploy --group TestGroup --archive_filename ~/archives/yaml-config.tar.gz --username admin --password changeme
```

### Use projdeploy to deploy a YAML configuration

You can encrypt, reencrypt, or decrypt the YAML `.tar.gz` file with the `projdeploy` tool before deploying to an API Gateway group.

Deploy `~/archives/yaml-config.tar.gz` via `projdeploy` to an API Gateway group when the `.tar.gz` is unencrypted and the API Gateway group is using no passphrase for decryption.

```
./projdeploy --name yaml-config --dir archives --group TestGroup --type targz --passphrase-none --change-pass-to "fred" --deploy-to --host-name=localhost --port=8090 --user-name=admin --password=changeme
```

Deploy `~/archives/yaml-config.tar.gz` via `projdeploy` to an API Gateway group when the `.tar.gz` is unencrypted and the API Gateway group is using a passphrase of "changeme" for decryption.

```
./projdeploy --name yaml-config --dir archives --group TestGroup --type targz --passphrase-none --change-pass-to "changeme" --deploy-to --host-name=localhost --port=8090 --user-name=admin --password=changeme
```

Deploy `~/archives/yaml-config.tar.gz` via `projdeploy` to an API Gateway group when the `.tar.gz` is encrypted using `targz-passphrase` and the API Gateway group is using a passphrase of `group-passphrase` for decryption.

```
./projdeploy --name yaml-config --dir archives --group TestGroup --type targz --passphrase "targz-passphrase" --change-pass-to "group-passphrase" --deploy-to --host-name=localhost --port=8090 --user-name=admin --password=changeme
```

Deploy `~/archives/yaml-config.tar.gz` via `projdeploy` to an API Gateway group when the `.tar.gz` is encrypted using `targz-passphrase` and the API Gateway group is using no passphrase for decryption.

```
./projdeploy --name yaml-config --dir archives --group TestGroup --type targz --passphrase "targz-passphrase" --change-pass-to-none --deploy-to --host-name=localhost --port=8090 --user-name=admin --password=changeme
```

See [projdeploy](/docs/apigtw_devops/deploy_package_tools/#example-projdeploy-use-cases) for more details on other `projdeploy` options. The options `--apply-env`, `--polprop`, `--envprop` do not apply for YAML configurations.

## Manage your domain topology when YAML is deployed

The API Gateway Manager UI and `managedomain` script will work as normal for topology management when YAML is deployed with a few exceptions described in this section.

For example, you can:

* Continue to use `managedomain` to change a group passphrase when YAML is deployed (`--change_passphrase` option).
* Download the currently deployed YAML `.tar.gz` (`–download_archive` option).
* Use API Gateway Manager UI to check if a group is consistent, and restart API Gateway instances.

Refer to [Manage domain topology in API Gateway Manager](/docs/apim_administration/apigtw_admin/managetopology/) and [managedomain command reference](https://axway-open-docs.netlify.app/docs/apim_reference/managedomain_ref/) for more information on these tools.

### Update deployment archive properties

You can use option `22` of `managedomain` to update the deployment package properties without performing a deployment. This is not supported for YAML deployments. Instead, edit the contents of the manifest files in the `META_INF`, then redeploy it.
