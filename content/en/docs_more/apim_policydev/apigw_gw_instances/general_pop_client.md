{
"title": "Configure a POP client",
  "linkTitle": "Configure a POP client",
  "weight": 8,
  "date": "2019-10-17",
  "description": "Poll a POP mail server and read email messages from it."
}
The API Gateway POP Client enables you to poll a Post Office Protocol (POP) mail server and read email messages from it. When the messages have been read, they can be passed into the core message pipeline where the full collection of message processing filters can act on them.

## Configuration

You can configure a POP client by right-clicking an API Gateway instance node under the **Environment Configuration** > **Listeners** node in the Policy Studio tree, and selecting the **POP Client**

**Add** menu option. Complete the following fields on the **POP Mail Server** dialog:

**Server Name**: Enter the host name or IP address of the POP mail server.

**Port**: Enter the port on which the POP server is listening. By default, POP servers listen on port 110.

**Connection Security**: Select the security used to connect to the POP server (`SSL`, `TLS`, or `NONE`). Defaults to `NONE`.

**User Name**: Enter the user name of a configured mail user for this POP server.

**Password**: Enter the password for this user.

**Poll Rate**: Enter the rate at which the instance polls the mail server in milliseconds.

**Delete Message from Server**: Specifies whether the POP server deletes email messages after they have been read by the instance. This setting is selected by default.

**Email Debugging**: Select this setting to find out more information about errors encountered by the API Gateway when polling the POP server. All trace files are written to the `/trace` directory of your API Gateway installation. This setting is not selected by default.

**Policy to Use**: Select the policy to use to process messages that have been read from the POP server.