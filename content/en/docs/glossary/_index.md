{
"title": "Glossary",
"linkTitle": "Glossary",
"weight":"99",
"date": "2019-11-07",
"hide_readingtime": "true",
"description": "Reference to Amplify API Management and Amplify Central terminology."
}
## A

Admin Node Manager

: API Gateway component responsible for managing API Gateway instances in a domain. For example, this includes collecting monitoring information, managing dynamic settings, and deploying API and policy configuration. The Admin Node Manager must be running to use the API Gateway management tools that connect to it (for example, Policy Studio and API Gateway Manager).

API

: Application Programming Interface (API) is a set of business services that an enterprise can expose to external customers, partners, or employees using a range of different technologies on a range of different devices. For example, APIs typically support HTTP requests and JSON or XML responses to enable mobile client applications.

API Catalog

: Contains the APIs that have been registered in API Manager and are available for use by client application developers. They can browse these APIs and their associated documentation, and invoke APIs using the built-in test capability.

API Gateway

: Server-side application that manages, delivers, and secures APIs. API Gateway provides services such as API transformation, control and governance, security, monitoring, development lifecycle, and administration.

API Manager

: Web-based API administration and partner management tool that is layered on API Gateway. API administrators use API Manager to administer the managed APIs that are exposed to API consumers.

API package

: The complete package of artifacts associated with an API registered in API Manager. This is used to export and import the API in a single package to enable promotion from sandbox to production APIs.

API Portal

: Self-service developer portal that enables client application developers to browse and consume APIs for use in their applications.

## B

Base64

: Method of encoding 8-bit characters as ASCII printable characters. It is typically used to encode binary data so that it may be sent over text-based protocols such as HTTP and SMTP. Base64 is a scheme where 3 bytes are concatenated, and split to form 4 groups of 6-bits each. Each 6-bits is translated to an encoded printable ASCII character, using a table lookup. The specification is described in RFC 2045.

Back-end API
: Used in context of the API-Manager. Is the actual REST API that is routed to, secured, and exposed on the network (for example, application server), or in the Cloud (for example, Twitter). This REST API can be registered manually in API Manager, or by importing a Swagger or Web Application Description Language (WADL) definition in API Manager.

## C

CA

: Certificate Authority (CA) issues digital certificates (especially X.509 certificates), and vouches for the binding between the data items in a certificate.

cacerts

: File used to keep the root certificates of signing authorities. This is typically stored in `..\jre\lib\security\cacerts`. Each entry is identified by a unique alias, and is a key entry or a certificate entry. Key entries consist of a key pair, and certificate entries consist of just a certificate. Because you implicitly trust all CAs in the `cacerts` file for code signing and verification, you must manage the `cacerts` file carefully. The `cacerts` file should contain only certificates of the CAs you trust.

CMS

: Content Management System.

CRL

: Certificate Revocation List (CRL) is a signed list indicating a set of certificates that are no longer considered valid by the certificate issuer. CRLs may be used to identify revoked public-key certificates or attribute certificates, and may represent revocation of certificates issued to authorities or to users. The term CRL is also commonly used as a generic term applying to different types of revocation lists.

## D

DName

: Distinguished Name (DName or DN) is an identifier that uniquely represents an object in the X.500 Directory Information Tree (DIT). A DName a set of attribute values that identify the path leading from the base of the DIT to the object that is named. An X.509 public-key certificate or CRL contains a DName that identifies its issuer, and an X.509 attribute certificate contains a DN or other form of name that identifies its subject.

Domain

: Multiple groups of API Gateways spanning multiple host machines. An API Gateway domain is a distinct administrative entity, which is managed separately by tools such as API Gateway Manager and API Gateway Analytics.

## F

Filter

: Executable rule that performs a specific type of processing on a message. For example, the Message Size filter rejects messages that are greater or less than a specified size. Many categories of message filters are available with API Gateway (for example, Authentication, Authorization, Content filtering, Conversion, Trust, and so on). In Policy Studio, a filter is displayed as a block of business logic that forms part of an execution flow known as a policy.

