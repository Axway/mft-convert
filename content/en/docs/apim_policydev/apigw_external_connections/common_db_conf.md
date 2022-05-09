{
"title": "Configure database connections",
  "linkTitle": "Configure database connections",
  "weight": 3,
  "date": "2019-10-17",
  "description": "Configure database connections and database queries."
}

## Configure database connections

The details entered on the **Configure Database Connection** dialog specify how API Gateway connects to the database. The API Gateway maintains a JDBC pool of database connections to avoid the overhead of setting up and tearing down connections to service simultaneous requests. This pool is implemented using *Apache Commons DBCP (Database Connection Pools)*.

The settings in the **Advanced - Connection pool** section of this window configures the database connection pool. For details on how the fields on this window correspond to specific DBCP configuration settings, see the table in [Database connection pool settings](#database-connection-pool-settings).

### Prerequisites

Before configuring a database connection, you must add the JDBC driver files for your chosen database to your API Gateway and Policy Studio installations. The following sections show how to add the third-party JDBC driver files for your database to API Gateway and Policy Studio.

#### Add third-party binaries to API Gateway

To add third-party binaries to API Gateway, perform the following steps:

1. Add the binary files as follows:
    * Add `.jar` files to the `INSTALL_DIR/apigateway/ext/lib` directory.
    * Add `.so` files to the `INSTALL_DIR/apigateway/<platform>/lib` directory.
2. Restart API Gateway.

#### Add third-party binaries to Policy Studio

To add third-party binaries to Policy Studio, perform the following steps:

1. Select **Window > Preferences > Runtime Dependencies** in the Policy Studio main menu.
2. Click **Add** to select a JAR file to add to the list of dependencies.
3. Click **Apply** when finished. A copy of the JAR file is added to the `plugins` directory in your Policy Studio installation.
4. Click **OK**.
5. Restart Policy Studio with the `-clean` option. For example:

    ```
    cd INSTALL_DIR/policystudio/
    policystudio -clean
    ```

### Configure the database connection

Configure the following fields on the **Configure Database Connection** window:

**Name**: Enter a name for the database connection in the **Name** field.

**URL**: Enter the fully qualified URL of the location of the database. The following table shows examples of database connection URLs, where `reports` is the name of the database and `DB_HOST` is the IP address or host name of the machine on which the database is running:

| Database                 | Example Connection URL                                                         |
|--------------------------|--------------------------------------------------------------------------------|
| **Oracle**               | `jdbc:oracle:thin:@DB_HOST:1521:reports`                                       |
| **Microsoft SQL Server** | `jdbc:sqlserver://DB_HOST:1433;DatabaseName=reports;integratedSecurity=false;` |
| **MySQL**                | `jdbc:mysql://DB_HOST:3306/reports`                                            |
| **MariaDB**              | `jdbc:mariadb://DB_HOST:3306/reports`                                          |
| **IBM DB2**              | `jdbc:db2://DB_HOST:50000/reports`                                             |

**User Name**: The user name to use to access the database.

**Password**: The password for the user specified in the **User Name** field.

**Initial pool size**: The initial size of the DBCP pool when it is first created.

**Maximum number of active connections**: The maximum number of active connections that can be allocated from the JDBC pool at the same time. The default maximum is 8 active connections.

**Maximum number of idle connections**: The maximum number of active connections that can remain idle in the pool without extra connections being released. The default maximum is 8 connections.

**Minimum number of idle connections**: The minimum number of active connections that can remain idle in the pool without extra connections being created. The default minimum is 8 connections.

**Maximum wait time**: The maximum number of milliseconds that the pool waits (when there are no available connections) for a connection to be returned before throwing an exception, or -1 to wait indefinitely. The default time is 10000ms, and a value of -1 indicates an indefinite time to wait.

**Time between eviction**: The number of milliseconds to sleep between runs of the thread that evicts unused connections from the JDBC pool.

**Number of tests**: The number of connection objects to examine from the pool during each run of the evictor thread. The default number of objects is 3.

**Minimum idle time**: The minimum amount of time, in milliseconds, an object may sit idle in the pool before it is eligible for eviction by the idle object evictor (if any).

### Database connection pool settings

The table below shows the correspondence between the fields in the **Advanced - Connection pool** section of the window and the Apache Commons DBCP configuration properties:

| Field Name                           | DBCP Configuration Property     |
|--------------------------------------|---------------------------------|
| URL                                  | `url`                           |
| User Name                            | `username`                      |
| Password                             | `password`                      |
| Initial pool size                    | `initialSize`                   |
| Maximum number of active connections | `maxActive`                     |
| Maximum number of idle connections   | `maxIdle`                       |
| Minimum number of idle connections   | `minIdle`                       |
| Maximum wait time                    | `maxWait`                       |
| Time between eviction                | `timeBetweenEvictionRunsMillis` |
| Number of tests                      | `numTestsPerEvictionRun`        |
| Minimum idle time                    | `minEvictableIdleTimeMillis`    |

### Connection validation

By default, when the API Gateway makes a connection, it performs a simple connection validation query. This enables the API Gateway to test the database connection before use, and to recover if the database goes down (for example, if there is a network failure, or if the database server reboots).

The API Gateway validates connections by running a simple SQL query (for example, a `SELECT 1` query with MySQL or MariaDB). If it detects a broken connection, it creates a new connection to replace it.

### Test the connection

Ensure that Policy Studio can make a network connection to the database server, and click the **Test Connection** button to verify that the connection to the database is configured successfully. This enables you to detect any configuration errors at design time instead at runtime.

## Configure database queries

The **Database Statement** dialog enables you to enter an SQL query, stored procedure, or function call that the API Gateway runs to return a specific user's profile from a database.

### Configuration

The following fields should be completed on this window:

**Name**: Enter a name for this database query here.

**Database Query**: Enter the actual SQL query, stored procedure, or function call in the text area provided. When executed, the query should return a single user's profile. The following are examples of SQL statements and stored procedures:

```
select * from users where username=${authentication.subject.id}
{ call load_user (${authentication.subject.id}, ${out.param}) }
{ call ${out.param.cursor} := p_test.f_load_user(${authentication.subject.id}) }
```

These examples show that you can use selectors in the query. The selector that is most commonly used in this context is `${authentication.subject.id}`, which specifies the message attribute that holds the identity of the authenticated user.

**Statement Type**: The database can take the form of an SQL query, stored procedure, or function call, as shown in the above examples. Select the appropriate radio button depending on whether the database query is an SQL **Query** or a **Stored procedure/function call**.

When using a stored procedure or function call:

* The [**Retrieve from or write to database**](/docs/apim_policydev/apigw_polref/attributes_retrieve/#retrieve-attribute-from-database-filter) filter does not support complex stored procedures or function calls. A complex stored procedure or function call performs multiple tasks such as updates, inserts, and deletes. These types of statements might also return multiple result sets and output parameters. The filter only handles the first result set, and if a stored procedure or function call returns multiple result sets the filter ignores it.
* You must handle any exception and throw it to the JDBC layer to ensure that the **Retrieve from or write to database** filter will fail. Otherwise the filter ignores the exception.

**Expect to retrieve**: If this value is greater than 0 and the SQL statement returns a result set, the result set is cached for a period of time controlled by the server setting [general parameter](/docs/apim_reference/general_settings) `Cache refresh interval`. If the value is 0, the result set is not cached. The value is not currently used as a constraint on the number of rows returned in the result set.

**Table Structure**:
To process the result set that is returned by the database query, the API Gateway needs to know whether the user's attributes are structured as rows or columns in the database table.

The following example of a database table shows the user's attributes (Role, Dept, and Email) structured as table columns:

| Username | Role          | Dept        | Email          |
|----------|---------------|-------------|----------------|
| Admin    | Administrator | Engineering | admin@org.com  |
| Tester   | Testing       | QA          | tester@org.com |
| Dev      | Developer     | Engineering | dev@org.com    |

In the following table, the user's attributes have been structured as name-value pairs in table rows:

| Username | Attribute Name | Attribute Value |
|----------|----------------|-----------------|
| Admin    | Role           | Administrator   |
| Admin    | Dept           | Engineering     |
| Admin    | Email          | admin@org.com   |
| Tester   | Role           | Testing         |
| Tester   | Dept           | QA              |
| Tester   | Email          | tester@org.com  |
| Dev      | Role           | Developer       |
| Dev      | Dept           | Engineering     |
| Dev      | Email          | dev@org.com     |

If the user's attributes are structured as column names in the database table, select the **attributes as column names** radio button. If the attributes are structured as name-value pair in table rows,  select the **attribute name-value pairs in rows** option.
