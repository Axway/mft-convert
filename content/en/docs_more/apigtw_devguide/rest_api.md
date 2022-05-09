{
"title": "API Gateway REST APIs",
"linkTitle": "API Gateway REST APIs",
"weight":"100",
"date": "2019-11-27",
"description": "API Gateway exposes services as REST APIs, which provide access and basic CRUD operations for the service resources."
}

API Gateway exposes some services as REST APIs. These APIs provide access and basic create, read, update, and delete (CRUD) operations for the service resources. API Gateway contains a [Jersey Servlet](http://jersey.java.net/) that scans a predefined list of packages to identify RESTful resources to be exposed over HTTP or HTTPS.

Jersey REST services are exposed on the internal management HTTPS listener that is running on every API Gateway. This HTTPS listener is not accessible to the outside world and only accepts traffic over two-way SSL from the local Node Manager. Therefore, to call any REST service exposed on the management interface, you must call it via the Admin Node Manager using the Routing API.

The API Gateway REST APIs are available from the following locations:

* `INSTALL_DIR/apigateway/samples/swagger`
* [Product APIs](https://docs.axway.com/category/api) page on the Axway Documentation portal

{{< alert title="Note" color="primary" >}}When viewing REST APIs on the Axway Documentation portal, the `consumes` field is not displayed if you are using `formData` type parameters in an API, due to limitations in the Swagger UI version.{{< /alert >}}

## API Gateway component REST APIs

The following table summarizes the API Gateway component REST APIs that are available:

| API                | Description                                                                                                                                                                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Router             | The Router REST API is available in the Node Manager. It acts as a relay that forwards requests to the appropriate API Gateway registered with the Node Manager.                                                                                                                                                                                              |
| Management         | The Management REST API is available in the Node Manager and all API Gateways. It provides the ability to retrieve the following API Gateway information: API Gateway name, group name, service type, product version, and the domain ID assigned to the Admin Node Manager on creation. This API can also be used to update the service name and group name. |
| Deployment         | The Deployment REST API is available in the Node Manager. It provides the ability to manage deployment archives for API Gateways.                                                                                                                                                                                                                             |
| Configuration      | The Configuration REST API is available in the API Gateway. It provides the ability to upload configurations to API Gateway Admin Node Manager instances. It is used in conjunction with the Deployment API.                                                                                                                                                  |
| API Manager        | The API Manager REST API is available in the API Manager. It provides the ability to view and update the configured users, organizations, applications and events related to the API Manager.                                                                                                                                                                 |
| Admin Users        | The Admin Users REST API is available in the Node Manager. It provides the ability to manage administrator users and roles for the API Gateway installation.                                                                                                                                                                                                  |
| Topology           | The Topology REST API is available in the Node Manager. It provides the ability to manage hosts, groups, and services in the topology.                                                                                                                                                                                                                        |
| Traffic Monitor    | The Traffic Monitor REST API is available in the API Gateway. It provides the ability to monitor traffic in and out of the API Gateway.                                                                                                                                                                                                                       |
| Service Manager    | The Service Manager REST API is available in the Node Manager. It provides the ability to manage virtualized REST APIs configured in the topology.                                                                                                                                                                                                            |
| Analytics          | The Analytics REST API is available in API Gateway Analytics. It provides read-only access to the database audit log and audit message/payload details, metrics for charting, and CRUD for custom reporting.                                                                                                                                                  |
| RBAC               | The RBAC (Role Based Access Control) REST API is available in the Node Manager. It ensures that only users with the assigned role can access parts of the management services exposed by the Admin Node Manager.                                                                                                                                              |
| Monitoring         | The Monitoring REST API is available in the API Gateway. It provides access to process summary details and listings of the real-time metrics for items that metrics are recorded for (for example, web services, external APIs, authenticated clients, external target servers, and so on).                                                                   |
| KPS                | The KPS REST API is exposed by the API Gateway and the Node Manager. The API Gateway interface provides a persistence mechanism. The Node Manager service provides administration functions.                                                                                                                                                                  |
| Domain Audit       | The Domain Audit REST API is available in the Node Manager and API Gateway. It provides the ability to read domain audit events recorded by the Node Manager and API Gateway.                                                                                                                                                                                 |
| Embedded Active MQ | The Embedded Active MQ REST API is exposed by the API Gateway.                                                                                                                                                                                                                                                                                                |

## Import the API Gateway REST API into API Manager

You can import the API Gateway REST API Swagger 2.0 definitions into API Manager in the same way that you import any other APIs. For example:

1. Click the **API Registration** > **Backend API** view in API Manager.
2. Click **New API** and select **Import Swagger API**.
3. In the **Import API** dialog, complete the following:
    * **Source**: Select Swagger definition file.
    * **File** or **URL**: Click the browse button to select the definition file. For example: `INSTALL_DIR/apigateway/samples/swagger/api-gateway-swagger.json`
    * **API Name**: Enter a user-friendly name for the API. The default is **api-gateway-swagger.json**.
    * **Organization**: Select the organization from the list (for example, **API Development**).
4. Click **Import** to import the API Gateway API.

![API Gateway REST API Swagger 2.0 Definition](/Images/APIGatewayDeveloperGuide/api_gateway_rest_api.png)

## Add a Jersey-based REST API

{{< alert title="Note" color="primary" >}}The following sections refer to `restJabber` sample code that is no longer included in the code samples supplied with API Gateway. We recommend that you use this section only as a general guide for adding a Jersey-based REST API.{{< /alert >}}

The following example shows how to add a Jersey REST service to your API Gateway and configure a corresponding servlet in Policy Studio. The REST service implements the SMACK API. The example also demonstrates how invoking a REST request sends an instant message to an account on Google Talk.

1. Annotate your Java class. The following example shows a code snippet of a Jersey-annotated Java class for the Smack API. The full class definition can be found in the `DEVELOPER_SAMPLES/restJabber` directory. You must replace the `username` and `password` in the sample code with appropriate values.

    ```java
    @Path("/jabber")
    public class RestJabberRequest {

        private static final String username = "yourEmailaddresst@here.com";
        private static final String password = "yourPassword";
        private static final String resource = "apiServer";
        XMPPConnection connection;

        // This method is called if TEXT_PLAIN is request
        @GET
        @Produces(MediaType.TEXT_PLAIN)
        @Path("{msg}/{to}")
        public String sendPlainMessage(@PathParam("msg") String msg,
                @PathParam("to") String to) {
            try {
                sendMessage(msg, to);
            } catch (XMPPException e) {
                System.err.println("Sending message failed:");
                e.printStackTrace();
            }
            return "Sent a message of : " + msg + " to " + to;
        }

        …

        private void sendMessage(String msg, String to) throws XMPPException {
            try {
                ConnectionConfiguration config =
                  new ConnectionConfiguration("talk.google.com", 5222, "gmail.com");
                connection = new XMPPConnection(config);
                SASLAuthentication.supportSASLMechanism("PLAIN", 0);
                connection.connect();
                connection.login(username, password, resource);
                Chat chat =
                  connection.getChatManager().createChat(to, new MessageListener(){
                    @Override
                    public void processMessage(Chat arg0, Message arg1) {
                        Trace.debug(arg1.getBody());
                    }
                });
                chat.sendMessage(msg);
                connection.disconnect();
            } catch (org.jivesoftware.smack.XMPPException ex) {
                System.out.println("Exception throw");
            }
        }

    }
    ```

2. Follow the instructions in the `README.TXT` in the sample directory to build the JAR file for the restJabber sample.
3. Add the new JAR and any third-party JAR files used by the restJabber classes (for example, the SMACK API JAR files) to the CLASSPATH for all API Gateways and Node Managers on a host by copying them to the `INSTALL_DIR/apigateway/ext/lib` directory.
4. Alternatively, you can add the JARs to the CLASSPATH for a single API Gateway instance only, by copying them to the `INSTALL_DIR/apigateway/groups/GROUP_ID/INSTANCE_ID/ext/lib` directory.
5. Restart API Gateway. The REST Jabber service is now available.
6. Add your servlet application and servlet using Policy Studio or ES Explorer.
7. Test the REST service.

## Add a servlet using Policy Studio

To create a servlet using Policy Studio, perform the following steps:

1. Start Policy Studio, and connect to the API Gateway.
2. Select **Environment Configuration > Listeners > API Gateway > Default Services > Paths**.
3. Right-click **Paths** and select **Add Servlet Application**.
4. On the **General** tab, enter `/` in the **Relative Path** field.
5. Click **OK**.
6. Right-click the servlet application path you just created in the Paths window, and select **Add Servlet**.
7. Enter `smack` in the **Name** field.
8. Enter `smack` in the **URI** field.
9. Enter `org.glassfish.jersey.servlet.ServletContainer` in the **Class** field.
10. Click **Add** under the Servlet Properties table to add a new property with the following values:
    * Name: `jersey.config.server.provider.packages`
    * Value: `com.vordel.jabber.rest`
11. Click **OK**.
12. Click **Deploy** or press **F6** to deploy the new configuration on the API Gateway.

## Test the REST Jabber service

To test the service, launch a web browser and enter the following URL:

```
http://localhost:8080/smack/jabber/{msg}/{to_email_address}
```

Replace `msg` and `to_email_address` in the URL with the message and the email address of the recipient.

Alternatively, you can execute the REST client that is included with the REST classes in the the `DEVELOPER_SAMPLES/restJabber` directory. Fill in the details of the message and the recipient's email address in the client class. You can then build and execute the client using the Ant targets supplied.

If the service is working correctly, an IM is sent and a string message is returned.

## Get the ID of a group or API Gateway instance

Every group and API Gateway instance in a domain is assigned a unique ID, and this ID is required to route a REST request to an API Gateway instance. This section describes how to find the ID of a group and API Gateway instance in a number of ways:

### Print the topology using managedomain

To run the `managedomain` script, enter the following commands:

```
cd INSTALL_DIR/apigateway/posix/bin/
managedomain --menu
```

Enter the domain user name and password when prompted. The topology management options are displayed as follows:

```
Topology Management:
  13) Print topology
  14) Check topologies are in synch
  15) Check the Admin Node Manager topology against another topology
  16) Synch all topologies
  17) Reset the local topology
```

Chose option 13, `Print topology`. This results in the following output:

```
Version: 2
Last updated: Wed Jan 30 10:17:20 GMT 2013

Hosts:
|
 ---127.0.0.1 [host-1]
Admin Node Manager:
|
 ---Node Manager on 127.0.0.1 [nodemanager-1] (https://127.0.0.1:8090)
Groups:
|
 ---QuickStart Group [group-2]
   |
    ---QuickStart Server [instance-1] (https://127.0.0.1:8085)
```

All IDs are shown in square brackets beside the node in the topology. In this example, the following are the names and associated IDs:

| Type        | Display name      | ID         |
|-------------|-------------------|------------|
| Group       | QuickStart Group  | group-2    |
| API Gateway | QuickStart Server | instance-1 |

### Use curl to call the Topology REST API

The Admin Node Manager is running the Topology API, and this can be called to return a list of groups and API Gateways running in the domain.

To call the API, execute the following curl command (replace `UNAME:PWD` with the domain user name and password):

```
curl --insecure --user UNAME:PWD https://127.0.0.1:8090/api/topology
```

The result is a JSON response with a format similar to the following.

```json
{
    "result": {
        "id": "50fd7b96-6e8f-401e-b38c-eb77891e3aeb",
        "version": 2,
        "timestamp": 1428938393531,
        "productVersion": "7.4.1",
        "hosts": [
            {
                "id": "host-1",
                "name": "ITEM-A21575.wks.axway.int"
            }
        ],
        "groups": [
            {
                "id": "group-1",
                "name": "Node Manager Group",
                "tags": {},
                "services": [
                    {
                        "id": "nodemanager-1",
                        "name": "Node Manager on ITEM-A21575.wks.axway.int",
                        "type": "nodemanager",
                        "scheme": "https",
                        "hostID": "host-1",
                        "managementPort": 8090,
                        "tags": {
                            "internal_admin_nm": "true"
                        },
                        "enabled": true
                    }
                ]
            },
            {
                "id": "group-2",
                "name": "QuickStart Group",
                "tags": {},
                "services": [
                    {
                        "id": "instance-1",
                        "name": "QuickStart Server",
                        "type": "gateway",
                        "scheme": "https",
                        "hostID": "host-1",
                        "managementPort": 8085,
                        "tags": {},
                        "enabled": true
                    }
                ]
            }
        ],
        "uniqueIdCounters": {
            "NodeManager": 2,
            "Group": 3,
            "Host": 2,
            "Gateway": 2
        },
        "fips": false,
        "services": [
            {
                "id": "nodemanager-1",
                "name": "Node Manager on ITEM-A21575.wks.axway.int",
                "type": "nodemanager",
                "scheme": "https",
                "hostID": "host-1",
                "managementPort": 8090,
                "tags": {
                    "internal_admin_nm": "true"
                },
                "enabled": true
            },
            {
                "id": "instance-1",
                "name": "QuickStart Server",
                "type": "gateway",
                "scheme": "https",
                "hostID": "host-1",
                "managementPort": 8085,
                "tags": {},
                "enabled": true
            }
        ]
    }
}
```

All IDs are found in strings named `id` and are highlighted above.

{{< alert title="Note" color="primary" >}}The Admin Node Manager is itself within a group with the ID `group-1`. {{< /alert >}}

In this example, the following are the names and associated IDs:

| Type        | Display name      | ID         |
|-------------|-------------------|------------|
| Group       | QuickStart Group  | group-2    |
| API Gateway | QuickStart Server | instance-1 |

### Use Jython to query the Topology API

You can call the Topology API from Jython scripts. The sample Jython script `INSTALL_DIR/apigateway/samples/scripts/topology/outputIDs.py` uses the Topology API to output the name and ID of the group and API Gateway instance.

```
cd INSTALL_DIR/apigateway/samples/scripts
./run.sh topology/outputIDs.py

API Server 'QuickStart Server' has id 'instance-1' belongs to Group 'QuickStart
Group' with id 'group-2', it is running on...
```
