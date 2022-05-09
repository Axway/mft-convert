{
"title": "KPS FAQ",
"linkTitle": "FAQ",
"weight":"90",
"date": "2020-01-06",
"description": "Frequently asked questions on KPS and API Gateway, KPS storage, and Apache Cassandra."
}

## KPS and API Gateway

This section includes frequently asked questions on KPS and API Gateway.

### What is KPS used for in API Gateway?

In addition to use with your API Gateway policies, KPS is used as an option for OAuth token storage and Client Application Registry. KPS storage in Apache Cassandra is required for API Manager and API Portal.

### What is KPS not suitable for?

KPS is not suitable for complicated data models, ad hoc queries, or where full ACID transaction support is required. KPS does not enforce referential integrity.

### What are the transaction semantics of KPS?

Individual KPS operations are atomic (A), isolated (I), and durable (D). Consistency (C) depends on the data storage mechanism chosen and the number of API Gateways in a group.

With file storage, data is consistent in a single API Gateway. With supported database storage, data is consistent. Cassandra storage allows consistency levels to be set per KPS table. KPS does not provide transactions across multiple operations. You cannot issue a set of KPS operations and roll them back.

### What is the KPS collection alias prefix for?

This provides an optional namespace for a KPS collection to help ensure that tables in the collection have a unique alias. In most cases, you can leave this prefix empty.

### How do I change the API Gateway group passphrase?

To change the group passphrase, perform the following steps:

1. Change the group passphrase in Policy Studio. For details, see the API Gateway *Administrator Guide*.
2. In kpsadmin, select the Collection Administration, Re-encrypt All option to re-encrypt the data in each collection.
3. You will be asked to enter the old API Gateway passphrase. This passphrase is used to decrypt the data. The data is then re-encrypted with the current API Gateway passphrase.
4. Restart the API Gateway instance(s) in the group.

For more details, see [How to use the kpsadmin command](/docs/apim_policydev/apigw_kps/how_to_use_kpsadmin_command).

## KPS storage options

This section includes frequently asked questions on KPS storage mechanisms.

### Is Apache Cassandra storage required? Can you use file or database storage?

Apache Cassandra is the default out-of-the-box storage, but is not a requirement for API Gateway. You can switch to database or file storage.

{{< alert title="Note" color="primary" >}}KPS data defined in Policy Studio supports Cassandra, database, and file data stores. API Manager KPS data (Client Registry and API Catalog) supports Cassandra only.

File-based KPS storage is deprecated and will be removed in a future release. {{< /alert >}}

### How do you switch storage for a KPS collection?

The general steps to switch storage for a KPS collection are as follows:

1. Back up the data for a collection using kpsadmin.
2. Add and configure a new data source (file, database, or Cassandra) using Policy Studio.
3. Deploy the configuration in Policy Studio.
4. Restore the data using kpsadmin.

For more details, see the example in [How to use the kpsadmin command](/docs/apim_policydev/apigw_kps/how_to_use_kpsadmin_command/).

### Why use database storage?

You may already have a relational database system and expertise that you wish to use. For more details, see [Configure database KPS storage](/docs/apim_policydev/apigw_kps/configure_database_storage).

### Why use file storage?

{{< alert title="Note" color="primary" >}}File-based KPS storage is deprecated and will be removed in a future release.{{< /alert >}}

File storage is very easy to use. Data is written to very simple JSON encoded files. File storage is suitable for single API Gateway use in development and production. If you have more than one API Gateway sharing the files, you must restart these API Gateways after a KPS update. It is only suitable here for rarely changing, read-mostly data. For more details, see [Configure file-based KPS storage](/docs/apim_policydev/apigw_kps/configure_database_storage#configure-file-based-kps-storage).

### When can you use kpsadmin? When should you use storage-specific tools?

You can use kpsadmin anytime. However, it is especially useful for development use. In production, you should also use storage-specific tools and procedures (for example, for data backup). For more details on kpsadmin, see [How to use the kpsadmin command](/docs/apim_policydev/apigw_kps/how_to_use_kpsadmin_command/).

## Apache Cassandra

This section includes frequently asked questions on the Apache Cassandra database:

### Why use Cassandra as a KPS storage option?

Cassandra is a key-value store. Cassandra has a non-restrictive Apache 2.0 license. It provides high availability, and has an active community.

### What versions of Cassandra are supported by API Gateway?

For details on supported Cassandra versions, see [Install an Apache Cassandra database](/docs/apim_installation/apigtw_install/cassandra_install/).

### What does all host polls marked down mean?

This means that the client cannot connect to the Cassandra server. For details on how to resolve this issue, see [Troubleshooting the KPS](/docs/apim_policydev/apigw_kps/troubleshooting).

## KPS REST API

 The KPS REST API is documented in the API Gateway REST API documentation, available from the [Product APIs on the Axway Documentation portal](https://docs.axway.com/category/api), and also included in your API Gateway installation at `INSTALL_DIR/apigateway/samples/swagger`.
