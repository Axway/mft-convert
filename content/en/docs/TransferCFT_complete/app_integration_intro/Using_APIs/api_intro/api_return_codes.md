{
    "title": "Return codes",
    "linkTitle": "REST API return codes",
    "weight": "340"
}The Rest server returns some data in HTTP format, including a status header, but the data may also contain a body. You can refer to the Swagger web interface for the exact reply syntax.

HTTP status codes include:


| Code  | Status  | Meaning, when to use  |
| --- | --- | --- |
| 200  | OK  | Everything went fine. The resource you requested has been found.  |
| 201  | Created  | Use it for synchronous processing, that is, when creating a resource synchronously. This status code goes along with the Response Location HTTP header, indicating where the new resource is accessible.  |
| 202  | Accepted  | Use it for asynchronous processing, typically for long running tasks (image processing, file compression, etc.). It indicates that the server has accepted the request but the result is not available yet. Responses usually contain a ‘task’ id the client can request (on a different URL) to get the status of a given long-running task.  |
| 400  | Bad request  | Indicates that the request is malformed. This is usually used to enforce that the requester is providing adequate parameters to the operation. For instance, if the operation requires a numerical identifier and gets a text instead, the server should return this status code.  |
| 401  | Unauthorized  | The requester provided invalid credentials. May indicate that the user and or password are not correct, or that the rights for the user are not sufficient to modify a transfer.  |
| 404  | Not found  | The resource was not found. The URL may not be correct.  |

