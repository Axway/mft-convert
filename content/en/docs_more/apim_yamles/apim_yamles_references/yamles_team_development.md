{
"title": "Team development with YAML configuration",
"linkTitle": "Team development with YAML configuration",
"weight":"90",
"date": "2020-09-25",
"description": "Use YAML configuration to convert projects, which were created using Policy Studio Team development functionality."
}

You can convert common and API projects, created using Policy Studio [Team development](/docs/apigtw_devops/team_dev_practices/#enable-team-development-in-policy-studio) functionality, into YAML configurations in the same way as any other XML federated configuration. The converted YAML projects will validate successfully, but they might need the `--allow-invalid-ref` option to pass validation because some of these projects might contain entities that reference entities in another project.

The `projpack` and `projgitupgrade` tools cannot be used with the YAML entity store. You can achieve the equivalent of `projpack` by using the [`yamles import`](/docs/apim_yamles/apim_yamles_cli/yamles_cli_importexport/#import-a-yaml-configuration-into-another-yaml-configuration) command, and the equivalent of `projupgrade` by using the [`yamles upgrade`](/docs/apim_yamles/apim_yamles_cli/yamles_cli_upgrade) command.

You can use the `projdeploy` tool with YAML configuration, but it requires a YAML `.tar.gz` file built using standard tooling. The design of the YAML configuration lends itself well to Team development for many use cases. The natural split of the configuration into separate files allows multiple developers to make changes on the same YAML configuration project without causing merge conflicts. The conflicts that do occur, are more easily understood and resolved as files are smaller and more human readable.

It is still necessary to support multiple YAML configuration projects for reuse purposes. Developers might want to develop policies once and reuse many times in other projects. For example, a company might have some common security or throttling policies that they wish to apply across all projects. Such policies can be developed once and imported into other projects at deployment time through a CI/CD pipeline. You can import many common projects into your main project before deployment by using the `yamles import` command multiple times.

The following example shows how a merged `.tar.gz` file can be created for deployment from two separate projects. Let's assume our starting point is a pair of projects that were created in XML format in Policy Studio using the Team Development templates, one using the `Common Project` and the other using the `API Project`.

The `common-security` project created using the `Common Project` template contains a `Common security` policy as follows:

![Team development](/Images/apim_yamles/yamles_team_dev.png)

* The `test-api` project, created using the **API Project** template contains a simple policy named `Test`, with a `Set Message` and `Reflect` filter.
* A listener on port `8080` on path `/test` is linked to this policy.
* The `common-security` project is added as a Team development project dependency via **File > Manage Dependencies**.
* The `Common security` policy is selected as a `Global Request Policy` (Right-click the policy as selecting **Set as Global Request Policy**).

![Team development](/Images/apim_yamles/yamles_team_dev_2.png)

Both projects can be converted separately to YAML using the `yamles fed2yaml` option.

```
./yamles fed2yaml --source federated:file:/home/user/apiprojects/common-security/configs.xml -o ~/team-dev/common-security --targz ~/team-dev/archives/common-security.tar.gz
./yamles fed2yaml --source federated:file:/home/user/apiprojects/test-api/configs.xml -o ~/team-dev/test-api --targz ~/team-dev/archives/test-api.tar.gz
```

They may be separately validated as follows:

```
./yamles validate --source yaml:file:/home/user/team-dev/common-security
./yamles validate --source yaml:file:/home/user/team-dev/test-api --allow-invalid-ref
```

Note the use of `--allow-invalid-ref` for the `test-api` project as it has a reference to an entity in the `common-security` project in the `System/Global Properties.yaml` file:

```yaml
---
type: GlobalProperties
fields:
  name: Global Properties
children:
- type: ReferenceProperty
  fields:
    name: system.policy.request
    value: /Policies/Common security
```

The `/Policies/Common security.yaml` file exists in the `common-security` project.

CI/CD pipelines for the `common-security` and `test-api` projects might each publish a `.tar.gz` into Artifactory. These need to get merged before deployment using the `yamles import` command.

```
./yamles import --source ~/team-dev/archives/common-security.tar.gz --target ~/team-dev/archives/test-api.tar.gz --allow-invalid-ref --allow-invalid-cardinality
```

The updated `test-api.tar.gz` will contain the policies from the `test-api` and `common-security` projects. The `test-api.tar.gz` should validate without the `--allow-invalid-ref option`:

```
./yamles validate --targz ~/team-dev/archives/test-api.tar.gz
```

The `test-api.tar.gz` can be deployed in the usual way via `managedomain` or `projdeploy`.

Currently, the import command does not support project dependencies in the same way as XML. You must ensure the final project has all references resolved. This can be verified using the `yamles validate` command. The ordering and use of `addIfAbsent` and `addOrReplace` in the [`_fragment.yaml`](/docs/apim_yamles/apim_yamles_cli/yamles_cli_importexport/#use-the-_fragmentyaml-file-to-export) file determines how to resolve conflicts.
