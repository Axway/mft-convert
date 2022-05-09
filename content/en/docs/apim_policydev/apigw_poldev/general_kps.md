{
"title": "Configure Key Property Store data",
"linkTitle": "Configure KPS data",
"weight": 10,
"date": "2019-10-17",
"description": "Use a Key Property Store to store data that can be referenced by policies running on an API Gateway."
}

A Key Property Store (KPS) is a table of data referenced by policies running on an API Gateway. Data in a KPS table is assumed to be read frequently and seldom written, and can be changed without incurring an API Gateway service outage. KPS tables are shared across an API Gateway group.

A KPS is typically used to store property values used in policies on a gateway. KPS data is injected into policies using selectors created in Policy Studio. Selectors are evaluated and expanded dynamically at runtime. For example, a KPS table contains authorization tokens for different users. A policy looks up the token for the current user, and inserts it into an HTTP request.

{{< alert title="Caution" color="warning" >}}Do not edit the default KPS tables in Policy Studio unless under strict supervision from Axway Support. This includes the **API Server**, **OAuth**, or **API Portal** KPS tables available under **Environment Configuration > Key Property Stores**.{{< /alert >}}

For more details on Key Property Stores, see the [API Gateway Key Property Store User Guide](/docs/apim_policydev/apigw_kps/).

## KPS tables and collections

KPS tables are organized into *collections*. The tables in a collection typically have a relationship to each other. For example, the OAuth collection contains a set of tables that store all OAuth-related data. Every KPS table is assigned an *alias* so that it can be easily referred to in a policy or a REST request.

| **Column**  | **Type**  | **Description**                                                                  |
|-------------|-----------|----------------------------------------------------------------------------------|
| `email`     | `String`  | User email address. This is the primary key used to identify a row in the table. |
| `password`  | `String`  | User password. This confidential data is encrypted.                              |
| `firstName` | `String`  | User first name.                                                                 |
| `lastName`  | `String`  | User surname.                                                                    |
| `age`       | `Integer` | User age.                                                                        |

## Enter data in a KPS table

You can enter data in a KPS table using the API Gateway Manager web console for viewing and modifying data. This is available in API Gateway Manager under **Settings > Key Property Stores**. The `kpsadmin` command-line tool also supports data entry in addition to other administrative functions. KPS data can also be read and written by remote programmatic clients using the KPS REST interface.

New values for encrypted fields are always transmitted to the server in the clear. For security, always use HTTPS when accessing KPS over its REST API (this is the default).

The following example shows some simple table data that has been entered in API Gateway Manager, and which follows the example structure:

| **`email`**        | **`password`** | **`firstName`** | **`lastName`** | **`age`** |
|--------------------|----------------|-----------------|----------------|-----------|
| `jdoe@acme.com`    | `*****`        | `John`          | `Doe`          | `21`      |
| `jbloggs@acme.com` | `*****`        | `Joe`           | `Bloggs`       | `42`      |
| `jdupont@acme.com` | `*****`        | `Jean`          | `Dupont`       | `33`      |

In this example, the `email` column is the primary key. You can use this to look up and uniquely identify a row using a selector expression. For example, the following selector expression evaluates to `John`:

```
${kps.customers["jdoe@acme.com"].firstName}
```

The following selector expression evaluates to `42`:

```
${kps.customers["jbloggs@acme.com"].age}

```

## KPS data sources

A KPS provides a consistent interface to data that can be stored in different data sources. API Gateway supports the following KPS data sources:

**External Apache Cassandra database (default)**: Used across an API Gateway group to provide high availability in a production environment.

**Relational Database**: Enables you to use your existing database (for example, Oracle, Microsoft SQL Server, IBM DB2, or MySQL). The following approaches to data storage are supported:

* Shared storage—data for multiple KPS tables is stored in a single dedicated database table. This is the recommended approach.
* Per-table storage—each KPS table is backed by a single database table.

**JSON files on the local file system**: Suited to single API Gateway deployments. In a multi-API Gateway scenario, file replication or a shared disk is required to ensure all API Gateways use the same data.

{{< alert title="Note" color="primary" >}}If a file-based KPS table is shared across API Gateways, the API Gateways must be restarted after data has changed. File-based KPS is deprecated in this release.{{< /alert >}}

## Add a KPS collection

A KPS collection is a group of KPS tables. To add a KPS collection, perform the following steps:
The newly created KPS collection is displayed on the window on the right.

1. In the Policy Studio tree, right-click **Environment Configuration > Key Property Stores**, and select **Add KPS Collection**.
2. Complete the following fields in the **Add KPS Collection** dialog:
    * Name: Enter the KPS name (for example, CustomerCollection).
    * Description: Enter a text description of your KPS collection.
    * Alias prefix: Leave this field blank.
    * Default data source: Select one of the following from the list:
        * File
        * Cassandra
        * SQL Database

        Defaults to **Cassandra**.

