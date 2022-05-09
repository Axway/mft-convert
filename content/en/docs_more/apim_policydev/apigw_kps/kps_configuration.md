{
"title": "Configure KPS in Policy Studio",
  "linkTitle": "Configure in Policy Studio",
  "weight": "40",
  "date": "2020-01-06",
  "description": "Define general KPS configuration in Policy Studio."
}

For details on data source-specific configuration, see the following topics:

* [Configure Apache Cassandra KPS storage](/docs/apim_policydev/apigw_kps/configure_database_storage/#configure-apache-cassandra-kps-storage)
* [Configure database KPS storage](/docs/apim_policydev/apigw_kps/configure_database_storage/#configure-database-kps-storage)
* [Configure file-based KPS storage](/docs/apim_policydev/apigw_kps/configure_database_storage/#configure-file-based-kps-storage)

{{< alert title="Caution" color="warning" >}}Do not edit the default KPS tables in Policy Studio unless under strict supervision from Axway Support. This includes the **API Server**, **OAuth**, or **API Portal** KPS tables available under **Environment Configuration** > **Key Property Stores**.{{< /alert >}}

## Configure a KPS collection

A KPS collection is a set of related KPS tables. To configure a KPS collection, perform the following steps:

1. In the Policy Studio tree, under **Environment Configuration**, right-click **Key Property Stores**, and select **Add Key Property Store**.
2. Specify the following settings in the dialog:
    * **Name**: A collection must have a unique name in the API Gateway group.
    * **Description**: You can provide an optional description.
    * **Alias prefix**: You can also specify an alias prefix, but in normal usage, you can leave this field blank.
    * **Default data source**: A collection has a default data source where all data for all tables in the collection is stored. You can change this data source or assign a different data source to individual tables in the collection.
3. When the collection is created, you can specify a **Cache** (only local caches are supported) for storage and retrieval of selector results. This will improve selector read performance for storage backends such as databases.

The following shows a KPS collection created in Policy Studio:

![Example KPS collection](/Images/APIGatewayKPSUserGuide/03000017.png)

## Configure a KPS table

A KPS table is a user-defined table managed by the KPS in the API Gateway. To configure a KPS table, perform the following steps:

1. Right-click a KPS collection in the Policy Studio tree, and select **Add Table**.
2. Specify the following settings in the dialog:
    * **Name**: A KPS table must have a unique name in the collection.
    * **Description**: You can provide an optional description.
    * **Override the default data source with the following**: You can specify a different data source than the collection if required.
    When the table is created, if required, you can use the **Override the default data source with the following** setting to specify a different data source than the collection:

        ![Example KPS table](/Images/APIGatewayKPSUserGuide/03000018.png)
3. Finally, click the **Structure** tab, and click **Add** to define the structure of the data stored in the table.

## KPS aliases

KPS tables are accessed by aliases. A table must have at least one alias. Aliases must be unique in an API Gateway group. You also can use the optional alias prefix for the collection to help  ensure that the alias is unique.

The full alias of a table is the *collection alias prefix* and the *table alias* combined. For example, `samples` and `User` gives `samplesUser`. If unspecified, the default value of the alias prefix for the collection is an empty string (for example, `User` only).

## KPS data sources

A KPS collection has one active data source associated with it. All tables in the collection use this data source by default. You can configure a table to use a different data source if  required (see [Configure a KPS table](#configure-a-kps-table)).

## KPS table structure

When creating a KPS table, you must define the structure of the data that is stored in that table. This consists of property (column) names and types.

Do not use just `key` as the property (column) name, because this causes deployment errors.

You can choose from the following types:

| Property  | Type |
| --- | --- |
|`String`, `Boolean`, `Byte`, `Integer`, `Long`, `Double` | Java type.|
|`List` | Java List of any one of the above Java types.|
|`Map` | Java Map. The key can be any one of the above Java types. The value can be any one of the above Java types.|

### Edit the structure of an existing KPS table

You cannot edit the structure of a KPS table that already exists in Cassandra. If you attempt to do so, you will find out that the structure of the KPS table has not changed.

To edit the structure of an existing KPS table:

1. Modify the alias of the old table.
2. Create a new table with the desired schema and the original alias.
3. Deploy the configuration.
4. Transfer the data.

If your only change was to add new properties to the KPS table, you can make a `kpsadmin` backup of the table prior to modifying the alias, then restore the data from that backup after.

## Query tables using properties and keys

You can directly access records in a table by specifying certain property names and values, without needing to read all the records in the table. You can use only properties of `String`, `Long`, and `Integer` types for this purpose. These properties are known as *indexable properties*.

Indexed properties include primary keys, secondary keys (which are indexed implicitly), and other properties that you explicitly select as **Indexed** in Policy Studio.

![Getting Started - User table in Policy Studio](/Images/APIGatewayKPSUserGuide/03000002.png)

### Primary key

You can directly access any record using its *primary key*. All records in the table must be accessible using unique primary keys. You must select one **Primary Key** per table in Policy studio. The specified property must be an indexable property. Primary key values cannot be null.

### Secondary key

You can optionally access any record directly using a unique secondary key. The secondary key can be a simple key (for example, `email` ) or a composite key (for example, `appId` or `companyId`). The specified properties must be indexable properties. The secondary key (and parts of a composite secondary key) cannot be null. You can specify one secondary key per table. A common use  case is to specify an internal unique ID as a primary key, and an external user facing ID as a secondary key. For example, `id` for internal primary key, and `email` for external ID as a  secondary key.

### Selector access

You can access records in a KPS table using an API Gateway selector. If a secondary key is defined for the table, you must specify all secondary key values in the selector. If no secondary key
is defined, you must specify the primary key value instead. In Policy Studio, you can specify a secondary key or a primary key in the **Use the following property name(s) for looking up a table from a selector** field.

#### Selector syntax

The KPS selector syntax is as follows:

```
${kps.alias[key].property}
```

The parts in the selector are described as follows:

| Selector part | Description    |
|---------------|----------------|
| `${`          | Indicates the start of the selector using a `{` bracket.|
| `kps`           | Specifies that selector should query a KPS table.|
| `.alias`        | Specifies the full alias of the KPS table, including the collection alias prefix if any (for example, `User`). |
| `[`             | Indicates the start of a table property reference using a `[` bracket. |
| `key`           | The key value to query the table (for example, `http.querystring.id`).  |
| `]`           | Indicate the end of a table property reference using a `]` bracket. |
| `.property`     | The field to retrieve from the returned row (for example, `age`).  |
| `}`             | Indicate the end of the selector using a `}` bracket. |

You can also use a composite key, for example:

```
${kps.alias[key1][key2].property}
```

#### Selector examples

For an example of accessing KPS data from a selector using a primary key, see [Get started with KPS](/docs/apim_policydev/apigw_kps/get_started/).

For examples of selectors that use both primary and composite keys, see [Configure database storages](/docs/apim_policydev/apigw_kps/configure_database_storage/).

##### Example 1

```
${kps.User[http.querystring.id].firstName}
```

* Get row from KPS table with `User` alias.
* Use key supplied in HTTP query string (`id`).
* Return `firstName` field of row.

##### Example 2

```
${kps.User["kathy.adams@acme.com"].age}
```

* Get row from KPS table with `User` alias.
* Use constant key `"kathy.adams@acme.com"` with quotation marks.
* Return `age` field of row.

##### Example 3

```
${kps.User[http.querystring.firstName][http.querystring.lastName].email}
```

* Get row from KPS table with `User` alias.
* Use key supplied in HTTP query string (`firstName` and `lastName`).
* Return `email` field of row.

### Auto-generated properties

In Policy Studio, you can select that `String` fields are **Auto-generated**. When a record is created, a `Java java.util.UUID` is assigned to the field if it is empty.

{{< alert title="Note" color="primary" >}}Values for auto-generated fields can be supplied and modified by users. KPS only generates a value at creation time if no value is already present.{{< /alert >}}

### Encrypted properties

In Policy Studio, you can select that `String` fields are **Encrypted** in storage. However, fields selected as **Indexed** (including primary and secondary key fields) cannot be encrypted. You can enter values for encrypted fields using the API Gateway Manager or the `kpsadmin` command. These values are forwarded to the API Gateway in the clear using the KPS REST service, and  encrypted before being written to storage.

{{< alert title="Note" color="primary" >}} The KPS REST service must always run over HTTPS (the default). You must set an encryption passphrase for the API Gateway group, because this is used in
the encryption process.{{< /alert >}}

When KPS tables are accessed using API Gateway selectors at runtime, encrypted fields are automatically decrypted. Selectors do not need to be aware that particular fields in a table are  encrypted in storage.

When KPS tables are read using the REST API, data is always returned in its encrypted state. Sometimes you may need to view decrypted data to help debug problems on an API Gateway. You can do this using debug mode in `kpsadmin`. This requires you to enter the passphrase for the API Gateway group.

If the in-built KPS encryption mechanism does not suit your needs, you can encrypt and decrypt data outside the KPS. In this case, you should not select properties in KPS tables as encrypted in Policy Studio. Encrypted data must be string-encoded for storage (for example, base64-encoded). Selectors that access the data must decrypt it themselves (for example, using a dedicated  decryption filter in Policy Studio).
