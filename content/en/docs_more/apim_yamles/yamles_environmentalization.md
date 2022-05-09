{
"title": "Environmentalize your YAML configuration",
"linkTitle": "Environmentalize your YAML configuration",
"weight":"50",
"date": "2020-09-24",
"description": "Learn how to environmentalize your YAML configuration."
}

The XML federated configuration provides different ways to environmentalize parts of the configuration. See [Environmentalize configuration](/docs/apigtw_devops/promotion_arch/#environmentalize-configuration) for details.

The YAML configuration format replaces environmentalization done via Policy Studio (using the *Earth* icon). The conversion process will look after the replacement of this type of environmentalization. Selectors such as `${environment.XYZ}` and `${env.XYZ}` will continue to work as before.

YAML environmentalization capabilities can be applied to:

* All entities.
* The value of a field. This is possible for all types of fields: numeric, string, references, passwords.
* Part of the value of a string.
* Some or all of the content in externalized files such as Scripts, Messages, and JSON Schemas. For more information, see **Environmentalization possible inside file content** column of the [Externalized files naming Scheme](/docs/apim_yamles/yamles_externalized_files/#externalized-files-default-naming-scheme) table.

In a nutshell, the YAML configuration is a template where you can environmentalize all types of values.

The following table describes all of the supported environmentalization options for the YAML format:

| Feature / File | Syntax | Description |
| -------------- | ------ | ----------- |
| YAML style           | `{{env "XYZ"}}` | This syntax is allowed everywhere, and it replaces the Policy Studio Environmentalization feature. Evaluated by API Gateway at load time using system environment variables, which means that you are warned about missing system environment variables sooner. |
| Environment selector | `${environment.XYZ}` | Only supported for fields that support selectors in runtime. Evaluated by API Gateway at runtime using system environment variables. This can be replaced by the `{{env "XYZ"}}` syntax. |
| envSettings.properties | `${env.xyz}` | Evaluated by API Gateway at load time, using `envSettings.props` content specific to an instance. These settings are instance-specific as opposed to environment-specific.|
| system.properties | `${system.xyz}` | Evaluated by API Gateway at load time, using `system.properties` file content. |
| KPS | `${kps.TableXYZ[some value].field}` | Evaluated at runtime, using the configured Key Properties Store. |

## Enable environmentalization

To enable environmentalization, create a file named `values.yaml` in the root directory where your YAML entity store is located.

The following is an example of environmentalization:

```yaml
# Environmentalized entity in the YAML configuration
---
type: "DbConnection"
fields:
   name: MySQL
   url: "jdbc:mysql://{{db.host}}:3306/DefaultDb"
   password: '{{db.password}}'
   username: "{{db.username}}"
```

```yaml
# Content of the value.yaml
db:
  host: localhost
  username: scott
  password: HDidnjekj=

```

```yaml
# Resolved YAML Entity (in memory at runtime)
---
type: "DbConnection"
fields:
   name: MySQL
   url: "jdbc:mysql://localhost:3306/DefaultDb"
   password: 'HDidnjekj='
   username: "scott"
```

`{{...}}` placeholders are replaced when the configuration is loaded by the API Gateway or in ES Explorer.

When you environmentalize references, the certificates also get environmentalized. For example:

```yaml
# Environmentalized FilterCircuit entity in the YAML configuration
---
type: FilterCircuit
fields:
  name: cert
  start: ./XML Signature Generation
  description: ""
children:
- type: GenerateSignatureFilter
  fields:
    name: XML Signature Generation
    signingCert: '{{certs.sign}}'
# ...
```

```yaml
# Content of the value.yaml
---
certs:
  sign: /Environment Configuration/Certificate Store/Samples Test CA
```

```yaml
# Resolved YAML Entity (in memory at runtime)
---
type: FilterCircuit
fields:
  name: cert
  start: ./XML Signature Generation
  description: ""
children:
- type: GenerateSignatureFilter
  fields:
    name: XML Signature Generation
    signingCert: '/Environment Configuration/Certificate Store/Samples Test CA'
# ...
```

The `values.yaml` above could also point to a system environment variable `CERT_DNAME` that would determine the certificate to be used at runtime.

```yaml
# Content of the value.yaml
certs:
  sign: /Environment Configuration/Certificate Store/{{env "CERT_DNAME"}}
```

Your CI/CD pipeline must ensure that any certificates and keys are packed up correctly for each environment in the `.tar.gz` that gets deployed. If you attempt to validate a YAML configuration that uses a system environment variable on a system that does not have a value set for that environment variable, you will need to use the `--allow-invalid-ref` option. For more information, see [Disabling entity reference check](/docs/apim_yamles/yamles_cli/#disabling-entity-reference-check).

## Environmentalization syntax

The following are elements of the environmentalization syntax.

### Variables

**Scope:**

* YAML Entity files.
* Compatible externalized files.

#### Global syntax

```
{{expression}}
```

Expression can contain:

* Simple properties: `{{foo}}`
* Nested properties: `{{foo.bar}}`
* Indexed properties: `{{foo.bar.[0]}}`, or the equivalent `{{foo.bar.0}}`. This syntax is useful for multiple values of the field.

{{< alert title="Note">}}`{{ expression }}`, with spaces, works but is not recommended as the parsing logic will change these expressions to `{{expression}}` at load time. Thus, when the YAML configuration is exported, it would create unexpected changes causing unwanted `diffs` in case the configuration is managed within Git.
{{< /alert >}}

#### Usage in YAML files

All YAML files *must be parsable*.

As `{` is interpreted, the first character of a value must be quoted. Files are parsed and they might be written again (in case the entity store is modified, for example, change of passphrase), so it is wise to choose single quotation.

```yaml
---
type: "DbConnection"
fields:
  name: MySQL

  # OK as it does not start with {
  url: jdbc:mysql://{{db.host}}:3306/DefaultDb
  
  # OK
  password: '{{db.password}}'

  # OK but will change to '{{db.username}}' in case of export
  username: "{{db.username}}"

  # Invalid YAML. Parsing will fail, missing ''
  maxIdle:  {{db.maxIdle}}

 ```

### Environment and JVM variables

**Scope**:

* YAML entity files.
* `values.yaml` files.

You can environmentalize any field or value by using system environment variables or JVM properties, `-Dxyz`, when starting a java process, such as the API Gateway.

* Environmentalize with no default value: `{{env "XYZ"}}` or `{{env 'XYZ'}}` (Not recommended, see [Usage in YAML files](#usage-in-yaml-files)).
* Environmentalize with a default value: `{{env "XYZ" "The end of the alphabet"}}` or `{{env "XYZ" default="The end of the alphabet"}}`.

The value of `XYZ` is evaluated by the first true condition from this list:

* An environment variable with name `XYZ` exists.
* An environment variable with name `xyz` exists (lowercase).
* The JVM was started with `-DXYZ`.
* The JVM was started with `-Dxyz` (lowercase).
* A default value is set.

If none of the above conditions are true, an error is raised in the logs when the API Gateway loads the YAML configuration.

### Base64 encoding

**Scope**:

* `values.yaml` files.
* YAML Entity files (not recommended).

You can encode a string value in base64 using ```{{base64 "changeme"}}```.

This is very useful for development or local environment, when your YAML configuration does not required to be encrypted. This will not work when your YAML configuration is encrypted, as recommended for production environments.

The placeholder will be replaced by an encrypted value in case a non-empty passphrase is set.

### Reserved words

The following words are not allowed at the beginning of a `{{...}}` expression:

* `null`
* `true` and `false`
* `undefined`
* Digits, as first characters. For example, `{{42_foo.bar}}`.

Examples:

```yaml
# Invalid because of reserved word
null_user:
  name: ""
  
false_expressions:
  - F
  - N
  - 0

# Invalid for 'true' or 'undefined' reserved words

# Invalid because digit at starting position
1_Services:
   registration: Registration Service

# Valid examples
user_is_null_if:
  name: ""
  
falsy_expressions:
  - F
  - N
  - 0

_1_Services:
   registration: Registration Service

# Valid because the resolved expression {{validation_schemas.1_user}} does not start with a digit
validation_schemas:
   1_user: http://acme.com/schemas/users
   2_group: http://acme.com/schemas/group
```

### Escape quotes in values.yaml

Follow these guidelines to avoid issues when using quotation marks in strings:

* Use two single quotation marks for double quotation marks enclosed string (`" "`).
* Use four single quotation marks for single quotation marks enclosed string (`' '`).

Example of an environmentalization placeholder surrounded by single quotation marks:

```yaml
fields:
  url: '{{foo.bar.url.value}}'
```

Example to set `value` with `a message with 'single quotation marks'`:

```yaml
value: "a message with ''single quotation marks'' "
```

or

```yaml
value: 'a message with ''''single quotation marks'''' '
```

## Environmentalization in values.yaml

### Syntax

As `{` is interpreted, the first character of a value must be quoted. Files are parsed and they might be written again (in case the entity store is modified, for example, change of passphrase), so it is wise to choose single quotation.

```yaml
---
some_global_value: 42
db:
  # OK
  initialSize: '{{env "DB_POOL_SIZE"}}'

  # OK as it does not starts with a {
  url: jdbc:mysql://{{env "DB_HOST"}}/{{env "DB_SCHEMA"}}
  
  # OK but will be written '{{env ''DB_USER''}}' on export
  username: "{{env 'DB_USER'}}"

  # Invalid. Self reference resolving is not supported
  minIdle: '{{db.initialSize}}'
  
  # Invalid YAML. Parsing will fail, missing ''
  password: {{base64 "changeme"}}

 ```

In `values.yaml`, you can only use letters (including `_`) and digits (not at starting position).

### Usage

To decouple where the value of an environment variable is coming from and where it is used, use the `{{env "XYZ"}}` syntax in the `values.yaml` file:

```yaml
---
db:
  host: '{{env "DB_HOST"}}' # set a value in an environment variable per environment
```

If you want to set a fixed value that does not change between all environments, change the `values.yaml` to:

```yaml
---
db:
  host: db.com
```

Alternatively, to use a different `values.yaml` for each environment, set it as follows:

```yaml
# values.yaml for staging
---
db:
  host: staging.db.acme.com
```

```yaml
# values.yaml for prod
---
db:
  host: prod.db.acme.com
```

This can all be done without changing the YAML file for the entity that always point to `db.host` environmentalized property:

```yaml
#Some Entity.yaml
---
type: DBConnection
fields:
    # this will will change depending on witch values.yaml is packaged
    host: '{{db.host}}'
```

Refer to [Environmentalization example](/docs/apim_yamles/apim_yamles_references/yamles_environmentalization_example) for a complete example of the different capabilities offered by the YAML configuration environmentalization.

## Convert Policy Studio environmentalization for XML federated configurations to YAML syntax

You can environmentalize XML federated configurations in Policy Studio by clicking **Preferences > Environmentalization > Allow environmentalization**, and using the `{{env "XYZ"}}` syntax.

When XML federated configurations that contain fields environmentalized via Policy Studio are converted to YAML format:

* A `values.yaml` is created.
* Each environmentalized field with a value yields an entry:
    * In `values.yaml`, with the environmentalized values.
    * In `values-original.yaml`, with the value of the field before being environmentalized in Policy Studio.
* A placeholder replaces the value in entity YAML file. The naming follows the YamlPK logic with sanitization that is compliant with the environmentalization syntax.

The following is an example for entity with an environmentalized URL, username, and password:

**Entity in XML federated configuration**:

* Type: `LdapDirectory` (LDAP Connection)
* Name: `api-env LDAP`
* Environmentalized fields: `url`, `userName`, `password`
* Contained in `LDAP Connections`

**YAML configuration after conversion**:

```yaml
---
type: LdapDirectory
fields:
  name: api-env LDAP
  url: '{{LDAP_Connection.api_env_LDAP.url}}'
  userName: '{{LDAP_Connection.api_env_LDAP.userName}}'
  password: '{{LDAP_Connection.api_env_LDAP.password}}'
```

**Content of values.yaml**:

```yaml
LDAP_Connection:
  api_env_LDAP:
    url: ldap://api-env:389
    userName: cn=Administrator,dc=demo.axway,dc=com
    password: KLJH95dHS7djshkjas54sa45s4d==
```

The following is an example for an entity with an environmentalized certificate:

**Entity in XML federated configuration**:

* Type: `ConnectToURLFilter`
* Name: `name: Connect to URL`
* Environmentalized Fields: `sslUsers`
* Contained in `Policies/YAML Demo/Connect with SSL`

**YAML configuration after conversion**:

```yaml
---
type: FilterCircuit
fields:
  name: Connect with SSL
  start: ./Connect to URL
  description: ""
children:
- type: ConnectToURLFilter
  fields:
    name: Connect to URL
    sslUsers: '{{YAML_Demo.Connect_with_SSL.Connect_to_URL.sslUsers}}'
    url: https://localhost:5555/cert-verifier
    name: Connect to URL
# ...
```

**Content of values.yaml**:

```yaml
---
YAML_Demo:
  Connect_with_SSL:
    Connect_to_URL:
      sslUsers: /Environment Configuration/Certificate Store/O=DEV,CN=localhost
```