## Edit a KPS collection

To edit a KPS collection, perform the following steps:

1. In the main Policy Studio tree, select the KPS collection to edit under **Environment Configuration > Key Property Stores**.
2. Click the **Properties** tab.
3. You can edit the **Name**, **Description**, or **Alias prefix** for the KPS collection as required.
4. To change the **Default data source**, click the browse button to display a tree of data sources, and select a new default data source from the tree.
5. You can also specify a cache for storage and retrieval of selector results. This improves selector read performance for storage backends such as databases. In the **Cache** field, click the browse button to display a tree of caches, and select a cache. Only local caches are supported.
6. You can add, edit, or delete KPS collection data sources on the **Data Sources** tab. For more information, see the following sections.
7. Click **Save** at the top right to save your changes.

### Add a Cassandra data store

To add an Cassandra database data store to a selected KPS collection, perform the following steps:

1. On the **Data Sources** tab, select **Add > Add Cassandra**
    at the bottom right.
2. Complete the following fields in the **Add Cassandra Data Source** dialog:
    * Name: Enter the KPS name (for example, Customer Cassandra Data Source).
    * Description: Enter a text description of your SQL database data source.
    * Read Consistency Level: Select the consistency level for Cassandra read operations from the list. Defaults to ONE.
    * Write Consistency Level: Select the consistency level for Cassandra write operations from the list. Defaults to ONE.

    For more details, see [How is the consistency level configured?](http://docs.datastax.com/en/archived/cassandra/2.2/cassandra/dml/dmlConfigConsistency.html)
3. Click **OK**.

### Add a database data store

To add an SQL database data store to a selected KPS collection, perform the following steps:

1. On the **Data Sources** tab, select **Add > Add Database**
    at the bottom right.
2. Complete the following fields in the **Add File Data Source** dialog:
    * **Name**: Enter the KPS name (for example, `Customer DB Data Source`).
    * **Description**: Enter a text description of your SQL database data source.
    * **Database Connection**: Click the button on the right, select a database connection in the dialog (for example, `Default Database Connection`), and click **OK**.
    * You can add more database connections to the list by right-clicking **Environment Configuration > External Connections > Database Connections** in the Policy Studio tree, and selecting **Add a Database Connection**.
3. Click **OK**.

### Add a file data store

To add a file-based data store to a selected KPS collection, perform the following steps:

1. On the **Data Sources** tab, select **Add > Add File** at the bottom right.
2. Complete the following fields in the **Add File Data Source** dialog:
    * **Name**: Enter the KPS name (for example, `Customer File Data Source`).
    * **Description**: Enter a text description of your file data source.
    * **Directory Path**: Enter the full directory path (for example, `c:\kpsdata`). Each table in the collection has its own JSON file in this directory.
3. Click **OK**.

## Add a KPS table

To add a KPS table to a KPS collection, perform the following steps:

1. In the main Policy Studio tree, right-click a KPS collection (for example **CustomerCollection**), and select **Add Table**.
2. Complete the following fields in the **Add KPS Table** dialog:
    * **Name**: Enter the KPS name (for example, `Customers`).
    * **Description**: Enter a text description of your KPS.
    * **Aliases**: Click **Add**, and enter an alias used to identify your KPS (for example, `customers`), and click **OK**. Every KPS must have at least one alias.
3. Click **OK**.

## Define the KPS table structure

To define the KPS table structure, perform the following steps:

1. In the main Policy Studio tree, select a KPS table (for example **Customers**), and click the **Structure** tab.
2. Click **Add**, and complete the following fields in the **Add Property** dialog:
    * **Name**: Enter the name of the table column (for example, `email`).
    * **Type**: Select the data type from the list (for example, `java.lang.String`).
    * **Key**: For `java.util.Map` types, select the key type from the list (for example, `java.lang.Integer`).
    * **Value**: For `java.util.Map` and `java.util.List` types, select the value type from the list (for example, `java.lang.Boolean`).
3. Click **OK**. The newly created KPS table column is added to the **Structure** tab.
4. Select **Primary Key** to make a field the primary key for the table.
5. Select **Autogenerated** to auto-generate a field in the KPS data source.
6. Select **Encrypted** to encrypt a field in the KPS data source.
7. Select **Indexed** to index a field in the KPS data source.
8. Repeat the preceding steps to add more table columns.
9. At the bottom, enter names of one or more properties used to look up this table from a selector (for example, `firstName,surname`). If none are specified, selectors can access the table using the primary key.
10. Click **Save** at the top right to save your changes.
