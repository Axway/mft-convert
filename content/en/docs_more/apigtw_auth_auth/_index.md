{
    "title": "Integrate with Identity Management servers",
    "linkTitle": "Integrate with Identity Management",
    "weight":"66",
    "date": "2020-01-20",
    "hide_readingtime": "true",
    "description": "Integrate API Gateway to authenticate and authorize users for Identity Management servers including LDAP servers, CA SiteMinder, RSA Access Manager, Oracle Access Manager, and Oracle Entitlements Server."
}

API Gateway contains a set of message filters that directly or indirectly restrict access to resources or web services.

Filters that directly control access include XML-signature verification, CA certificate chain verification, and SAML assertion verification. With these filters, policy decisions are made and enforced within API Gateway itself.

Filters that _indirectly_ control access offload the policy decision to an external access management system. With these filters, the policy decision is made by the external system but then enforced by API Gateway.

API Gateway can leverage your existing Identity Management infrastructure, thus avoiding the need to maintain separate silos of user information. For example, if you already have a database full of user credentials, API Gateway can authenticate requests against this database rather than using its own internal user store. Similarly, the API Gateway can authorize users, look up user attributes, and validate certificates against third-party Identity Management servers.
