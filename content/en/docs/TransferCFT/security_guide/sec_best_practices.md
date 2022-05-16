---

    title: Best practices
    linkTitle: Best practices
    weight: 80

---
When using this product, please follow the security best practices described in this section.

## Secure connections

Secure connections with external networks are absolutely mandatory and yet can also make sense for internal connections. For example, connections between an SSO proxy, such as PassPort, and an application must always be SSL/TLS-secured with mutual authentication to avoid someone with no credentials connecting to the proxy header.

## Self-signed certificates

Using self-signed certificates may also be a security risk for many reasons, including:

- Anyone can generate his own certificate and you need a very secure process to receive/send these certificates to make sure they are coming from the right partner. When using CA-signed certificates, you can rely on the CA.
- If self-signed certificates are not securely stored, anyone can change them. CA-signed certificates also must be securely stored, but no one can change them.
- There is no way to revoke self-signed certificates.

## Privileged access user list

Maintain a list of users with privileged access. At a minimum, maintain a list of administrators and a list of users with access to multiple functions.

## Internet access limitation

As much as possible, limit the number of Internet access points. Do not open useless Internet connections and limit interconnections with external networks as much as possible. This reduces the risk of external attacks and makes it easier to audit the product.

## Service packs and patches

In the event of a possible vulnerability discovered in the product, you must be able to apply the patch or new service pack as soon as possible. Make sure you have the correct procedure to complete the update. Always use the latest version of the product, if possible, as it contains fixes to known vulnerabilities.

## Generic or anonymous users

The term "generic users" means that the password is shared among multiple specific users. This makes it easier for an attacker to retrieve this password. In addition, the procedure to change shared passwords can be complicated and risky. In case of an incident, these generic or anonymous users make it impossible to determine who completed the erroneous action.

## Password policy

Password policy refers to size and complexity of the password, as well as to all the rules to manage this password. The size and complexity is important. The policy should define that the length be a minimum of 8 characters, contain a mix of alphabetic characters with numbers and special characters, and be case-sensitive.

Your password policy:

- Must force passwords to be changed periodically.
- Should prohibit reuse of a password before n number of different passwords and within a certain time period.
- Must define how the password is created differently from the old one (such as: at least a certain number of different characters).
- Must limit the number of failed attempts.

## Remote connections

You should limit your remote connections in the following ways:

- If no one needs to access the product remotely, make sure that the UI ports are closed in your firewall.
- If someone needs to connect remotely only occasionally, set a procedure to open the port only on demand.
- If there are regular remote users, make sure that the connections are as secure as possible, such as using HSTS or VPN.

## Logging, audit, and alert rules

An important aspect of security is to be notified when something goes wrong, and to be able to investigate the issue. Therefore, it is mandatory that you define the correct level of logging and audits, and set up the appropriate alerts. This can be done using an external Sentinel server.

Additionally, logging and auditing are the main tools for investigating when something happens in the system. Unfortunately, it could take some time before a problem is detected, meaning that the investigation may take place much later than when the problem occurred or was detected. For this reason, it is very important to periodically archive the log and audit trails, and to define a log and audit retention policy.

To enable logging and audit when using Central Governance, please refer to the *Central Governance User Guide* ****Transfer CFT Configuration &gt; Configure visibility**** section.

Without Central Governance, you can enable logging and audits through Axway Sentinel using the following Transfer CFT{{< TransferCFT/axwayvariablesComponentLongName  >}} parameters. Please refer to the [Sentinel Monitoring User Guide](https://docs.axway.com/bundle/Sentinel_420_Monitoring_allOS_en_webhelp/page/Content/monitoring_web_interface/using_interface/AxwayStartPage_Monitoring.htm) for information on using Sentinel.


| Parameter | Description | Type |
| --- | --- | --- |
| sentinel.xfb.enable | Enables the Sentinel connector. | Boolean: Yes | No |
| sentinel.xfb.transfer | Level of detail for Transfer Message content. | Enumeration: ALL SUMMARY ERROR NO |
| sentinel.xfb.audit  | Enables configuration change logging.  | Boolean: Yes | No  |


## Sensitive files and databases

It is imperative that you protect sensitive files and databases. The configuration file contains information that can be useful to a hacker. Even if part of this information is encrypted, access to this file must be restricted to local access management. Only the product and one or two administrators should have access to this file in any mode (read, update, or delete).

The configuration files are located by default in the data folder in the runtime directory:

- The `cftuconf.dat` file containing customized uconf values
- The `cftparm `file is the database containing parameters, keys, certificates, and partners

The encryption key and salt files are located by default in the `data/crypto` directory.


| File | Description | Access for Transfer CFT<br/> Product and administrator | Access for others |
| --- | --- | --- | --- |
| cftuconf.dat | Configuration file | READ, WRITE | READ |
| cftparm | Configuration database | READ, WRITE | READ |
| crypkey  | Ciphering file  | READ, WRITE  | NONE  |
| crypsalt  | Salt file  | READ, WRITE  | NONE  |


The product database also contains sensitive data; you should prohibit access to this database from any other tools or applications using the API, apart from one or two administrators.

In the `<runtimedir>/data` directory:

- The cftcom file (and cftcomNN files in multi-node setting), which contains the wait list for commands processed by the Transfer CFT
- The cftcat file (and cftcatNN files in multi-node setting), which contains information about the processed transfer
- The CFTAM file, which contains access management privileges


| File | Description | Access for Transfer CFT<br/> Product and administrator | Access for others |
| --- | --- | --- | --- |
| cftcom | Communication file | READ, WRITE | READ, WRITE (an application requires write authorization to create and manipulate new transfer requests) |
| cftcat | Catalog file | READ, WRITE | READ |
| CFTAM  | Access management's persistent cache  | READ, WRITE  | READ  |


The runtime/log folder contains log files. Read access to these files may enable a user to access to sensitive information. Additionally, note that the runtime/run folder contains data, temporary files, as well as traces.

### SAML 

When implementing SAML, it is recommended that you use the same precautions, and have the appropriate rights, as described in *Sensitive files and databases* (section above) if you modify the SAML related UCONF values or certificates in the PKI database.

## Unusual behavior and automated tools detection

This product features both a web UI and REST API, meaning that it is susceptible to attacks on the Internet. Even if the product already has the protection described in the “Security Features” section of this document, we strongly recommend that you also implement additional protection such as proxies and WAF to secure connections. These tools enable you to detect unusual behavior and to identify connections from an automated tool. Make sure to receive alerts when using these tools, and to secure involved users and their addresses.

At a minimum, you should perform a periodic review of the log and audit to inductively detect any issues.
