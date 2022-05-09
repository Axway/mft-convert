{
"title": "YAML Schema",
"linkTitle": "YAML Schema",
"weight":"80",
"date": "2020-09-25",
"description": "List of all valid syntax and data structures for YAML configuration files." 
}

{{% alert title="Warning" color="warning" %}}YAML entity store files do not support YAML anchors.{{% /alert %}}

## Versioning

The schema version of YAML Entity Store is the major version (M) of the jar file, located in `%VINST%/apigateway/system/lib/plugins/yaml-entity-store-M.m.p.jar`. You can also find the version in your YAML configuration files in `META-INF/_version.yaml`.

The major version is defined following [Semantic Versioning 2.0.0](https://semver.org/) specification.

## Conventions

All schemas use the following conventions:

| Token  | Meaning                 |
|------- |-------------------------|
| `|`    | or                      |
| `{a|b}`| possible choices        |
| `{x-y}`| x to y                  |
| `[ ]`  | optional                |
| `*`    | zero or many elements   |
| `+`    | one or many elements    |
| `...`  | repetition              |

## General entity files

This type of file contains configuration (Entity) values.

### Entity schema

The following is an example template of an entity scheme file:

```yaml
---
type: string       # Type name, should exist in META-INF/types
fields:            # Name/value pair(s). Field names exist in the entity type
   fieldName1: {integer | long | boolean | 'placeholder' | string {text|base64|reference} | null | array (of any type)}
   fieldName2: idem
   fieldNameArray:
   - idem 
   - idem
[children]:        # a list of child entities as described above (order do not matter)
- type: string     # should be one of the type listed in "components" see entity type schema
  ...
```

The `string` value can be any UTF-8 character, except two consecutive `{` that can be replaced by `{{dcb}}`. For more information, see [Placeholders syntax](#placeholders-syntax) section.

The following are examples of entity scheme files:

```yaml
---
type: DbConnection
fields:
  name: oracle                               # unquoted string
  minIdle: 1                                 # integer
  username: "scott"                          # double quoted string
  password: dGlnZXI                          # base64 unquoted string
  timeBetweenEvictionRunsMillis: 100000      # long
  url: '{{oracle.url}}'                      # placeholder
  wildcardPassword: ""                       # empty string
  timezoneAware: true                        # boolean
```

```yaml
---
type: AuthnRepositoryDbGroup
fields:
  name: Database Repositories
  allowedFilter:              # array of string
  - HttpBasicFilter
  - HttpDigestFilter
  - WsBasicFilter
children:                     # child entity
- type: AuthnRepositoryDb
  fields:
    name: Oracle Repo
    authZColmn: null          # null value
    dbConn: /External Connections/Database Connections/oracle  # reference to another entity
  children:                   # grand child entity
  - type: Query
    fields:
      name: call P_CHECK_PASSWORD
      statementType: 2
      sqlStatement: '{{file "Database Repositories/Oracle Repo - call P_CHECK_PASSWORD.sql"}}'  
                              # file placeholder ^
```

For more information on how to format YAML, see [YAML Syntax considerations](/docs/apim_yamles/yamles_edit/#yaml-syntax-considerations).

### Reference syntax

A reference is the identifier of another entity, the `YamlPK` key. An entity identifier is computed from:

* The position of the entity in the hierarchy.
* The value of the key fields.
* What top level directory it is located in.

The reference entity takes 2 attributes:

* **keyFields**: `keyFieldValue[,nextKeyFieldValue]*`
* **Type**: entity type name

The reference entity is formed as follows:

```
/Top-level-Folder/[(Type)]keyFields[/[(Type)]keyFields]*
```

* `keyFields` can contains any kind of characters including `/` or `,` this means that a reference is not parsable and it must be used as a whole.
* `Type` is mandatory when two entities of different types have identical `keyFields` at a given level of hierarchy.

For more information about the `Top level folder` see, [YAML Entity Store Directory Mapping](/docs/apim_yamles/apim_yamles_references/yamles_top_directories). You can also get some examples of references in [YAML entity store structure](/docs/apim_yamles/yamles_structure/).

#### Absolute reference

An absolute reference starts at the root entity and go through all nodes of the hierarchy.

Examples:

```
/System/Filter Categories/miscellaneous
```

```
/System/Namespace Configuration/http://schemas.xmlsoap.org/ws/2002/12/secext
```

```
/Policies/(CircuitContainer)Main App
```

This example implies that there is another entity with the same name (Main App) at the same hierarchal level with a different type. In this example the other entity would be: `/Policies/(FilterCircuit)Main App`.

#### Relative reference

A reference that points to an entity in the same file can be written as a relative reference:

Child reference:

```
./[(Type)]keyFields
```

Sibling reference:

```
../[(Type)]keyFields
```

Cousin reference:

```
../../[(Type)]keyFields/[(Type)]keyFields
```

#### Selector reference

A reference can also be a selector. For more information, see [Selector syntax](/docs/apim_policydev/apigw_poldev/general_selector/#selector-syntax).

#### Null reference

`/null` is a reference that points to *no entity*, but remains a valid reference. `/null` is different from `null`, which represents *no value*.

### Placeholders syntax

A placeholder starts with `{{` and ends with `}}`. Leading and trailing spaces are allowed but they are removed during the export task. Placeholders must be surrounded by single quotation marks only if the value of the field `value` starts with a placeholder.

For more information, see an example of how to [apply placeholders in YAML files](/docs/apim_yamles/yamles_environmentalization/#usage-in-yaml-files).

The following values are not allowed at the *beginning* of a placeholder:

* `null`
* `true`
* `false`
* `undefined`
* All digits

#### File

The `file` placeholder reads a file to populate a field.

Syntax:

```
{{file "location" ["pem"|"binary"]}}
```

* `location`: The locations are relative to the YAML entity file they are declared in. In case the value of the location is relative, the file must exist, otherwise an error is shown. When value of the location is absolute, the `import` and `upgrade` CLI options can support missing absolutes files as their location might only exist at runtime. But, an error is shown if a file does not exist at deployment time in API Gateway.
* `pem`: For base64 encoded files containing certificate (or key) PEM headers and footers (`----- BEGIN CERTIFICATE -----`, `----- END PRIVATE KEY -----`...)
* `binary`: If the file is a binary file that ought to be encoded in base64, it corresponds to `binary` type fields.

#### base64

The `base64` placeholder encodes a token in base64.

Syntax:

```
{{base64 "token to encode"}}
```

#### env

The `env` placeholder reads an environment variable, and a JVM property, if the environment variable does not exist.

If the environment variable is not found, `env` tries to read the it again, this time, in lowercase.

If it does not find a value for the environment variable or the JVM property, an `invalid field` string is returned, unless a default value is provided.

```
{{env "variable_name" [[default=]"some default value"]}}
```

Examples:

* `{{env "HOME"}}` - Returns the home directory.
* `{{env "OS.NAME"}}` - Returns the JVM property `os.name`.
* `{{env "APP_DB_HOST" default="localhost"}}` - Return `localhost` if, and only if, `APP_DB_HOST` or `app_db_host` are not environment variables, or `-DAPP_DB_HOST` or `-Dapp_db_host` are not set when starting the JVM.

#### dcb

The `dcb` placeholder allows to escape `{{`. "dbc" stands for **d**ouble **c**urly **b**races.

* `{{dbc}}` is rendered as `{{`
* `{{dbc "close"}}` is rendered as `}}`, although `}}` generally do not need to be escaped.

#### values

The `values` placeholders allow you to read properties out of `values.yaml` file.

Allowed characters for a property from the `values.yaml` file are:

* letters (case insensitive)
* digits (except in first position)
* `_`

**Special characters**:

* `.` is the property separator
* `.[x]` or `.x`, where `x` is a integer, is the indexed property accessor

**Expressions**:

* Property access: `{{property_name}}`
* Nested property access: `{{property_name.nested_property_name}}` no limits in number of nested property.
* Indexed property access: `{{array_property_name.[0]}}` or `{{array_property_name.0}}`.

**Forbidden property names**:

* `null`
* `true`
* `false`
* `undefined`

#### Sub expression

The `Sub expression` placeholder can be combined with other expressions. One placeholder can contain a "call" to another placeholder.

`{{placeholder1 (placeholder2a)[ (placeholder2b)]}}`

Examples:

```
{{base64 (env "DB_PASSSWORD" "s3cr3t")}}
```

```
{{file (env "CERT_FILE") "pem"}}
```

## Specialized entity files

Entity files, used in a policy, contain two extra properties to group routing and logging related fields that are present for all sub-entity types of `Filter`.

```yaml
---
type: string
fields:
  field1: ...
[routing:]
  [success]: reference      # maps to Filter 'successNode' field
  [failure]: reference      # maps to Filter 'failureNode' field
[logging:]
  [success]: string         # maps to Filter 'logSuccess' field
  [failure]: string         # maps to Filter 'logFailure' field
  [fatal]: string           # maps to Filter 'logFatal' field
  [mask]: string            # maps to Filter 'logMask' field
  [maskType]: string        # maps to Filter 'logMaskType' field
```

## values.yaml file

Allow you to set free formed hierarchal properties.

Allowed characters for a property are:

* letters (all cases)
* digits (except in first position)
* `_`

Property names cannot start with:

* `null`
* `true`
* `false`
* `undefined`

Because the `values.yaml` file follows a free format, a proper schema definition does not apply as long as the file is YAML compliant.

```yaml
---
property: { integer | long | boolean | 'placeholder' | string {text|base64|reference} | array 
  sub_property: idem
  sub_array_property:
  - idem
  - idem
```

Example:

```yaml
---
db:
  user: scott
  password: '{{base64 "tiger"}}'
  host: localhost
  maxIdle: 1
user:
  policy:
    sslUsers: /Environment Configuration/Certificate Store/O=DEV,CN=localhost
Test:
  Set_Message:
    body: '{{file "E18N-FILES/Policies/Test/Set Message/body.json"}}'
```

## Fragment descriptor

The fragment descriptor must be located in the `META-INF/_fragment.yaml` folder. However, when importing a fragment, the file is not mandatory.

When exporting a fragment, the file can have any name and can be placed anywhere. But, providing a file is mandatory.

For more information, see [Import and export YAML configuration using CLI](/docs/apim_yamles/apim_yamles_cli/yamles_cli_importexport).

```yaml
---
[flags: ] # useless for import
- {EXPORT_ENTITIES | EXPORT_CLOSURE [EXPORT_TYPES | EXPORT_TRUNKS]}
                                   # the last two ^ are set by default
- ...

[addIfAbsent]: 
- absolute reference
- ...

[addOrReplace]:
- absolute reference
- ...

[cutBranchIfPresent]:
- absolute reference
- ...
```

Example:

```yaml
---
flags:
- EXPORT_TYPES # set but useless
- EXPORT_ENTITIES
- EXPORT_CLOSURE
addIfAbsent:
- /Policies/My Test Container
addOrReplace:
- /External Connections/Database Connections/Default Database Connection
cutBranchIfPresent:
- /Policies/Policy Library/WS-Policy/Test Timestamp is Absent

 ```

Missing `META-INF/_fragment.yaml` during import is equivalent to the following descriptor, which imports all entities without override:

```yaml
---
flags:
- EXPORT_ENTITIES
- EXPORT_TYPES

addIfAbsent: 
- /
```

## Version file

This file is mandatory in YAML configurations and must be located in `META-INF/_version.yaml`.

```yaml
---
version: string # format is: {0-9}+.{0-9}+.{0-9}+
```

This file generated automatically and matches the implementation version.

## Entity Type files

All entity types are located in the `META-INF/types` folder, in there own file. For more information and examples see [Entity types in YAML configuration](/docs/apim_yamles/apim_yamles_references/yamles_types).

In the following schema, `AnyTypeName` represents any other type amongst existing types in the configuration.

```yaml
name: string                 # by convention, name is CamelCase (with leading capital) and contains only letters
version: integer (non negative)
[class]: string
[constants]:
  constantField1:            # name is camelCase and is of {a-z}{A-Z}{0-9}+ '_' is discourage despite being legal 
    type: {integer | long | boolean | string | base64 | utctime}
    value: {integer | long | boolean | string {text|base64} | array} 
fields:                      # The name of the field isn't mandatory but is restricted to some internal types
  field1:                    # same rule as the name of the fields
    type: {integer | long | boolean | string | binary | encrypted | utctime | @AnyTypeName}
    defaultValues:
    - {data|ref}: {integer | long | boolean | string {text|base64} | pk (iff ref) | array (of any type)}
    - {}                     # set this -> {} when no default values is set and cardinality is {'?'|'*'}
    cardinality: {'?'|1|'*'|'+'} # defaults to 1
  field2:
    ...
[components]:
  AnyTypeName: {'?'|1|'*'|'+'} 
[keyFields:]                 # not mandatory when abstract: true
- keyFieldName
- ...
[loadorder: long]            # for internal usage only
[abstract: true]             # false by default
```

The `pk` (a YamlPK key) is formatted as such for default values and is somehow a `reference`. `keyFields` and `Type` tokens are already defined for `reference`.

The following shows the format of a `pk`:

```
/Top-level-Folder/(Type)keyFields[/(Type)keyFields]*
```

In a `reference` syntax, the `type` is generally not specified, but in Entity type files it is mandatory. As a result, `pk` are parsable.

Examples of `pk` usage:

```
/System/(CategoryGroup)Filter Categories/(Category)miscellaneous
```

```
/System/(NamespacesConfiguration)Namespace Configuration/(WSSENamespace)http://schemas.xmlsoap.org/ws/2002/12/secext
```

This way of writing a `reference` expects the format described in the [reference](#reference-syntax) section, therefore it *cannot* be used in entity fields.
