{
"title": "Security architecture",
"linkTitle": "Security architecture",
"weight": 50,
"date": "2019-11-25",
"description": "Architecture of API Gateway and API Portal from a security perspective."
}

## API Gateway architecture

The following diagram shows the product architecture from a security perspective.  The legend explains the security level on connections (SSL by default, always SSL, can be SSL, and so on) and on data storage (signed or encrypted).

![API Gateway architecture and connections](/Images/security/apigw_sec_arch_a5.png)

The diagram includes the following components:

* ES Conf: Entity Store Configuration, which is a file-based store for all policy data.
* Domain Creds: Salted hash of administrator user credentials.
* LDAP/IDM: Identity Management products, such as authentication or authorization servers.
* Domain: An administrative entity comprising at least one Admin Node Manager and at least one API Gateway.  These logical components can be located on the same physical or virtual host or separated across multiple physical or virtual hosts as required.

## API Portal architecture

The following diagram shows the API Portal architecture from a security perspective. The legend explains the security level on connections (SSL by default, always SSL, can be SSL, and so on) and on data storage (signed or encrypted).

![Configuration architecture for API Portal](/Images/APIPortal/api_portal_security_architecture.png)
