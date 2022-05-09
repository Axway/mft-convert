{
"title": "Known validation errors",
"linkTitle": "Known validation errors",
"weight":"60",
"date": "2020-09-25",
"description": "Learn how to fix errors that might occur when validating your YAML configuration."
}

This section covers examples of issues that might occurs while using the `validate` option of the `yamles` CLI tool. All errors are shown on `stdout`.

Note that `WARNINGs` will only be listed on `stdout` if there are other `ERRORs`.
If only `WARNINGs` are found, they are listed in the trace file only.

## Incorrect value for an integer field

**Severity**: ERROR

```
entity=/External Connections/DB Connections/MySQL/local, type=DbConnection, field='initialSize', file=/home/user/yaml/External Connections/DB Connections/MySQL.yaml, message='Expected to find data of type <integer> but value is <4.5>'
```

Look into the file named `/home/user/yaml/External Connections/DB Connections/MySQL.yaml`. It contains an entity of type DbConnection named `local`, with a field named `initialSize`. Its value is set to `4.5`, and an integer is expected by the model. Fix the field value to be a valid integer.

## Incorrect value for a long field

**Severity**: ERROR

```
entity=/External Connections/DB Connections/MySQL/local, type=DbConnection, field='maxWait', file=/home/user/yaml/External Connections/DB Connections/MySQL.yaml, message='Expected fo find data of type <long> but value is <9999999999999999999999999999999999>'
```

Look into the file named `/home/user/yaml/External Connections/DB Connections/MySQL.yaml`. It contains an entity of type `DbConnection` named `local`, with a field named `maxWait`. Its value is set to `9999999999999999999999999999999999`, and a `long` is expected by the model. Fix the field value to be an valid `long`. Max long value is `9.223372e+18 2⁶³-1`.

## Incorrect value for an encrypted field

**Severity**: ERROR

```
entity=/External Connections/DB Connections/MySQL/local, type=DbConnection, field='password', file=/home/yaml/External Connections/DB Connections/MySQL.yaml, message='Value for field of type encrypted is not a legal base64 string. Value: *#{[]#}#&&'
```

Look into the file named `/home/user/yaml/External Connections/DB Connections/MySQL.yaml`. It contains an entity of type `DbConnection` named `local`, with a field named `password`. Its value is set to `'*#{[]#}#&&'` but a base64 encoded string is expected. If your YAML entity store is unencrypted, fix this by encoding the database connection password with base64 as follows:

```
echo -n 'changeme' | openssl base64
```

It will output: `Y2hhbmdlbWU=`

Then, set the value for the `password` to be `Y2hhbmdlbWU=` as follows:

```yaml
---
type: DbConnection
fields:
  .....
  password: Y2hhbmdlbWU=
```

Or, you can set the value for the `password` as follows:

```yaml
---
type: DbConnection
fields:
  .....
  password: '{{base64 "changeme"}}'
```

If your entity store is encrypted, you must encrypt the `password` and encode it with base64. Use the yamles CLI encrypt option for this:

```
./yamles encrypt --text "mydatabasepassword" --passphrase "myentitystorepassphrase"

Your encrypted base64 encoded string content is:-
7ScBASwwV19S+dgmMVbirMxkqGE4bl9nyyvw6nLyzfI=
```

Then, set the value for the `password` to be `7ScBASwwV19S+dgmMVbirMxkqGE4bl9nyyvw6nLyzfI=` as follows:

```yaml
---
type: DbConnection
fields:
  .....
  password: 7ScBASwwV19S+dgmMVbirMxkqGE4bl9nyyvw6nLyzfI=
```

## Incorrect value for a certificate

**Severity**: ERROR