Front-end API

: Used in context of the API-Manager. Is the virtualized publicly exposed REST API in API Manager that is hosted on the API Gateway, and which client applications invoke (for example, iPhone or Android client apps). By default, the front-end API is the same as the back-end API, proxying the API as is. However, you can edit the front-end API to present an enriched, public-facing API to client applications.

## G

Group

: One or more API Gateway instances that are managed as a unit and run the same configuration to virtualize the same APIs and execute the same policies. API Gateway groups enable you to organize API Gateway instances by solution type and manage them as a single entity.

## H

HTTP

: Hypertext Transfer Protocol (HTTP) is a protocol for distributed hypermedia systems. HTTP is the foundation of data communication for the World Wide Web. For more details, see <http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol>.

HTTPS

: Hypertext Transfer Protocol Secure (HTTPS) is a protocol for secure communication over a computer network, and which is widely deployed on the Internet. It is the result of layering HTTP on top of the SSL/TLS protocol. For more details, see <http://en.wikipedia.org/wiki/HTTP_Secure>.

## J

JMS

: Java Message Service (JMS) is a messaging standard that enables application components based on Java 2 Enterprise Edition (J2EE) to create, send, receive, and read messages. It enables communication between different components of a distributed application to be loosely coupled, reliable, and asynchronous. For more details, see <http://en.wikipedia.org/wiki/Java_Message_Service>.

JSON

