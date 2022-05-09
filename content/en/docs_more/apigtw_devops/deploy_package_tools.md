{
"title": "Packaging and deployment tools",
"linkTitle": "Packaging and deployment tools",
"weight":"90",
"date": "2019-11-19",
"description": "Use the package and deployment tools to automate packaging, deploying, and upgrading configuration for continuous integration."
}

You can run the package and deployment tools from the following directory:

```
INSTALL_DIR/apigateway/posix/bin
```

For example, to run the `projpack --help` command:

```
cd /opt/Axway-7.7/apigateway/posix/bin
projpack --help
```

For a full listing of command options for the tools, see [Continuous integration tools reference](/docs/apim_reference/devopstools_ref).

## Generate configuration packages from API Gateway projects

The `projpack` tool enables you to use automatic processes to generate API Gateway configuration packages from multiple API Gateway projects. For example, you can automatically generate deployment packages (`.fed`), policy packages (`.pol`), and environment packages (`.env`), which you can then use to promote APIs and policy configuration to upstream environments.

### Example projpack use cases

The following examples describe how you might use `projpack`.

#### Create encrypted policy and environment packages from a single project

The following example shows how to create an encrypted policy package (`.pol`) and environment package (`.env`) from a specified API Gateway project:

```
projpack --create --passphrase=my_text --name=my_package --add /home/user1/apiprojects/proj1 --projpass=my_text
```

The `--passphrase` and `--projpass` options are required, or you can use `--passphrase-none` and `--projpass-none`.

#### Create an encrypted deployment package from multiple projects

The following example shows how to create a deployment package (`.fed`) from multiple API Gateway projects:

```
projpack --create --dir=/home/jbloggs/testfeds/ --passphrase=my_text --name=dev.fed --type=fed --add /home/jbloggs/apiprojects/proj1 /home/jbloggs/apiprojects/proj2 --projpass-none
```

#### Create an encrypted deployment package from multiple projects using a passphrase file

The following example shows how to create a deployment package (`.fed`) from multiple API Gateway projects, where the passphrases are supplied in a passphrase file:

```
projpack --create --dir=/home/jbloggs/testfeds/ --name=dev.fed --type=fed --add /home/jbloggs/apiprojects/proj1 -add /home/jbloggs/apiprojects/proj2 --passfile=/home/jbloggs/passfile.txt
```

