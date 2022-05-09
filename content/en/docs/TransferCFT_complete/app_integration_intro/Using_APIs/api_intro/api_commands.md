{
    "title": "REST API operations",
    "linkTitle": "REST API operations",
    "weight": "330"
}This page provides information about Transfer CFT REST API, and examples of how you can use these REST API to simplify business needs.

About the requests
------------------

You can use REST API in Transfer CFT to perform several operations. These operations are done using GET, POST, PUT and DELETE HTTP requests.

The requests are comprised of a URL and parameters, where a different URL is used for each operation.

The HTTP body can only be sent in json format, and application/json is the only Content-Type accepted.

Click to access the [Transfer CFT API Swagger](http://apidocs.axway.com/swagger-ui/index.html?productname=transfercft&productversion=3.8&filename=transfercft-swagger-api.json) documentation.

Manage transfers with REST API
------------------------------

You can perform the following operations using REST API:

- Create transfers using an HTTP POST request.
    -   The PART and IDF parameters are mandatory.
    -   Other optional parameters can be given.
- Modify existing transfers using HTTP PUT request.
    -   The IDTU is mandatory.
- View transfer catalog records using HTTP GET request.
    -   You can retrieve a single transfer using the IDTU field.
    -   You can also retrieve a list of transfers in a set and a limit of the number of transfers.
- Delete a transfer using HTTP DELETE request, to remove a transfer based on its IDTU.

REST API operation examples
---------------------------

This section provides examples of the REST API requests that you can use to do a variety of tasks. The following examples use the curl tool.

For more information about curl, please visit [https://curl.haxx.se](https://curl.haxx.se/).

### GET retrieves data for a transfer or a set of transfers

To retrieve a set of transfers execute an HTTP GET request on the `/cft/api/v1/transfers` resource. For example:

```
curl -k -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGnMQdLK6lwYPwy6" -X GET "https://<copilot_host>:<uconf:copilot.restapi.serverport>/cft/api/v1/transfers"
```

To retrieve a transfer execute an HTTP GET request on the `/cft/api/v1/transfers/<IDTU>` resource. For example:

```
curl -k -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGnMQdLK6lwYPwy6" -X GET "https://<copilot_host>:<uconf:copilot.restapi.serverport>/cft/api/v1/transfers /<IDTU>
"
```

### POST creates a new transfer

To create a send file transfer request execute an HTTP POST request on the `/cft/api/v1/transfers/files/outgoings` resource. You must specify the PART and IDF in the URL, however you can add optional parameters in the HTTP body. For example:

```
curl -k -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGnMQdLK6lwYPwy6" -X POST "https://<copilot_host>:<uconf:copilot.restapi.serverport>/cft/api/v1/transfers/files/outgoings?part=<PART>&IDF=<IDF>"
```

#### Manage the REST API timeout

In the URL request, you can use the `apiTimeout `parameter to set the timeout for the current request.

For example:

```
curl -k -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGnMQdLK6lwYPwy6" -X POST "https://<copilot_host>:<uconf:copilot.restapi.serverport>/cft/api/v1/transfers/files/outgoings?part=<PART>&IDF=<IDF>&apiTimeout=20"
```

When set to:

- -1: The timeout is infinite for the request.
- 0: No timeout is set for the request. The result is sent ASAP.
- Any other value: This value is used as a maximum retry timeout for the request.
- If this parameter is not set, the `copilot.restapi.com.retry_timeout` UCONF parameter is used.

### PUT updates a transfer identified by its IDTU

To perform an action on an existing transfer, such as restarting a transfer, execute an HTTP PUT request. For example:

```
curl -k -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGnMQdLK6lwYPwy6" -X PUT "https://<copilot_host>:<uconf:copilot.restapi.serverport>/cft/api/v1/transfers/<IDTU>/start"
```

### DELETE removes a transfer

To delete a transfer, execute the HTTP DELETE request on the `/cft/api/v1/transfers/<IDTU>` resource.

For example:

```
curl -k -H "Content-Type: application/json" -H "Authorization: Bearer eyJhbGnMQdLK6lwYPwy6" -X DELETE "https://<copilot_host>:<uconf:copilot.restapi.serverport>/cft/api/v1/transfers/<IDTU>"
```