: JavaScript Object Notation (JSON) is a lightweight data-interchange format, which is easy for humans to read and write, and easy for machines to parse and generate. JSON is based on a subset of the JavaScript programming language. Its text format is programming language independent, but uses conventions that are familiar to programmers of the C family of languages (for example, C, C++, C#, Java, JavaScript, Perl, and Python). For more details, see <http://www.json.org>.

JSON Path

: JSON Path enables you to locate and process specific parts of a JSON document. It is available in programming languages such as JavaScript, Java, Python and PHP. For more details, see the JSON specification.

## K

Keystore

: The keystore file of the JDK contains your public and private keys. It has a file name of `.keystore` (leading dot makes the file read-only on Unix). It is stored in PKCS #12 format, contains both public and private keys, and is protected by a passphrase.

KPS

: Key Property Store (KPS) is a data management component in the API Gateway. Data in a KPS table is assumed to be read frequently and seldom written, and can be changed without incurring an API Gateway service outage. KPS tables are shared across an API Gateway group.

## L

LDAP

: LDAP is a lightweight version of Directory Access Protocol (DAP), which is part of X.500, a standard for directory services in a network. An LDAP directory stores information on resources in a hierarchical fashion, which makes data retrieval very efficient.

## N

Node Manager

: API Gateway component responsible for managing API Gateway instances on a host machine. There must be one Node Manager on each managed host machine. A single Admin Node Manager communicates with all Node Managers in a domain to perform management operations.

## O

OCSP

: Online Certificate Status Protocol (OCSP) is an automated certificate checking network protocol. A client will query the OCSP responder for the status of a certificate. The responder returns whether the certificate is still trusted by the CA that issued it.

## P

PEM

: Privacy Enhanced Mail (PEM) was originally intended for securing email using various encryption techniques. Its scope widened for use in a broader range of applications, such as Web servers. Its format is essentially a base64-encoded certificate wrapped in `BEGIN CERTIFICATE` and `END CERTIFICATE` directives.

PKCS#12

: Standard for storing private keys and X.509 certificates securely (for example, in a `.p12` file).

Policy

: Network of API Gateway filters in which each filter is a modular unit that processes a message. Messages can traverse different paths through the policy, depending on which filters succeed or fail. For example, you could configure policies routing messages that pass a Schema Validation filter to a back-end system, and routing messages that pass a different Schema Validation filter to another system. A policy can also contain other policies, which enables you to build modular reusable policies.

Private key

: Secret component of a pair of cryptographic keys used for asymmetric cryptography.

Public key

: Publicly-disclosed component of a pair of cryptographic keys used for asymmetric cryptography.

## R

RBAC

: Role-Based Access Control (RBAC) restricts system access to authorized users based on assigned roles. Permissions to perform specific system operations are assigned to specific roles, and system users are granted permission to perform specific operations only through their roles. This simplifies system administration because users do not need to be assigned permissions directly, and instead acquire them through their assigned roles.

REST

: Representational State Transfer (REST) is an architectural style for building large-scale distributed software that uses the technologies and protocols of the World Wide Web (for example, JSON/XML and HTTP). For more details, see <http://en.wikipedia.org/wiki/Representational_state_transfer>.

## S

SAML

: Security Assertion Markup Language (SAML) is an XML standard for establishing trust between entities. SAML assertions contain identity information about users (authentication assertions), and information about user access permissions of (authorization assertions). When a user is authenticated at a site, the site issues a SAML authentication assertion to the user. The user can use this assertion in requests at other affiliated sites. These sites need only check the details in the authentication assertion to authenticate the user. In this way, SAML allows authentication and authorization information to be shared between different sites.

Selector

: Special syntax that enables API Gateway configuration settings to be evaluated and expanded at runtime based on metadata values (for example, from a KPS, message attribute, or environment variable).

Signature
: Value computed with a cryptographic algorithm and added to a data object in such a way that any recipient of the data can use the signature to verify its origin and integrity.

SOAP

: Simple Object Access Protocol (SOAP) is an XML-based object invocation protocol. SOAP was originally developed for distributed applications to communicate over HTTP and corporate firewalls. SOAP defines the use of XML and HTTP to access services, objects, and servers in a platform-independent way. SOAP is a wire protocol that can be used to facilitate highly ultra-distributed architecture. For more details, see the SOAP specification.

SSL

: Secure Sockets Layer (SSL) is an encrypted communication protocol for sending information securely across the Internet. It sits just above the transport layer, and below the application layer and transparently handles the encryption and decryption of data when a client establishes a secure connection to the server. It optionally provides peer entity authentication between client and server.

## T

TLS

: Transport Layer Security (TLS) is the successor to SSL 3.0. Like SSL, it allows applications to communicate over a secure channel.

## U

UDDI

: Universal Description, Discovery, and Integration (UDDI) is an XML-based lookup service for locating Web services on the Internet. For more details, see the UDDI standard.

URI

: Uniform Resource Identifier (URI) is a platform-independent way to specify a file or resource on the Web. Strictly speaking, every URL is also a URI, but not every URI is also a URL. For more details on URI formats, see RFC 2396 and RFC 2732.

## W

WSDL

: Web Services Description Language (WSDL) is an XML format for describing network services as a set of endpoints operating on messages containing document-oriented or procedure-oriented information. Operations and messages are described abstractly, and bound to a concrete network protocol and message format to define an endpoint. Related concrete endpoints are combined into abstract endpoints (services). WSDL is extensible to allow description of endpoints and messages regardless of what message formats or network protocols are used. For more details, see the WSDL specification.

## X

X.509

: Standard that defines the contents and data format of a public key certificate.

XKMS

: XML Key Management Specification (XKMS) uses XML to provide key management services so that a Web service can query the trustworthiness of a user's certificate over the Internet. XKMS aims to simplify application building by separating digital-signature handling and encryption from the applications themselves. For more details, see the XML Key Management specification.

XML

: Extensible Markup Language (XML) is a subset of Structured General Markup Language (SGML). Its goal is to enable generic SGML to be served, received, and processed on the Web in a similar way to HTML. See the XML Specification for more details.

XPath

: XML Path (XPath) is a language that describes how to locate and process specific parts of an XML document. For more details, see the XML Path Language specification.

XSL

: XML Stylesheet Language (XSL) is used to convert XML documents into different formats, the most common of which is HTML. In a typical scenario, an XML document references an XSL stylesheet, which defines how the XML elements of the document should be displayed as HTML. This enables a clear separation of content and presentation.

XSLT

: Extensible Stylesheet Language Transformation (XSLT) is used to convert XML documents into other XML documents or other formats (for example, HTML, plain text, or XSL Formatting objects).
