{
"title": "Use the KPS scripting API",
"linkTitle": "Use the scripting API",
"weight":"80",
"date": "2020-01-06",
"description": "The KPS scripting API facilitates create, read, update, delete (CRUD) operations to KPS tables from your policies."
}

You can use this API via a custom **Scripting Language** filter in Policy Studio.

## Prerequisites

This topic assumes that you are already familiar with using a KPS, and that you know how to define KPS tables and read data from KPS tables using selectors. For more information, see the following topics:

* [Configure KPS in Policy Studio](/docs/apim_policydev/apigw_kps/kps_configuration)
* [Access KPS data using selectors](/docs/apim_policydev/apigw_kps/kps_configuration#selector-access)

## Usage guidelines

When using Apache Cassandra as a KPS:

* It is recommended that you set up Cassandra as detailed at [Administer Apache Cassandra](/docs/cass_admin/).
* For a Cassandra-backed KPS, this API allows you to set a Time to Live (TTL) value for each record. The TTL specifies a time period after which a record expires and becomes unavailable from the table. If you set a TTL, you must specify all fields that are not auto-generated when creating or updating records.
* You should also set the Cassandra `gc_grace_seconds` value to a value larger than the TTL value. This ensures that data is purged sooner than the default (10 days).

When configuring KPS tables to use a local Ehcache:

* In this case the local Ehcache Time to Live (TTL) or Time to Idle (TTI) settings might conflict with the TTL parameters specified to KPS. You must test your use case carefully and you should never use an Ehcache TTL/TTI value that is bigger then the KPS table TTL value.
* When configuring the cache in Policy Studio, you must ensure that you do not select the **Eternal** option for a local cache, as this results in the specified TTL/TTI values being ignored.
* Since the caches are local to each API Gateway, you might have eventual inconsistencies between the cache and the KPS, for any record updates or deletes. The time during which eventual consistencies for a record happens is defined by the cache TTL.
* If you plan to mostly read from the KPS table, it is recommended that you use a local cache.

## Method descriptions

The API is defined in the java package `com.vordel.kps.Table`. It has the following methods.

### Create record in table

```
Map<String, Object> createRecord(String tableAlias, Map<String, Object> record, int ttl) throws ObjectExists, ObjectNotFound
```

Parameters
: `tableAlias` – alias of table to use.
: `record` – record to create, conforms to table definition in KPS (excluding auto-generated fields).
: `ttl` - record time to live, in seconds. If 0, TTL is ignored.

Returns
: Returns the newly created record (with auto-generated fields completed if necessary).

Throws
: `ObjectExists` – if a record with the same primary key already exists.

### Update record in table

```
Map<String, Object> updateRecord(String tableAlias, Map<String, Object> record, int ttl) throws ObjectExists, ObjectNotFound
```

Parameters
: `tableAlias` – alias of table to use.
: `record` – record to update, conforms to table definition in KPS (excluding auto-generated fields, primary key field is mandatory).
: `ttl` – record time to live, in seconds. If 0, TTL is ignored.

Returns
: Returns the updated record.

Throws
: `ObjectNotFound` – if no record with the given primary key exists.

### Read record from table

```
Map<String, Object> readRecord(String tableAlias, Object primaryKey) throws ObjectNotFound
```

Parameters
: `tableAlias` – alias of table to use.
: `primaryKey` – primary key of the record to read.

Returns
: Returns the record with given primary key.

Throws
: `ObjectNotFound` – if no record with the given primary key exists.

### Delete record from table

```
deleteRecord(String tableAlias, Object primaryKey) throws ObjectNotFound
```

Parameters
: `tableAlias` – alias of table to use.
: `primaryKey` – primary key of the record to delete.

Returns
: Nothing.

Throws
: `ObjectNotFound` – if no record with the given primary key exists.

## Example code

The following Jython code examples show how you can use the KPS scripting API in a **Scripting Language** filter. For more information on using this filter, see
[Scripting language filter](/docs/apim_policydev/apigw_polref/utility_additional/#scripting-language-filter).

### Create a new record

```java
from java.util import HashMap
from com.vordel.kps import Table, ObjectExists
def invoke(msg):
    record = HashMap()
    # If the primary key is auto-gererated then, the next line should be deleted
    record.put("Primary key Name", <Primary Key Value>)
    # Repeat next line for each of the non-auto-generated properties properties, note that if TTL is non-zero then, all non-auto-generated properties must be set
    record.put("Property Name", <Property Value>)
    try:
        record = Table.createRecord("table Alias", record, <Integer setting ttl in seconds>)
    except ObjectExists:
        #Handle case when an object with the given primary key already exists
```

### Update a record

```java
from java.util import HashMap
from com.vordel.kps import Table, ObjectNotFound
def invoke(msg):
    record = HashMap()
    record.put("Primary key Name", <Primary Key Value of record to update>)
    # Repeat next line for each of the properties to update, note that if TTL is non-zero then, all properties must be set
    record.put("Property Name", <Property Value>)
    try:
        record = Table.updateRecord("table Alias", record, <Integer setting ttl in seconds>)
        # Note that, the updated record TTL has been reset to value used in the last parameter
    except ObjectNotFound:
        # Handle case when an object with the given primary key does not exists
```

### Extend TTL for a record

```java
from java.util import HashMap
from com.vordel.kps import Table, ObjectNotFound
def invoke(msg):
    record = HashMap()
    record.put("Primary key Name", <Primary Key Value of record to update>)
    # Repeat next line for each of the properties to update, note that if TTL is non-zero then, all properties must be set
    record.put("Property Name", <Property Value>)
    try:
        record = Table.readRecord("table Alias", <Primary Key Value>)
        Table.updateRecord("table Alias", record, <Integer setting ttl in seconds>)
        # The record TTL is now extend to the new value
    except ObjectNotFound:
        # Handle case when an object with the given primary key does not exists
```

### Read a record

```java
from com.vordel.kps import Table, ObjectNotFound

def invoke(msg):

    try:
        record = Table.readRecord("table Alias", <Primary Key Value for record to read>)
    except ObjectNotFound:
        # Handle case when an object with the given primary key does not exists
```

### Delete a record

```java
from com.vordel.kps import Table, ObjectNotFound

def invoke(msg):

    try:
        record = Table.deleteRecord("table Alias", <<Primary Key Value for record to delete>)

    except ObjectNotFound:
        # Handle case when an object with the given primary key does not exists
```
