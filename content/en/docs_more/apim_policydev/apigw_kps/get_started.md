{
"title": "Get started with KPS",
"linkTitle": "Get started",
"weight":"30",
"date": "2020-01-06",
"description": "Develop an example KPS table for managing simple user information."
}

The final structure of the example KPS table in Policy Studio is as follows:

![Getting Started - User table in Policy Studio](/Images/APIGatewayKPSUserGuide/03000002.png)

This table structure is described as follows:

| Column    | Type              | Description                                                                                                                |
|-----------|-------------------|----------------------------------------------------------------------------------------------------------------------------|
| age       | java.lang.Integer | User age.                                                                                                                  |
| email     | java.lang.String  | User email address. This is selected in Policy Studio as a unique **Primary Key**, which is indexed implicitly by default. |
| firstName | java.lang.String  | User first name.                                                                                                           |
| lastName  | java.lang.String  | User surname.                                                                                                              |
| password  | java.lang.String  | User password, which is selected as **Encrypted** in Policy Studio.                                                        |

Example table data is displayed on the **Settings** > **Key Property Stores** tab in the API Gateway Manager web console:

![Getting Started - sample User data in KPS browser](/Images/APIGatewayKPSUserGuide/03000003.png)

## Before you begin

The following prerequisite steps apply to this example:

1. Ensure that an API Gateway and an Admin Node Manager are running.
2. Start Policy Studio, and connect to the Admin Node Manager.

## Define KPS configuration with Policy Studio

The main steps for configuring KPS tables in Policy Studio are as follows:

### Step 1: Define where data will be stored

You must first create a KPS collection in which to store the KPS table configuration. Perform the following steps:

1. In the Policy Studio tree, select **Environment Configuration** > **Key Property Stores**, then select **Add KPS Collection**.

    ![KPS collection with the default Embedded (Cassandra) data source](/Images/APIGatewayKPSUserGuide/03000004.png)

2. In the **Add KPS Collection** dialog, enter a name to the collection, for example **Samples**, and select **Cassandra** from the **Default data source** list.

    Leave the **Alias prefix** field blank.

### Step 2: Define the KPS table

To create a table, perform the following steps:

1. In the Policy Studio tree, right-click the newly-created **Samples** collection, and select **Add Table**:

    ![Add Table menu option](/Images/APIGatewayKPSUserGuide/03000005.png)

2. In the dialog, enter a **Name** (for example, `User`) and provide a **Description**.
3. Click **Add** to assign an alias of **User** to this table. A table must have at least one alias.
4. Next define the table structure. This consists of the table columns and the data type stored in each column. Select the **User** table and **Structure** tab, and click **Add**:

    ![KPS table configuration - specify table structure](/Images/APIGatewayKPSUserGuide/03000007.png)

5. Repeat to add the following columns for your table structure in the **Add Property** dialog:
    * email
    * password
    * firstName
    * lastName
    * age

    `age` has an Integer (numeric) **Type**. All the other columns are String. For more details, see [KPS table structure](/docs/apim_policydev/apigw_kps/kps_configuration/#kps-table-structure).

6. When you select the **User** table, you should have the following structure:

    ![Column structure of the User KPS table](/Images/APIGatewayKPSUserGuide/0300000A.png)

7. You want the **email** field to be the primary key for the table, so select **Primary Key** for this field.

    ![Set the email column to be a primary key](/Images/APIGatewayKPSUserGuide/0300000B.png)

8. You want the **password** field to be encrypted when stored in the data source, so select **Encrypted** for this field.

    ![Set the password column to be encrypted](/Images/APIGatewayKPSUserGuide/0300000C.png)

### Step 3: Define a policy that accesses the table

To define a test policy that accesses the table, perform the following steps:

1. Add a test policy with a **Set Message** filter from the **Conversion** filter category.

    ![Test policy to access the table](/Images/APIGatewayKPSUserGuide/0300000E.png)

2. Right-click the filter and set it as the **Start** filter for the policy.
3. Enter a filter **Content-Type** of text/plain.
4. Enter the following **Message Body** for use in the policy:

    ```
    ========================
    User
    ===
    Email: ${kps.User[http.querystring.id].email}
    First Name: ${kps.User[http.querystring.id].firstName}
    Last Name: ${kps.User[http.querystring.id].lastName}
    Age: ${kps.User[http.querystring.id].age}
    ========================
    ```

    {{< alert title="Note" color="primary" >}}Add a **Reflect Message** filter (**Conversion** category) to return a successful HTTP response status of `200`.{{< /alert >}}

5. Set up a path to this policy. In this example, the path is `/kpsGetViaSelector`:

![Create a path to the test policy](/Images/APIGatewayKPSUserGuide/03000010.png)

### Step 4: Deploy the configuration

When you are finished with your configuration changes, you must deploy them to the API Gateway. To deploy the new configuration, click **Deploy** in the Policy Studio toolbar:

![Deploy the configuration](/Images/APIGatewayKPSUserGuide/03000011.png)

This pushes the configuration to the API Gateway group.

{{< alert title="Tip" color="primary" >}}If you deploy an incorrect configuration (for example, specify an incorrect primary key, property type, or name) you can use the `kpsadmin` command to drop the table in storage. For more details, see [Manage KPS using the kpsadmin tool](/docs/apim_policydev/apigw_kps/how_to_use_kpsadmin_command/).{{< /alert >}}

## Add KPS data using API Gateway Manager

You can use API Gateway Manager to populate the User table with data.

For example, perform the following steps:

1. To access API Gateway Manager in your browser, go to `https://localhost:8090`.
2. Select the **Settings** > **Key Property Stores** > **Samples** > **User** table.
3. To enter new records, select **Actions** > **New Entry** on the right.
4. Click **Save** to save a record.

For example, the table should look as follows:

![KPS browser showing sample record](/Images/APIGatewayKPSUserGuide/03000003.png)

## Access KPS data from a policy

To access KPS data from a policy, go to the following URL in your browser:

```
http://localhost:8080/kpsGetViaSelector?id=patrica.allen@acme.com
```

This URL specifies the user ID (email) as patrica.allen@acme.com

You must specify an email that exists in your data. For example:

![Sample policy execution](/Images/APIGatewayKPSUserGuide/03000014.png)

{{< alert title="Note" color="primary" >}}If you enter an email that does not exist, you will see `[invalid field]` results. For example:{{< /alert >}}

![Sample policy execution - unknown primary key (email)](/Images/APIGatewayKPSUserGuide/03000015.png)

## Enable API Gateway tracing

To enable API Gateway debug tracing, perform the following steps in Policy Studio:

1. In the tree on the left, select **Environment Configuration** > **Server Settings** > **General**.
2. Select a **Tracing level** of **DEBUG**.
3. Click **Save**.
4. Click **Deploy**.

![Enable debug tracing](/Images/APIGatewayKPSUserGuide/03000016.png)

{{< alert title="Note" color="primary" >}}This setting enables debug tracing for the entire API Gateway, and not just for the KPS.{{< /alert >}}
