{
"title": "Upgrade YAML configuration using CLI",
"linkTitle": "Upgrade YAML configuration using CLI",
"weight":"80",
"date": "2021-04-15",
"description": "Learn how to use the YAML configuration CLI to upgrade a YAML configuration."
}

{{< alert title="Caution" color="warning" >}}
If you are using YAML configurations made with a [technical preview release](/docs/apim_relnotes/20210330_apimgr_relnotes/#yaml-configuration-store-technical-preview-capability), you must refer to the [Known limitations](/docs/apim_yamles/apim_yamles_references/yamles_known_limitations) section before using this command. {{< /alert >}}

The `upgrade` option is the centralized entry point to upgrade any kind of YAML configuration. It enables you to upgrade YAML configurations or YAML configuration fragments to their latest version.

The following are examples of different ways that you can use the `upgrade` option to update your YAML configuration.

**Example 1:**

* Specify the directory for the YAML configuration to upgrade.
* Add the upgraded YAML configuration to the `/home/user/yaml-upgraded` directory.
* Create a `.tar.gz` file of the content of `/home/user/yaml-upgraded`.

```
./yamles upgrade --source /home/user/yaml --output-dir /home/user/yaml-upgraded --targz /home/user/archives/yaml-upgraded.tar.gz
```

**Example 2:**

* Specify the URL for the YAML configuration to upgrade.
* Add the upgraded YAML configuration to the `/home/user/yaml-upgraded` directory.

```
./yamles upgrade --source yaml:file:///home/user/yaml --output-dir /home/user/yaml-upgraded
```

**Example 3:**

* Specify the `.tar.gz` file for the YAML configuration to upgrade.
* Add the upgraded YAML configuration to the `/home/user/yaml-upgraded` directory.
* Create a `.tar.gz` file of the content of `/home/user/yaml-upgraded`.

```
./yamles upgrade --source yaml-to-upgrade.tar.gz --output-dir /home/user/yaml-upgraded --targz /home/user/archives/yaml-upgraded.tar.gz
```

**Example 4:**

* Specify the `.tar.gz` file for the YAML configuration to upgrade.
* Upgrade the YAML configuration with a non-default passphrase.
* Add the upgraded YAML configuration to the `/home/user/yaml-upgraded` directory.
* Create a `.tar.gz` file of the content of `/home/user/yaml-upgraded`.

```
./yamles upgrade --source yaml-to-upgrade.tar.gz --passphrase secret --output-dir /home/user/yaml-upgraded --targz /home/user/archives/yaml-upgraded.tar.gz
```

{{< alert title="Note">}}
The YAML configuration upgraded is encrypted with the same passphrase as the source YAML configuration.
{{< /alert >}}

You can run the following help command for more details on each parameter of the `upgrade` option:

```
yamles upgrade --help
```
