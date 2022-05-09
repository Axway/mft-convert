{
"title": "Import and export YAML configuration using CLI",
"linkTitle": "Import and export YAML configuration using CLI",
"weight":"60",
"date": "2020-11-11",
"description": "Learn how to use the YAML configuration CLI to import and export YAML configuration."
}

A YAML configuration fragment is created by exporting from a YAML configuration using the `export` option, or by converting an XML configuration fragment using the [`frag2yaml`](/docs/apim_yamles/apim_yamles_cli/yamles_cli_convert/#convert-your-xml-configuration-fragment-to-a-yaml-configuration-fragment) option.

The fragment has the following characteristics:

* It is a valid configuration itself.
* It can be imported into another YAML configuration.
* It has the same structure as a complete configuration, with less types and less entities, meaning that some of the usual folders might be missing.
* You can run the `import` and `export` commands on complete YAML configurations or YAML configuration fragments.
* A number of `import` operations can be chained together to build up the required YAML configuration needed for a deployment.
* You can validate an exported YAML fragment by using the yamles [`validate`](/docs/apim_yamles/apim_yamles_cli/yamles_cli_validate/#validate-configuration-changes-in-the-yaml-configuration) command.

The following options in the YAML CLI are related to import and export of YAML configuration fragments:

* `export`: Exports part of a YAML configuration to create another YAML configuration.
* `import`: Imports a YAML configuration into another YAML configuration.

## Control data export with the _fragment.yaml file

The `_fragment.yaml` file is used to control what data is exported from a source configuration to create a YAML configuration fragment. It is also used to determine how to import a YAML configuration fragment into another configuration.

The following is an example of a `_fragment.yaml` file:

```yaml
---
flags:
- EXPORT_TYPES
- EXPORT_ENTITIES
- EXPORT_CLOSURE
- EXPORT_TRUNKS
addIfAbsent:
- /Policies/My Test Container/My Test Policy
- /Environment Configuration/Certificate Store/Samples Test CA
- /System/Policy Categories/miscellaneous
- /System/Filter Categories/miscellaneous
- /System/Filter Categories/attribute
addOrReplace:
- /External Connections/Database Connections/Default Database Connection
cutBranchIfPresent:
- /Policies/Policy Library/WS-Policy/Test Timestamp is Absent
```

### Use the _fragment.yaml file to export

The `export` option uses the `_fragment.yaml` file passed in the `--export-descriptor` parameter to determine what data to export from the source configuration.

The configuration fragment that `export` generates contains a `META-INF/_fragment.yaml` file with the same content as the one passed to it in the `--export-descriptor` parameter, with some minor modifications to enable required flags, if they are not already enabled.

The YamlPKs listed in `addIfAbsent` and `addOrReplace` are combined to provide the list of selected entities to export. If an entity is listed in either section, they are included in the exported YAML configuration. All child entities of these selected entities are also exported, even if they are not explicitly listed. All parent entities of the selected entities are also exported.

You can also export all entities, along with their related types, using a `_fragment.yaml` with flags set to EXPORT_ENTITIES and with the root PK `/` specified in `addIfAbsent`.

The flags have the following meaning at export time:

| Flag            | Description                                                        |
|-----------------|--------------------------------------------------------------------|
| EXPORT_TYPES    | Export entity types for selected entities. Always `ON`.              |
| EXPORT_ENTITIES | Export selected entities and their children. Normally `ON`.          |
| EXPORT_CLOSURE  | Export entities referred to by entities selected for export.       |
| EXPORT_TRUNKS   | Export parent entities of entities selected for export. Always `ON`. |

`EXPORT_TYPES` and `EXPORT_TRUNKS` are always enabled, meaning that they are set even if they are not provided in the file provided through the `--export-descriptor` parameter to the `export` command.

The `EXPORT_TYPES` flag ensures all the type information for the selected entities is exported. Type information for the parent entities and the child entities is also included. The type information is stored at `META-INF/types/`. If this flag is disabled, only entity types for the selected entities are exported.

You can also export all the type definitions only from a YAML configuration by supplying a `_fragment.yaml` with flags set to EXPORT_TYPES and no `addIfAbsent` or `addOrReplace` entries.

If `EXPORT_CLOSURE` is enabled, all entities referred to by the selected entities and their children are also exported. The type information for these entities will also be included. If this flag is disabled, the fragment might not contain all entities that are referred to from those included in the fragment. In this case, the YAML configuration can only be validated with the `--allow-invalid-ref` parameter. For more information, see [how to allow unresolved references](/docs/apim_yamles/apim_yamles_cli/yamles_cli_validate/#disable-entity-reference-check).

The `EXPORT_TRUNKS` flag ensures that all parent entities of the selected entities are exported.

The `cutBranchIfPresent` field is not used during export, but it can be set at export time to control what entities might get removed when the fragment is imported.

### Use the _fragment.yaml file to import

A source configuration gets imported into a target configuration. The `META-INF/_fragment.yaml` file in the source configuration is optional for import. If it does not exist, all entities in the source are added to the target configuration only if they do not already exist in the target. This is equivalent to having YamlPKs for all entities listed in the `addIfAbsent` field.

The flags in `META-INF/_fragment.yaml` of the source configuration are ignored at import time. They are retained for informational purposes.

The YamlPKs identify entities as follows:

* When listed in the `addIfAbsent` field, they identify the entities that will be added to the target configuration at import time only if they do not already exist in the target.
* When listed in the `addOrReplace` field, they identify the entities that will replace the target copies of the entities if they already exist, or that get added if they do not already exist.

If there are entities in the source configuration that are not listed in `addIfAbsent` or `addOrReplace`, they are imported as if they were listed in the `addIfAbsent` section.

The `cutBranchIfPresent` field is used at import time to identify entities in the target configuration that should be removed if they exist in the target.

The `import` operation involves a source and a target configuration. The target can have a `META-INF/_fragment.yaml` file also, but this is not used during import.

## Export from a YAML configuration

The `export` option creates a new YAML configuration fragment in the `--output-dir`, and it publishes a report indicating what entities and types were exported. The source configuration is never modified.

You can specify the source configuration using the `--source` parameter as a directory name, URL, or tar.gz file. The directory name and URL formats are the same. A directory name `/home/user/yaml` is equivalent to URL `yaml:file:/home/user/yaml`.

You can specify other parameters as follows:

* `--export-descriptor`: Specifies the `_fragment.yaml` file, which determines what gets exported.
* `--targz`: Generates a `.tar.gz` file of the newly created YAML configuration fragment.

The passphrase is not needed for the `export` command. If the source configuration is encrypted with a passphrase, the generated YAML configuration fragment will be encrypted with the same passphrase. It can be changed later using the `encrypt` or [`change-passphrase`](/docs/apim_yamles/apim_yamles_cli/yamles_cli_ecryption)  commands.

The following are examples of how you can use the `export` option in the `yamles` CLI to export configuration.

**Example 1**: Specify the directory of the source YAML configuration:

```
./yamles export --source /home/user/yaml --export-descriptor _fragment.yaml --output-dir ~/yaml-frag
```

**Example 2**: Specify the URL of the source YAML configuration:

```
./yamles export --source yaml:file:/home/user/yaml --export-descriptor /home/user/descriptors/_fragment.yaml --output-dir ~/yaml-frag
```

**Example 3**: Export from a source YAML configuration that is in a `.tar.gz` file:

```
./yamles export --source ~/archives/config.tar.gz --export-descriptor _fragment.yaml --output-dir ~/yaml-frag
```

**Example 4**: Generates a `.tar.gz` of the created configuration fragment:

```
./yamles export --source /home/user/yaml --export-descriptor _fragment.yaml --output-dir ~/yaml-frag --targz ~/archives/fragment.tar.gz
```

You can run the following help command for more details on each parameter:

```
yamles export --help
```

## Import a YAML configuration into another YAML configuration

You can use the `import` option to import a source YAML configuration into a target YAML configuration. The import merges the source into the target configuration, and it publishes a report indicating what entities were added, updated, or deleted as a result of an import. It will also indicate what new types, if any, were imported. The `import` option also checks that the version of each entity type in the source matches the version in the target if it exists in the target, otherwise the import will fail.

The source configuration is never modified.

You can specify the source and target configuration using the `--source` and `--target` parameters as directory names, URLs, or tar.gz files. The directory name and URL formats are the same. A directory name `/home/user/yaml` is equivalent to URL `yaml:file:/home/user/yaml`. If a `.tar.gz` is specified as the source or target configuration, the `.tar.gz` file must exist before the `import` option is run. Importing into a target `.tar.gz` file will edit the contents of the existing `.tar.gz` file.

You can use the `--targz` parameter to generate a `.tar.gz` file from the updated target configuration. If the target configuration is already a `.tar.gz` file, this parameter is ignored as the result is already packaged into a `.tar.gz` file.

If either the source or target configuration is encrypted with a non-default passphrase, the passphrases must be provided via the `--source-passphrase` or `--target-passphrase` parameters respectively. If source and target configurations have different passphrases, the source is re-encrypted in a temporary location before it is imported into the target configuration. Passphrase checking is performed on both the source and target configurations where possible.

To check the passphrase, an ESConfiguration entity must exist in the configuration. If a passphrase check cannot be performed, a warning is shown on stdout. If passphrase checks cannot be done and your configurations contains encrypted data, you must ensure the passphrases for the source and target configurations are correct so that the sensitive data can be decrypted successfully from the resulting target configuration. The target configuration passphrase is used for the updated target configuration that contains the merged configuration.

If a source configuration contains `{{file "..." }}` placeholders pointing to an absolute file, an error might occur at load-time if those files are not present in the system running the CLI. To import these types of configurations, use must the `--allow-unresolved-absolute-files` parameter instead.

When using [Team development in Policy Studio](/docs/apigtw_devops/team_dev_practices/#enable-team-development-in-policy-studio) or when your configuration is fragmented into smaller pieces, it is very likely that you import incomplete, invalid stores into a main store. To workaround these validation issues, you can set two flags with `yamles import`:

* Set `--allow-invalid-ref` if some references can only be resolved in the main store.
* Set `--allow-invalid-cardinality` if the import returns cardinality errors.

This is to mitigate some legacy small inconsistencies in some entity types. The YAML Entity Store is a little more strict than the Federated Store when an entity gets updated.

The following are examples of how you can use the `import` option in the `yamles` CLI to import a YAML configuration into another YAML configuration.

**Example 1**: Specify the YAML configuration directory names:

```
./yamles import --source /home/user/yaml-source --target /home/user/yaml-target
```

**Example 2**: Creates a `tar.gz` of the updated target YAML configuration where non-default passphrases have been used for both the source and the target:

```
./yamles import --source /home/user/yaml-source --target /home/user/yaml-target --source-passphrase pass1 --target-passphrase pass2 --targz /home/user/new-target.tar.gz
```

**Example 3**: Specify configuration URLs:

```
./yamles import --source yaml:file:/home/user/yaml-source --target yaml:file:/home/user/yaml-target
```

**Example 4**: Specify a new YAML configuration tar.gz as target:

```
./yamles import --source /home/user/yaml-source.tar.gz --target /home/user/yaml-target.tar.gz
```

**Example 5**: Mix the format of how you specify the source and target YAML configurations. For example import a `tar.gz` into a YAML configuration in a directory:

```
./yamles import --source /home/user/yaml-source.tar.gz --target /home/user/yaml-target
```

**Example 6**: Typical Team development import options to overcome intermediate validation issues:

```
./yamles import --source /home/user/app-part-1 --target /home/user/yaml-main-config --allow-invalid-ref --allow-invalid-cardinality
```

**Example 7**: Importing a source YAML configuration containing externalized files pointing to absolute files not present on the system:

```
./yamles import --source /home/user/yaml-source.tar.gz --target /home/user/yaml-target.tar.gz --allow-unresolved-absolute-files
```

You can run the following help command for more details on each parameter:

```
yamles import --help
```