For more details on the passphrase file format, see [Read passphrases from a file](#read-passphrases-from-a-file).

#### Import project configuration into an existing deployment archive

The following example shows how to import configuration from specified API Gateway projects into a specified deployment package:

```
projpack --import --replace --dir=/home/user1/testfeds/ --passphrase=my_text --name=my_package --type=fed --add /home/user1/apiprojects/proj1 --projpass=my_text1 --add /home/user1/apiprojects/proj2 --projpass=my_text2
```

{{< alert title="Note" color="primary" >}}You can use the `--replace` option before `--add` to override conflicts. Otherwise, if conflicts are found the command exits. {{< /alert >}}

### Read passphrases from a file

The `projpack` command provides the`--passfile` or `-f` option to specify the location of a passphrases file. This file contains a passphrase for the generated target configuration package and passphrases for the projects to be packaged.

The section names in the passphrase file are predefined. The names of the keys in a section are arbitrary, but must be unique in that section. For example:

```
[target]
pp=my_text

[projects]
p1=my_text1
p2=my_text2
```

For security reasons, this file should be protected with appropriate permissions. For example, the following command changes the file ownership:

```
sudo chown root: <passphrases_file>
```

The following command specifies file permissions:

```
sudo chmod 400 <passphrases_file>
```

For more examples, run `projpack --help`.

## Build and deploy API Gateway configurations

Environment settings are subject to change during the development, test, and production phases. The `projdeploy` tool enables you to use automated processes to build API Gateway configurations, to apply or modify the environment settings of a specified configuration package (`.fed` or `.pol`), and to deploy the package to specified API Gateway group instances.

### Example projdeploy use cases

The following examples describe how to use `projdeploy`.

#### Deploy a deployment package on a local host

The following simple example shows how to deploy a specified deployment package (`.fed`) to a locally running API Gateway instance:

```
projdeploy --passphrase-none --name=test.fed
```

#### Deploy a deployment package with new environment settings and passphrase

The following example shows how to deploy a specified deployment package and apply a new passphrase and environment settings on a local host:

```
projdeploy --dir=/tests --passphrase=pass --name=my_package --type=fed --change-pass-to=newpass --apply-env=/tests/prod/prod.env
```

#### Deploy a policy package to a specified host and group

The following example shows how to deploy a specified policy package and apply new settings on a specified Admin Node Manager host and port, and API Gateway group:

```
projdeploy --dir=/tests --passphrase=pass --name=mypackage --type=pol --change-pass-to-none --apply-env=/tests/prod/prod.env --deploy-to --host-name=myhost --port=myport --user-name=myname --password==mypass --group-name=mygroup
```

#### Deploy a deployment package to specified instances

The following example shows how to deploy a specified deployment package and apply new settings on a specified Admin Node Manager host, port, and set of API Gateway instances:

```
projdeploy --dir=/tests --passphrase=pass --name=mypackage --type=fed --deploy-to --host-name=myhost --port=myport --user-name=myname --password==mypass --group-name=mygroup --includes mygateway1 mygateway2 mygateway3
```

For more examples, run `projdeploy --help`.

## Change the passphrase of an API Gateway project

The `projchangepass` tool enables you to change the passphrase of an API Gateway project.

### Example projchangepass use cases

The following examples describe how you might use `projchangepass`.

#### Change the passphrase of a project

This example shows how to change the project passphrase on proj1 from `changeme` to `newpassPhrase`:

```
projchangepass --proj=/home/user1/apiprojects/proj1 --oldpass=changeme --newpass=newpassPhrase --confirmpass=newpassPhrase
```

#### Change the passphrase of a project using a passphrase file

This example shows how to change the project passphrase on proj1 using the contents in `passfile.txt`:

```
projchangepass --proj=/home/user1/apiprojects/proj1 --passfile=/home/user1/passfile.txt
```

Example `passfile.txt`:

```
[currentProj]
oldPP="changeme"
newPP=newpassPhrase
confirmPP=newpassPhrase
```

For more examples, run `projchangepass --help`.

## Upgrade an API Gateway project

The `projupgrade` tool enables you to upgrade one or more API Gateway projects from earlier versions (7.5.1 or later) to version 7.7. For example, after using `sysupgrade` to upgrade your API Gateway installation to version 7.7, you must upgrade the configuration projects in your development environment, or you cannot deploy them to your upgraded API Gateways.

You can use the `projupgrade` tool to upgrade a number of configuration projects at once. These projects might be independent of one another, or could include shared projects and their dependencies.

{{< alert title="Note" color="primary" >}}`projupgrade` upgrades all of the projects in a particular directory. If you keep projects in different source directories, remember to run `projupgrade` on all of them.{{< /alert >}}

After using `projupgrade` to upgrade your projects, you can:

* Open these projects in Policy Studio 7.7 and continue working as before.
* Deploy them to an API Gateway 7.7 instance.
* Merge the projects to create configuration packages (`.fed` or `.pol`/`.env`) using `projpack`.

### Example projupgrade use cases

The following examples describe how you might use `projupgrade`.

#### Back up and upgrade projects

This example shows how to upgrade the projects contained in a specified directory:

```
projupgrade --projdir=/home/user1/apiprojects --backupdir=/home/user1/backup
```

Before the upgrade, the projects are backed up to the specified backup directory. As no passphrases are specified the command uses a zero-length string as the passphrase for all projects.

#### Upgrade projects with different passphrases using a passphrase file

This example shows how to upgrade the projects contained in a specified directory, and how to specify the passphrases with the `--projpass` and `--passfile` options:

```
projupgrade --projdir=/home/user1/apiprojects --projpass=mypass --passfile=/tmp/passfile.txt
```

Example `passfile.txt`:

```
[projects]
Project 1=
Project 2=My passphrase!
```

The passphrase `mypass` is used for any projects not listed in `passfile.txt`, while the passphrases specified in the file are used for the projects listed in `passfile.txt`.

{{< alert title="Note" color="primary" >}}`projupgrade` accepts the same passphrase file format as `projpack`, but only the section shown in the example above is actually used.{{< /alert >}}

### projupgrade output

The `projupgrade` command produces the following outputs:

* `projupgrade.log` – A log of any errors or warnings generated by the `projupgrade` command.
* `projupgrade.report` – A summary report of the projects upgraded.

For example, the following command is run on a project directory containing two projects:

```
projupgrade --projdir=/home/user1/apiprojects --backupdir=/tmp --tracedir=/tmp
```

The following is an example of the `projupgrade.report`:

```
# ProductName=projupgrade 7.5.3-2017-01-17 rev. 4712b77 (Linux.x86_64)
# CurrentDate=Mon, 23 Jan 2017 15:59:49 +0000
# CurrentDateUTC=1485187189
# TZ=GMT

REPORT 23/Jan/2017:15:59:49.876 ... Tracing to directory: /tmp/projupgrade_logs_2017-01-23_15-59-49 filename: projupgrade.report
REPORT 23/Jan/2017:15:59:49.876 ... ===================
REPORT 23/Jan/2017:15:59:49.876 ... Running projupgrade
REPORT 23/Jan/2017:15:59:49.876 ... ===================
REPORT 23/Jan/2017:15:59:49.876 ... Projects directory : /home/user1/apiprojects
REPORT 23/Jan/2017:15:59:49.876 ... Backup directory : /tmp/projupgrade_backup_2017-01-23_15-59-49
REPORT 23/Jan/2017:15:59:49.876 ... Logging directory : /tmp/projupgrade_logs_2017-01-23_15-59-49
REPORT 23/Jan/2017:15:59:49.876 ...
REPORT 23/Jan/2017:15:59:49.876 ... Skipping non-project directory: /home/user1/apiprojects
REPORT 23/Jan/2017:15:59:49.877 ...
REPORT 23/Jan/2017:15:59:49.882 ... Upgrading project 'Project1': /home/user1/apiprojects/Project1 ...
REPORT 23/Jan/2017:15:59:49.885 ... Upgrading 'federated:file:////home/user1/apiprojects/Project1/configs.xml'
and putting output into '/home/user1/dev/temp/tmpB3F1P5'...
REPORT 23/Jan/2017:15:59:50.726 ... Writing upgrade to 'federated:file:/home/user1/dev/temp/tmpB3F1P5/configs.xml', please wait...
REPORT 23/Jan/2017:15:59:53.650 ... Migrating TivoliSettings from version 0 to version 1
REPORT 23/Jan/2017:15:59:53.651 ... Downgrade the cardinality on version field so we can remove it later
REPORT 23/Jan/2017:15:59:59.641 ... ... Upgrade successful.
REPORT 23/Jan/2017:15:59:59.863 ... OK
REPORT 23/Jan/2017:15:59:59.864 ...
REPORT 23/Jan/2017:15:59:59.864 ... Upgrading project 'Project2': /home/user1/apiprojects/Project2 ...
REPORT 23/Jan/2017:15:59:59.866 ... Upgrading 'federated:file:////home/user1/apiprojects/Project2/configs.xml'
and putting output into '/home/user1/dev/temp/tmp2Pd4Dz'...
REPORT 23/Jan/2017:16:00:00.005 ... Problem connecting to store:
REPORT 23/Jan/2017:16:00:00.005 ... Invalid group passphrase
ERROR 23/Jan/2017:16:00:00.005 ... FAILED: 1
REPORT 23/Jan/2017:16:00:00.005 ...
REPORT 23/Jan/2017:16:00:00.005 ... projupgrade summary
REPORT 23/Jan/2017:16:00:00.005 ... ===================
REPORT 23/Jan/2017:16:00:00.005 ... Number of projects successfully upgraded: 1
REPORT 23/Jan/2017:16:00:00.005 ... Number of projects where upgrade failed: 1
REPORT 23/Jan/2017:16:00:00.005 ...
REPORT 23/Jan/2017:16:00:00.005 ... Projects where upgrade failed:
REPORT 23/Jan/2017:16:00:00.005 ... /home/user1/apiprojects/Project2
REPORT 23/Jan/2017:16:00:00.005 ...
REPORT 23/Jan/2017:16:00:00.005 ... The contents of these projects have not been changed.
REPORT 23/Jan/2017:16:00:00.005 ... Fix the problems identified in the logs and rerun projupgrade.
REPORT 23/Jan/2017:16:00:00.005 ... Logging directory: /tmp/projupgrade_logs_2017-01-23_15-59-49
```

The output shows that Project1 was upgraded successfully, but Project2 failed to upgrade because of an `"Invalid group passphrase"`. To fix this, you could write the Project2 passphrase to a file and rerun `projupgrade` with the `--passfile` option.

For more examples, run `projupgrade --help`.
