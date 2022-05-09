{
"title": "YAML CLI overview",
"linkTitle": "YAML CLI",
"weight":"30",
"date": "2020-11-11",
"description": "YAML CLI tool overview"
}

The `yamles` CLI tool is available in the `apigateway/posix/bin` directory. It is installed with a standard server-side install or with client tooling, when the **Package & Deploy Tools only** option is selected during the installation.

You can run the `yamles` script with `--help` to see all the available options:

```
./yamles --help
```

The following lists the CLI options and their descriptions:

* `fed2yaml`: Convert an XML [federated configuration](/docs/apim_yamles/apim_yamles_cli/yamles_cli_convert/#convert-your-xml-configuration-to-a-yaml-configuration) to a YAML configuration.
* `frag2yaml`: Convert an XML [configuration fragment](/docs/apim_yamles/apim_yamles_cli/yamles_cli_convert/#convert-your-xml-configuration-fragment-to-a-yaml-configuration-fragment) to a YAML configuration fragment.
* `validate`: [Validate](/docs/apim_yamles/apim_yamles_cli/yamles_cli_validate/#validate-configuration-changes-in-the-yaml-configuration) a YAML configuration.
* `export`: [Export](/docs/apim_yamles/apim_yamles_cli/yamles_cli_importexport/#export-from-a-yaml-configuration) part of a YAML configuration to create another YAML configuration.
* `import`: [Import](/docs/apim_yamles/apim_yamles_cli/yamles_cli_importexport/#import-a-yaml-configuration-into-another-yaml-configuration)  a YAML configuration or a YAML configuration fragment into another YAML configuration.
* `encrypt`: [Encrypt](/docs/apim_yamles/apim_yamles_cli/yamles_cli_encryption/#encrypt-an-unencrypted-yaml-configuration)  an unencrypted YAML configuration, a string, or a file. For more information.
* `change-passphrase`: [Change the passphrase](/docs/apim_yamles/apim_yamles_cli/yamles_cli_encryption/#change-the-encryption-passphrase-of-a-yaml-configuration) of a YAML configuration.
* `upgrade`: [Upgrade](/docs/apim_yamles/apim_yamles_cli/yamles_cli_upgrade) a YAML configuration.

To get help with a specific option, run:

```
./yamles <option> --help
```

All script parameters have shorthand parameter names. Check the relevant help for more information.

All script options generate a trace file in the current directory by default. You can use `--tracedir` to write trace to another directory, or `--tracelevel` to change the level of tracing.

The `yamles` script returns the following exit codes:

* `0` for success
* `2` for invalid parameters
* `1` for all other failures

{{< alert title="Note">}}
For Windows systems, the CLI tool is located in the `apigateway\Win32\bin` directory. You must replace all `./yamles ...` with `yamles.bat ...`.

Also, all options with URLs must be set as follows:

* FED: `--source federated:file:/C:\Users\...\configs.xml` or `--source federated:file:/C:/Users/.../configs.xml`
* YAML: `--source yaml:file:/C:\Users\...` or `--source yaml:file:/C:/Users/...`
{{< /alert >}}
