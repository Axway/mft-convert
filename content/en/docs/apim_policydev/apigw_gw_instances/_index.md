{
"title": "Configure API Gateway instances and listeners",
"linkTitle": "Configure instances and listeners",
"weight": 30,
"date": "2019-10-17",
"description": "Configure API Gateway instances and listeners in Policy Studio."
}

A single running instance of API Gateway enables you to configure at least two interfaces: one for public traffic, and a second for listening for and serving configuration data. The configuration interface should rarely need to be updated. However, you might need to add several HTTP interfaces. For example, an HTTP interface and an SSL-enabled HTTPS interface.

Furthermore, you can add features such as the following at the API Gateway instance level:

* Remote hosts to control connection settings to a server
* SMTP interfaces to configure email relay
* File transfer services for FTP, FTPS, and SFTP
* Policy execution schedulers to run policies at regular time intervals
* JMS listeners to listen for JMS messages
* Packet sniffers to inspect packets at the network level for logging and monitoring
* FTP pollers to retrieve files to be processed by polling a remote file server
* Directory scanners to scan messages dumped to the file system

Because API Gateway can read messages from HTTP, SMTP, FTP, JMS, or a directory, this enables it to perform protocol translation. For example, API Gateway can read a message from a JMS queue, and then route it on over HTTP to a web service. Similarly, API Gateway can read XML messages that have been put into a directory on the file system using FTP, and send them to a JMS messaging system, or route them over HTTP to a back-end system.
