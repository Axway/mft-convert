{
"title": "Edit the YAML configuration",
"linkTitle": "Edit the YAML configuration",
"weight":"70",
"date": "2020-09-24",
"description": "Learn how to edit the YAML configuration."
}

You can use the file explorer from your operating system to navigate the user friendly directory structure of a YAML configuration, and you can use any standard text editor to view and edit the individual YAML files.

Ideally, a standard IDE can be used to create a project and edit the files of a YAML configuration. The externalized files for scripts, JSON, and so on allow you to edit these files in an editor that is syntax-aware.

## Edit a YAML configuration with ES Explorer

[ES Explorer](/docs/apigtw_devguide/entity_store/#use-the-es-explorer) can be used with some limitations to view, navigate, and edit a YAML configuration. To connect to a YAML configuration, enter a URL of the form `yaml:file:/root/dir/of/yaml`.

## Create a new policy

Policy Studio is not supported for YAML configuration as yet, but making minor changes to an existing policy, or other configuration, using an IDE with YAML syntax checking should be straightforward. You must also use the `yamles` validate option to check the validity of the configuration.

Building a new complex policy without an existing YAML format example might be challenging. To simplify this level of development, you can use the ES Explorer tool or Policy Studio as follows:

1. Create the structure of the policy.
2. Export the policy to a configuration fragment.
3. Convert the XML configuration fragment to YAML using `yamles frag2yaml` command.
4. Import the YAML configuration fragment into your main YAML project using `yamles import` command.
5. Repeat the export, `frag2yaml`, and `import` steps as many times as required to import all the required configuration into your main YAML configuration.

We recommend you to develop a new complex policy in an XML project that contains all the configuration you previously converted to YAML, so that you can reference other existing configuration. You must ensure that all referenced entities that need to be resolvable can be resolved eventually.

### Convert the XML configuration fragment to YAML

Certificates and private keys that exist in the XML federated configuration, which then get converted to YAML format, will be formatted as is, without header (starting line `-----BEGIN...`) and footer (ending line `-----END...`).

The related certificate and private key files in the YAML configuration will be formatted as follows:

`Axway-converted.yaml`

```yaml
---
type: Certificate
fields:
  dname: Axway-converted
  key: '{{file "Axway-converted-key.pem"}}'
  content: '{{file "Axway-converted-cert.pem"}}'
```

`Axway-converted-key.pem`

```
---
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDLd+Zv/rC6Wf/FAGc6YM8APIZD
MgEtJjOWKJiOajzI22GFsUcU3XYfV8oayI12j67IMVaD1RU+o+u6D+Y7tPj60/kvqORAjpnad7OS
0zs59KMDvfXghM3EkN8/mDxb11yK19VMJvk2ZMcpRn/6/gCFbpa6Nj8OpF7oLCRhbcue1R+JiSLF
R9zbJzxfkgkQ9Q256+ZMlOgYdrqpytTltYPOrKD3vKR9DiOTc/KC5kCZFmkgDJz5AQTpTtvZCXJW
aro+PbAqbyc//ABTTrhTIBkyB+aFM49hg/Ej90KitvxI/Nu4lzT2XiDd9wqiTzElYM+Q0UMk0BFd
vnckh1sLYHe3AgMBAAECggEABK9VEf0WSqQp3Hpe5hw2h/XczY1IM6buhyWWJalSjvlmLHLhhRx4
TM5zq9w0TaePSbLBIDX20ENr+RPGpFdNaFEbKrrDzqy55CrfaqEMexAj9MEZ+Tp1lnITgd5afW7f
BZ9knOVE1bjKUSv7ZGcW0fuy4sS+/PJR8RybFdc2WgjZt0kw8L9fUsmq/M4ye00DVOoI/eTGjfky
ooVtUAJJXYI1FMvUuyC43qJuOMpfme3xY79AOQpXBvTamG/xpr7/MX5Q4yxu5GEo7eLk91xwSKFG
OYgC33SLgxOIRecLxgxZp72g4GZqGLzcjmTDxpMgS32RzbRtqDNCp5Twjvw0aQKBgQDmzo6WAMa9
jCbEtXsqCQ4CkCYbMJp3d9Jxzk6R2kjwMRQLDL+P+YkIMd/tntrd3DS2vbdbzlGxFkDROhpV+T+T
iYlqFkY60cUaeGBWyWTWkKksrLfsJz4M5/seflYX4L9H5vjDChybzpaViTUHof1xB6f44fheeW82
sV7v+UQQ9QKBgQDhrWwtGJjccJgAfZwqnBd0snKDBUjpSlUgBQnyYzNXbM9IN9q4vaHymt5Zgg5f
do4Bfjdmx4O3W6RtIysmw92hyad8SIbl2JNN15FwQQ2ZpgaIgsUyVQGywtkiXzFGnir3YyEjtb/S
5y1X4Au2mnof1K2Q9UYUKeEsoLFNDj/KewKBgQCddz8AR/dPSlcIzWgB/bt5NC9LTZWU/EKvMjmY
eHxaoqEyCLiI2Y4L8Tr9OuvHgXzVUAnQsKo7TxtZo3JkRXCCj8sYfancZ1E6BfZ0P2J0oK1KtWul
ygAjfgFthHPoRoU54PLG3hc2lXNXAg0T8AihHMAUpNZ2XhLqFYjX4A/4IQKBgQCldRF3qq4ACKjY
yz8g2lo0G9TrDIfdSrtIg4k8ZdCxizwZ1aGNirLefP8CHuFMyk3o+FHEEAkY+J5/yaYMgNPQl1kt
PLtybqvpCWA/LeK7wMbPdRkBAuQA3Ox3T9V/0dzsjYgxd0JRbV6IK+JKmc1p4vLx8XHUvLOzlYkI
VqccWwKBgByv6gva+WWT5H7CZCicURkLdkRx0RGJjZKW5m7uwWPLAkb5zVozsCSGhNZDX3xuPzqx
z+m92RPtpagPgowuDomFodteFZOsSjCkvbiwx7JGEsvwEraMwxYGPg3sEpVa+bS/4Ptpj8llGMpN
NWjvpIPxQaBeo3FHRDPFjvc7qjUP
```

`Axway-converted-cert.pem`

```
---
MIICrTCCAZUCBgE9RY7XxjANBgkqhkiG9w0BAQUFADAaMRgwFgYDVQQDEw9TYW1wbGVzIFRlc3Qg
Q0EwHhcNMTMwMzA3MTU1MzAwWhcNMzcwMTEwMTA1NjAwWjAaMRgwFgYDVQQDEw9TYW1wbGVzIFRl
c3QgQ0EwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDLd+Zv/rC6Wf/FAGc6YM8APIZD
MgEtJjOWKJiOajzI22GFsUcU3XYfV8oayI12j67IMVaD1RU+o+u6D+Y7tPj60/kvqORAjpnad7OS
0zs59KMDvfXghM3EkN8/mDxb11yK19VMJvk2ZMcpRn/6/gCFbpa6Nj8OpF7oLCRhbcue1R+JiSLF
R9zbJzxfkgkQ9Q256+ZMlOgYdrqpytTltYPOrKD3vKR9DiOTc/KC5kCZFmkgDJz5AQTpTtvZCXJW
aro+PbAqbyc//ABTTrhTIBkyB+aFM49hg/Ej90KitvxI/Nu4lzT2XiDd9wqiTzElYM+Q0UMk0BFd
vnckh1sLYHe3AgMBAAEwDQYJKoZIhvcNAQEFBQADggEBAECtUaJVPApyeJrxqb3j49QbtL3rhusQ
ae70epkkU0XFu+8NuYoz4pVs5Mj9g0HPXXvRvgJAQzCzQ34XVFZGB18IjOkoyu0RodTadGi7fAH5
0B/SEteGRhYV1JzE+IKxG3n6nTrrHgINL8+QL4eHiozEKfJ/vWqw1j2Zc/47vcalGYYqTJyrSmJy
pX6tVTYRbArvwaU+LzYLjEJkWgfZc09eO8JiA1ZwE4ud5o9Tpbj5IdUQkoLyPTxtU0a6A2PMAs4j
MTnD3Mh2YRB4gGCAPDN+Xl999ls2KCoVdLjXIhOD2YwWXGmv3vaAUyc32wwcro69/wXvj41tFIez
w98SGM8=
```

## Add a new certificate and private key to a YAML configuration

This section covers the steps required to add a new certificate and private key to an existing YAML configuration.

1. You must have a certificate in a `PEM` file and its related private key in a separate `PEM` file. For example, `Axway-cert.pem` and `Axway-key.pem`. The private key cannot be encrypted with OpenSSL, as it is not supported.

2. For testing purposes, you can generate an unencrypted private key and a self-signed certificate PEM files using OpenSSL as follows:

    ```
    > openssl req -nodes -new -x509 -keyout Axway-key.pem -out Axway-cert.pem
    Generating a RSA private key
    ................+++++
    ...............................................................................+++++
    writing new private key to 'Axway-key.pem'
    -----
    You are about to be asked to enter information that will be incorporated
    into your certificate request.
    What you are about to enter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', the field will be left blank.
    -----
    Country Name (2 letter code) [AU]:IE
    State or Province Name (full name) [Some-State]:Dublin
    Locality Name (eg, city) []:
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:Axway
    Organizational Unit Name (eg, section) []:
    Common Name (e.g. server FQDN or YOUR name) []:Axway
    Email Address []:axway@axway.com
    ```

3. Copy the `Axway-cert.pem` and `Axway-key.pem` files into the `/Environment Configuration/Certificate Store` directory in your YAML entity store.

4. Create the `/Environment Configuration/Certificate Store/Axway.yaml` file in your YAML entity store:

    ```yaml
    ---
    type: Certificate
    fields:
      dname: Axway
      key: '{{file "Axway-key.pem" "pem"}}'
      content: '{{file "Axway-cert.pem" "pem"}}'
    ```

    The `pem` option in `{{file}}` placeholder is mandatory when using a `PEM` file containing header and footer.

5. Edit another entity that requires a certificate and private key, for example, an `XML Signature filter` (see the `signingCert` field in the following example). It now points to the new certificate and private key via its YamlPK `/Environment Configuration/Certificate Store/Axway`.

    ```yaml
    ---
    type: FilterCircuit
    fields:
      name: cert
      start: ./XML Signature Generation
    children:
    - type: GenerateSignatureFilter
      fields:
        signingCertAttribute: ""
        keyWrapAlgorithm: http://www.w3.org/2001/04/xmlenc#rsa-oaep-mgf1p
        symmetricKeyEncryptionCertAttribute: ""
        name: XML Signature Generation
        signingCert: /Environment Configuration/Certificate Store/Axway   # <-- certificate ref
        symmetricKeyAttribute: ""
        wsuIdElementSpecification:
        - /System/Element Specifiers/http://schemas.xmlsoap.org/soap/envelope/,Body,1
        - /System/Element Specifiers/http://www.w3.org/2003/05/soap-envelope,Body,1
        attachmentTransform: ""
      routing:
        success: ../XML Signature Verification
      logging:
        fatal: 'Error during message signing. Error: ${circuit.exception}'
        failure: Failed to sign message
        success: Signed message successfully
      children:
      - type: KeyInfoFormat
        fields:
          keyNameValue: CN
          publicKeyInfoMask: 15
          keyNameType: 2
          certAttachmentId: ""
    ```

If the YAML configuration you are adding the certificate and private key into is encrypted with a passphrase, you will need to encrypt the private key file. To encrypt the private key file `Axway-key.pem`, execute all the previous steps and also perform the following:

```shell
cd apigateway/posix/bin
./yamles encrypt --file ~/yaml/Environment\ Configuration/Certificate\ Store/Axway-key.pem --source /home/user/yaml --passphrase changeme
The file `/home/user/yaml/Environment Configuration/Certificate Store/Axway-key.pem` has been encrypted
```

* `yamles encrypt` with `--file` option only supports encryption of `PEM` file containing header and footer.

* The YAML configuration referenced in the `--source` parameter of the `encrypt` command must be the destination YAML configuration to ensure it is encrypted correctly.

* After you complete editing the YAML configuration, create a `.tar.gz` file and deploy it to your running API Gateway. Your API Gateway group must use the same encryption passphrase.

{{< alert title="Note">}}Encrypting a private key via `openssl` and adding it to the YAML configuration is not supported.{{< /alert >}}

## Manually create an entity instance YAML file

To create an entity instance of [`NetService` type](/docs/apim_yamles/apim_yamles_references/yamles_types), perform the following:

1. Search for the `NetService.yaml` file within the `META-INF/types` directory.

    To know what fields can be used, move up to the ancestor types:

    * `NetService.yaml` is contained within `JavaProcess` directory. Check the fields within YAML entity type file `JavaProcess.yaml`.
    * `JavaProcess.yaml` is contained within `Process` directory. Check the fields within YAML entity type file `Process.yaml`.
    * Continue until you reach the root directory `Entity`.

2. Search for components.

    Use the same approach you have used to search for fields. `NetService` has two components: `LoadableModule` and `ClassLoader`. Search for `LoadableModule.yaml` and `ClassLoader.yaml` YAML files within the `META-INF/types` directory.
3. Execute all the previous steps again to get all required and optional fields for each entity type.

### Create a container entity

If the entity instance you have created is a [container entity](/docs/apim_yamles/yamles_structure#_parentyaml):

1. Create a new directory with the same name of the new entity instance.
2. Create a YAML file named `_parent.yaml`.
3. Add the YAML file to the newly created directory.

Example:

```yaml
---
type: NetService
fields:
  name: MyService
```

![new container entity](/Images/apim_yamles/yamles_new_container_entity.png)

### Create a non-container entity

If the entity instance you have created is not a container entity:

1. Create a YAML file named like the **name** field.
2. Add the YAM file to the correct directory.

Example:

```yaml
---
type: InetInterface
fields:
  port: 8080
  name: My Traffic HTTP Interface
```

![new entity](/Images/apim_yamles/yamles_new_entity.png)

Despite what is in the model, some fields that are said to be mandatory (cardinality=1 and no default value) are not mandatory. Required fields and default values can be checked in Policy Studio on an XML-based project by creating the same type of entity and observing what is allowed and created by default.

## YAML syntax considerations

When modifying your YAML configuration, consider the following:

### Strings fields

For strings literal we chose the "plain mode" of YAML as it is more readable, here are some caveat you want to avoid:

| Syntax                                 | Status    |
|----------------------------------------|-----------|
| `name: Foo`                            | OK        |
| `name: "Foo"`                          | OK        |
| `name: ${system.host}`                 | Error     |
| `name: "${system.host}"`               | OK        |
| `name: '${system.host}'`               | OK        |
| `name: "Please avoid trailing spaces "`| OK        |
| `name: " Please avoid leading spaces"` | OK        |
| `name:     So many whitespaces !`      | Warning   |

For the last case, leading and trailing spaces will be removed. The value will be "`So many whitespaces !`"

### int, long, and boolean fields

Quotation marks are not recommended for int, long, and boolean fields.

| Syntax                          | Status                      |
|---------------------------------|-----------------------------|
|`integerField: 0`                | OK                          |
|`integerField: -1`               | OK                          |
|`integerField: 9999999999999999` | Error (it a long)           |
|`integerField: 100_000`          | Error                       |
|`integerField: "1"`              | Avoid (though it will work) |
|`longField: 21546873548687`      | OK                          |
|`longField: 1_000_000_000_000`   | Error                       |
|`longField: "21546873548687"`    | Avoid (though it will work) |
|`booleanField: true`             | OK                          |
|`booleanField: false`            | OK                          |
|`booleanField: 0`                | Avoid (though it will work) |
|`booleanField: 1`                | Avoid (though it will work) |
|`booleanField: yes`              | Avoid (though it will work) |
|`booleanField: no`               | Avoid (though it will work) |

### Lists

YAML Entity Store exports lists without indentation.

The following is an example of how YAML Entity Store writes a YAML file. This is done by tools, such as ES Explorer, when updating or creating and Entity.

```yaml
type: FilterCircuit
fields:
  name: ...
# some other fields
children:
- type: ...
  fields:
    name: ...
    multi:
    - value 1
    - value 2
  children:
  - type: ...
    fields:
# some other stuff
```

The following example is also supported:

```yaml
type: FilterCircuit
fields:
  name: ...
# some other fields
children:
  - type: ...
    fields:
      name: ...
      multi:
        - value 1
        - value 2
    children:
      - type: ...
        fields:
# some other stuff
```

{{% alert title="Note" color="primary" %}}`refs: {}` is an empty list.{{% /alert %}}