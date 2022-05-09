{
"title": "Secure by default configuration",
"linkTitle": "Secure by default configuration",
"weight": 70,
"date": "2019-11-25",
"description": "Summary of the secure-by-default settings for API Gateway, API Manager, and API Portal."
}

Security best practices recommend that products are shipped secure-by-default to ensure that an out-of-the-box installation is not vulnerable to attacks.

## API Gateway and API Manager secure-by-default settings

The following measures have been taken to ensure that API Gateway is secure-by-default:

* The default behavior of the API Gateway installer is to force the user to set an administrator password during installation.
* Administrator passwords are stored as a salted hash using 102400 iterations of PBKDF2 with HMACSHA2. The iterations and algorithm are not configurable.
* The installer sets permissions on the image so that only the user that installed the product can modify API Gateway files (for example, logs, traces, configuration, and so on).
* HTTPS interfaces are configured with secure cipher suites by default; for example, TLSv1.2 only, block SSLv3, FIPS compliant cipher suites only, no unauthenticated ciphers, and so on.
* Connection filters are configured with secure cipher suites by default; for example, TLSv1.2 only, block SSLv3, FIPS compliant cipher suites only, no unauthenticated ciphers, and so on.
* All connections between internal server-side components (API Gateway and Node Manager) are secured by two-way SSL.
* Connections between Policy Studio and the Node Manager are secured by SSL.
* When deploying configuration to API Gateway instances in Policy Studio, you must enter the administrator user name and password to authenticate to the Admin Node Manager. This ensures that only an administrator user can push policies to API Gateway instances.
* API Gateway seeds itself from `/dev/random` for all cryptographic operations, which is cryptographically more secure than using `/dev/urandom`.
* API Manager users are set to change password on first login.

Unless otherwise specified, all of these options can be turned off by an administrator user.

{{< alert title="Caution" color="warning" >}}Turning off secure-by-default configuration effectively makes API Gateway less secure.{{< /alert >}}

## API Portal secure-by-default settings

For API Portal, the following are used to ensure that it is secure-by-default:

* Joomla! administrators are set to change password on first login.
* API Portal users are set to change password on first login.
* Error reporting is set to **None**.
* SSL is enforced for entire set.
* Connections to API Manager are secured.
* User registration through Joomla!’s authentication plugin is disabled.
* Password minimum length for Joomla! administrators is set to 8 symbols.
* The permissions of the API Portal files are set so that only the wwwrun that has write access and only users belonging to the www group have write access.
* The HTTPS interface is configured with secure cipher suites by default - TLSv1.2 only and FIPS compliant cipher suites only.
* Search engine friendly URLs are enabled. This helps mask information and prevents hackers from finding vulnerabilities.