```
entity=/Environment Configuration/Certificate Store/Whatever, type=Certificate, field='content', file=/home/user/yaml/Environment Configuration/Certificate Store/Whatever.yaml, message='Value for field of type binary is not a legal base64 string. Value: MIIDdzCCAl+gAwIBAgIIXDPLYixfszIwDQYJKoZIhvcNAQELBQAwPDEeMBwGA1UEAwwVQXRvcyBU
cnVzdGVkUm9vdCAyMDExMQ0wCwYDVQQKDARBdG9zMQswCQYDVQQGEwJERTAeFw0xMTA3MDcxNDU4
MzBaFw0zMDEyMzEyMzU5NTlaMDwxHjAcBgNVBAMMFUF0b3MgVHJ1c3RlZFJvb3QgMjAxMTENMAsG
A1UECgwEQXRvczELMAkGA1UEBhMCREUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCV
hTuXbyo7LjvPpvMpNb7PGKw+qtn4TaA+Gke5vJrf8v7MPkfoepbCJI419KkM/IL9bcFyYie96mvr
54rMVD6QUM+A1JX76LWC1BTFtqlVJVfbsVD2sGBkWXppzwO3bw2+yj5vdHLqqjAqc2K+SZFhyBH+
DgMq92og3AIVDV4VavzjgsG1xZ1kCWyjWZgHJ8cblithdHFsQ/H3NYkQ4J7sVaE3IqKHBAUsR320
HLliKWYoyrfhk/WklAOZuXCFteZI6o1Q/NnezG8HDt0Lcp2AMBYHlT8oDv3FdU9T1nSatCQujgKR
z3bFmx5VdJx4IbHwLfELn8LVlhgf8FQieowHAgMBAAGjfTB7MB0GA1UdDgQWBBSnpQaxLKYJYO7R
l+lwrrw7GWzbITAPBgNVHRMBAf8EBTADAQH/MB8GA1UdIwQYMBaAFKelBrEspglg7tGX6XCuvDsZ
bNshMBgGA1UdIAQRMA8wDQYLKwYBBAGwLQMEAQEwDgYDVR0PAQH/BAQDAgGGMA0GCSqGSIb3DQEB
CwUAA4IBAQAmdzTblEiGKkGdLD4GkGDEjKwLVLgfuXvTBznk+j57sj1O7Z8jvZfza1zv7v1Apt+h
k6EKhqzvINB5Ab149xnYJDE0BAGmuhWawyfc2E8PzBhj/5kPDpFrdRbhIfzYJsdHt6bPWHJxfrrh
TZVHO8mvbaG0weyJ9rQPOLXiZNwlz6bb65pcmaHFCN795trV1lpFDMS3wrUU77QR/w4VtfX128a9
61qn8FYiqTxlVMYVqL2Gns2Dlmh6cYGJ4Qvh6hEbaAjMaZ7snkGeRDImeuKHCnE96+RapNLbxc3G
3mB/ufNPRJLvKrcYPqcZ2Qt9sTdBQrC6YB3y/gkRsPCHe6ed[`
```

Look into the file named `/home/user/yaml/Environment Configuration/Certificate Store/Whatever.yaml`. It contains an entity of type `Certificate`, named `Whatever`, with a field named `content`. Its value is set to a filename that holds the base64 encoded content of your certificate. Check it contains base64 encoded content only.

## Reference field refers to an entity that does not exist

**Severity**: ERROR / WARNING

```
entity=/Policies/My policy/Sample policy, type=FilterCircuit, field='category', file=/home/user/yaml/Policies/My policy/Sample policy.yaml, message='Cannot resolve reference: /System/Policy Categories/invalidcategory'
```

Look into the file named `/home/user/yaml/Policies/My policy/Sample policy.yaml`. It contains an entity of type `FilterCircuit`, named `Sample Policy`, with a field named `category`, that is pointing to an entity that does not exists in the configuration. Fix the `YamlPK` value in the `category` field to something like `/System/Policy Categories/authentication`. In this case, removing the field would also fix the issue as long as the default value of `/System/Policy Categories/miscellaneous` is what you want.

If you are validating a configuration project that points to entities that are contained in another project, that will be merged together later. You can use the `--allow-invalid-ref` parameter to downgrade these issues from an ERROR to a WARNING in the trace file. The same message with a severity of WARNING will show in this case.

## Reference field refers to a selector

**Severity**: WARNING

```
entity=/Environment Configuration/Services/5555, type=SSLInterface, field='serverCert', file=/home/user/yaml/Environment Configuration/Services/5555.yaml, message='${env.SSL.CERT} is used as a reference. Make sure envSettings.props or system.properties is populated accordingly'
```

A shorthand key must be set in `envSettings.props` for `env.SSL.CERT`, for example, `env.SSL.CERT=/[Certificates]name=Certificate Store/[Certificate]dname=O=listener\,CN=localhost`, for the gateway to pick up the certificate at startup. If a selector starts with `${system.`, you must add a setting to the `system.properties` file.

This allows users to environmentalize a certificate for a given instance rather than for the entire API Gateway group which shares the same configuration.

## Invalid value for environmentalized certificate reference field

**Severity**: ERROR / WARNING

```
entity=/Policies/cert/XML Signature Generation, type=GenerateSignatureFilter, field='signingCert', file=/home/user/yaml/Policies/cert.yaml, message='Cannot resolve reference: /Environment Configuration/Certificate Store/invalid field'
```

Look into the file named `/home/user/yaml/Policies/cert.yaml`. It contains an entity of type `GenerateSignatureFilter`, with a field named `signingCert`, which points to a certificate via a reference that does not exist in the configuration. The reference might be environmentalized using a YamlPK like this: `/Environment Configuration/Certificate Store/{{env "CERT"}}`, which has resulted in the "invalid field" in the error message. This issue occurs when you try to validate an environment where the system environment variable `CERT` does not exist. To avoid errors, use the `--allow-invalid-ref` option, or set a dummy value for the environment variable `CERT` in the validating environment.

## Reference field refers to an entity of the wrong type

**Severity**: ERROR

```
entity=/Policies/My policy/Sample policy, type=FilterCircuit, field='category', file=/home/user/yaml/Policies/My policy/Sample policy.yaml, message='Reference '/Policies/My policy/Sample policy/My IP Filter' leads to an entity of type IpFilter instead of PolicyCategory'
```

Look into the file named `/home/user/yaml/Policies/My policy/Sample policy.yaml`. It contains an entity of type `FilterCircuit`, named `Sample Policy`, with a field named `category`, which points to an entity that exists in the configuration but is of the wrong type. It is pointing to an entity of type `IPFilter`, but it needs an entity of type `PolicyCategory`. Fix the YamlPk value in the field `category` to something like `/System/Policy Categories/authentication`. In this case, removing the field would also fix the issue as long as the default value of `/System/Policy Categories/miscellaneous` is what you want.

## Missing required field

**Severity**: ERROR

```
ERROR: EntityStoreException: com.vordel.es.EntityStoreException: No field found with name 'test' for type 'IpFilter' for child entity of /Policies/My policy/Sample policy. Error occurred in file: /home/user/yaml/Policies/My policy/Sample policy.yaml
Cause: com.vordel.es.EntityStoreException: No field found with name 'test' for type 'IpFilter' for child entity of /Policies/My policy/Sample policy
```

Look into the file named `/home/user/yaml/Policies/My policy/Sample policy.yaml`. It contains an entity of type `IpFilter`, named `Sample Policy`, with a field named `test`, but a field named `test` does not exist in the model for this entity type. To fix, remove the field `test`.

## YAML format - indentation error

**Severity**: ERROR

```
ERROR: EntityStoreException: com.vordel.es.EntityStoreException: Error while processing file: /home/user/yaml/Policies/Policy Library/Health Check.yaml: while parsing a block mapping
in 'reader', line 2, column 1:
type: FilterCircuit
^
expected <block end>, but found '<block mapping start>'
in 'reader', line 5, column 3:
start: ./Set Message
^

at [Source: (StringReader); line: 5, column: 3]
Cause: com.fasterxml.jackson.dataformat.yaml.snakeyaml.error.MarkedYAMLException: while parsing a block mapping
in 'reader', line 2, column 1:
type: FilterCircuit
^
expected <block end>, but found '<block mapping start>'
in 'reader', line 5, column 3:
start: ./Set Message
^

at [Source: (StringReader); line: 5, column: 3]
Cause: while parsing a block mapping
in 'reader', line 2, column 1:
type: FilterCircuit
^
expected <block end>, but found '<block mapping start>'
in 'reader', line 5, column 3:
start: ./Set Message
^
```

Check for YAML indentation issues in file `/Policies/Policy Library/Health Check.yaml`

## YAML format - Unescaped special character

**Severity**: ERROR

```
ERROR: EntityStoreException: com.vordel.es.EntityStoreException: Error while processing file: /home/user/yamlconfig/Policies/Policy Library/Health Check.yaml: while scanning an alias
in 'reader', line 10, column 11:
body: *** Welcome ***
^
expected alphabetic or numeric character, but found *(42)
in 'reader', line 10, column 12:
body: *** Welcome ***
^
```

In this case, a field in the file `/home/user/yamlconfig/Policies/Policy Library/Health Check.yaml` is set as follows:

```yaml
body: *** Welcome ***
```

but should be changed to

```yaml
body: "*** Welcome ***"
```

As the `*` character needs to be contained within double quotation. Refer to the [YAML standard](https://yaml.org/spec/1.2/spec.html#Characters) to discover what characters are considered as special characters in YAML.

## No value to substitute for {{env XYZ}}

**Severity**: Non-fatal ERROR

```
No value to substitute for {{env "MY_ENV_VARIABLE"}}
```

You might see this in the `yamles` trace file at validation time because the validation environment does not have the environment variable `MY_ENV_VARIABLE` defined. This is not a problem. The environment variable `MY_ENV_VARIABLE` only needs to be defined in the runtime environment where the API Gateway is running. If you see this at deployment time in the API Gateway trace, create the environment variable in the API Gateway's environment and restart the API Gateway.

## Invalid value for boolean field

**Severity**: WARNING

```
entity=/External Connections/DB Connections/MySQL/local, type=DbConnection, field='setPoolPreparedStatements', file=/home/user/yaml/External Connections/DB Connections/MySQL.yaml, message='Value will be set to 'false' but is neither falsy or truthy. Value: truex'
```

Look into the file named `/home/user/yaml/External Connections/DB Connections/MySQL.yaml`. It contains an entity of type `DbConnection`, named `local`, with a field named `setPoolPreparedStatements`. Its value is set to `truex`, but a boolean is expected by the model. Fix the field value to be an valid boolean (`true` or `false`). The value defaults to `false`.

## Filter with same name

**Severity**: WARNING

```
WARNING: Found entities of different types at same level with same PK for parent PK :'/Policies/Some Policies/Test' PK end with: (ChangeMessageFilter)Same name filter
WARNING: Found entities of different types at same level with same PK for parent PK :'/Policies/Some Policies/Test' PK end with: (Reflector)Same name filter
```

This is generated at conversion time using the `fed2yaml` option. To fix after converting, edit the policy file (`/Policies/Some Policies/Test.yaml`), and rename one of the filters.  Update all references to the filters so that the correct YamlPK is used:

```yaml
---
type: FilterCircuit
fields:
  name: Test
  start: ./Same name filter
  description: ""
children:
- type: Reflector
  fields:
    name: The Reflector
  logging:
    fatal: 'Error occurred while echoing the message. Error: ${circuit.exception}'
    failure: Failed to echo back the message
    success: Successfully echoed back the message
- type: ChangeMessageFilter
  fields:
    name: Same name filter
    body: <body>Test!</body>
    outputContentType: text/html
  routing:
    success: ../The Reflector
  logging:
    fatal: 'Error in setting the message. Error: ${circuit.exception}'
    failure: Failed in setting the message
    success: Success in setting the message
```

The fix is optional, but will lead to a more easily understood policy.

## Unset property in envSettings.props

**Severity**: WARNING

```
WARNING: entity=/System/AMA loadable module, type='AmaModule', field='port', file=/home/user/yaml/System/AMA loadable module.yaml, message='integer value is set to ${env.BROKER.PORT}. Make sure the selector value is integer'
```

Check that the value in the `envSettings.props` for `env.BROKER.PORT` is an integer. No further action is required.
