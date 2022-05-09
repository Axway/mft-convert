{
"title": "Encrypt YAML configuration using CLI",
"linkTitle": "Encrypt YAML configuration using CLI",
"weight":"70",
"date": "2020-11-11",
"description": "Learn how to use the YAML configuration CLI to encrypt, decrypt, and re-encrypt a YAML configuration."
}

The YAML CLI provides two encryption options:

* `encrypt`: Encrypt an unencrypted YAML configuration, a string, or a file.
* `change-passphrase`: Change the passphrase of a YAML entity store.

## Encrypt an unencrypted YAML configuration

Encryption of XML or YAML configuration means that sensitive fields of type `encrypted` in the Entity Store model are encrypted. All other data remains in the clear. If the XML federated configuration you converted to YAML was not encrypted, it remains unencrypted after conversion. If you wish to deploy the configuration to an API Gateway group that has a group passphrase set, you must encrypt the YAML configuration before deployment.

If you get an error when attempting to encrypt the YAML configuration, this means that the passphrase for the configuration is incorrect or that the configuration is already encrypted. Use the `change-passphrase` option to change the passphrase.

You can also use [projdeploy](/docs/apim_reference/devopstools_ref/#projdeploy-command-options) to encrypt a YAML configuration at deployment time.

The following are examples of how you can use the `encrypt` option in the `yamles` CLI to encrypt an unencrypted YAML configuration.

**Example**: Specify a directory with passphrase `changeme`:

```
./yamles encrypt --source /home/user/yaml --passphrase changeme
```

or alternatively

```
./yamles change-passphrase --source /home/user/yaml --old-passphrase "" --new-passphrase newpassphrase
```

**Example**: Specify a URL with passphrase `changeme`:

```
./yamles encrypt --source yaml:file:/home/user/yaml --passphrase changeme
```

**Example**: Specify a `.tar.gz` file with passphrase `changeme`:

```
./yamles encrypt --source /home/user/yaml.tar.gz --passphrase changeme
```

You can run the following help command for more details on each parameter:

```
yamles encrypt --help
```

## Encrypt strings and files to add to an encrypted YAML configuration

If the XML federated configuration you converted to YAML was encrypted with a passphrase, it remains encrypted with the same passphrase after conversion to YAML format. If you wish to add new configuration that includes fields of type `encrypted`, you need to encrypt that data with the same entity store passphrase so that it can be read by the API Gateway when deployed. For example, you wish to add an entity of type `DbConnection`, which has a `password` field, of `encrypted` type. You must put the encrypted value of the password string into the YAML file for the `DbConnection`.

If your database password is `dbpassword` and your passphrase for the YAML configuration is `changeme`, use the following command to encrypt the database password string value:

```
./yamles encrypt --text "dbpassword" --source /home/user/yaml --passphrase "changeme"
```

The output will be as follows:

```
Your encrypted base64 encoded string content is:-
wWVn7dS/ycwDg7Miqd1TU0YKTiOY//5i
```

Copy and paste the string `wWVn7dS/ycwDg7Miqd1TU0YKTiOY//5i` into your yaml file for your `DbConnection`:

```yaml
---
type: DbConnection
fields:
  url: jdbc:mysql://127.0.0.1:3306/DefaultDb
  name: MySQL/local
  username: root
  password: wWVn7dS/ycwDg7Miqd1TU0YKTiOY//5i
```

A private key is sensitive data that is externalized into a separate file in the YAML configuration. Its data is also of type `encrypted`, and the file content must be encrypted if the YAML configuration is encrypted. A private key file can be encrypted as follows:

```
./yamles encrypt --file /home/user/private-key.pem --source /home/user/yaml --passphrase "changeme"
```

{{< alert title="Note">}}`yamles encrypt` with `--file` option only supports encryption of `PEM` file containing header (starting line `-----BEGIN...`) and footer (ending line `-----END...`){{< /alert >}}

Both the `--text` and `--file` options require a YAML configuration to be specified in the `--source` parameter. The YAML configuration is not updated by the command. You must use the same YAML configuration that you will be adding the encrypted text or file into so that the data is encrypted correctly for the destination YAML configuration.

See section [Add a new certificate and private key to a YAML configuration](/docs/apim_yamles/yamles_edit/#add-a-new-certificate-and-private-key-to-a-yaml-configuration) for more information regarding adding a certificate and private key to a YAML configuration. Note that `encrypt --file` option may be used to encrypt the content of any type of file. The operation will update the file which will always be base64 encoded after encryption, regardless of the format of the original unencrypted file. Note that encrypting a private key via `openssl` and adding it to the YAML configuration is not supported.

## Change YAML configuration passphrase

To change the passphrase of an encrypted YAML configuration you must know your current passphrase.

The following are examples of how you can use the `change-passphrase` option in the `yamles` CLI to change the passphrase of a YAML configuration:

**Example**: Specify a directory to change from `changeme` to `newpassphrase`:

```
./yamles change-passphrase --source /home/user/yaml --old-passphrase changeme --new-passphrase newpassphrase
```

**Example**: Specify a URL to change from `changeme` to `newpassphrase`:

```
./yamles change-passphrase --source yaml:file:/home/user/yaml --old-passphrase changeme --new-passphrase newpassphrase
```

**Example**: Specify `.tar.gz` file to change from `changeme` to `newpassphrase`:

```
./yamles change-passphrase --source /home/user/yaml.tar.gz --old-passphrase changeme --new-passphrase newpassphrase
```

You can run the following help command for more details on each parameter:

```
yamles change-passphrase --help
```

You can also use [projdeploy](/docs/apim_reference/devopstools_ref/#projdeploy-command-options) to change the passphrase of a YAML configuration at deployment time.

The following snippet shows how the `change-passphrase` option affects the content of environmentalized YAML files. Encrypted data located in `values.yaml` is also changed unless it is an environment variable.

```yaml
# Entity file
...
fields:
  # remains unchanged
  password: '{{conf.password}}'  
  alternatePassword: '{{env "ALT_PASSWORD"}}'
  # will be changed
  someCipheredKey: eW91c2hhbGxub3RwYXNzeW91Zm9vbGlzaHBpcmF0ZQ==
...

# values.yaml
---
# both will be changed
conf:  
  password: wWVn7dS/ycwDg7Miqd1TU0YKTiOY//5i
cassandra:
  password: '{{base64 "s3cr3t"}}'

# remains unchanged
ldap:  
  password: '{{env "LDAP_PASSWORD"}}'

```

## Decrypt a YAML configuration

To decrypt YAML configuration you must know your current passphrase.

The following are examples of how you can use the `change-passphrase` option in the `yamles` CLI to decrypt a YAML configuration:

**Example**: Specify a directory to decrypt a configuration having passphrase `changeme`.

```
./yamles change-passphrase --source /home/user/yaml --old-passphrase changeme --new-passphrase ""
```

**Example**: Specify a URL to decrypt a configuration having passphrase `changeme`.

```
./yamles change-passphrase --source yaml:file:/home/user/yaml --old-passphrase changeme --new-passphrase ""
```

**Example**: Specify a `.tar.gz` file to decrypt a configuration having passphrase `changeme`.

```
./yamles change-passphrase --source /home/user/yaml.tar.gz --old-passphrase changeme --new-passphrase ""
```
