{
"title": "Environmentalization example",
"linkTitle": "Environmentalization example",
"weight":"30",
"date": "2020-09-25",
"description": "Detailed example that illustrates different aspects of the environmentalization capability supported by the YAML configuration."
}

This example illustrates the following aspects of the environmentalization:

* Injection of integer, long, and string types.
* Partial strings (URL field).
* System environment variable with and without default value.
* Encrypted passwords.
* Conservation of other environmentalization methods.
* An unresolved placeholder.

In the following example snippet, we assume that `ES_DB_PASSWORD` is set with a value encrypted by a passphrase, and `ES_DB_HOST` is not set.

```yaml
# A YAML Entity
---
type: DbConnection
fields:
   name: MySQL
   maxIdle: '{{db.maxIdle}}'
   maxWait: '{{foo}}'
   url: jdbc:mysql://{{db.host}}:3306/DefaultDb
   password: '{{db.password}}'
   username: '{{db.username}}'
   maxActive: ${env.dbMaxActive}
   timezoneAware: ${environment.IS_TIMEZONE_AWARE}
```

```yaml
# value.yaml
db:
  maxIdle: 1
  username: scott
  password: '{{env "ES_DB_PASSWORD"}}'
  host: '{{env "ES_DB_HOST" "staging.db.acme.com"}}'
```

```yaml
# Processed result (in-memory at load time)
---
type: DbConnection
fields:
   name: MySQL
   maxIdle: 1
   maxIdle: '{{foo}}'                    # <--- error
   url: jdbc:mysql://staging.db.acme.com:3306/DefaultDb
   password: Zm9pdWRiZm9pdWRiZm9pdXNkbmaXM=
   username: scott
   maxActive: ${env.dbMaxActive}
   timezoneAware: ${environment.IS_TIMEZONE_AWARE}
```
