---

    title: Security features
    linkTitle: Security features
    weight: 50

---
Transfer CFT provides the following security features:

- Secure connections
- Password management
- Certificate management
- Identity and access management
- System user control (USERCTRL)

<span id="__RefHeading___Toc473905757"></span>

## Secure connections

The following secure connections are available:

- Connections to other Axway products can be SSL/TLS secured. When using Central Governance, the certificate used to secure these connections is generated when the product registers with Central Governance.
- Connections between the UI and the server can be secured. This is accomplished using the HTTPS and SSL/TLS.
- Connections with partners for file transfers can be secured using SSL/TLS.

********Secure connections and descriptions********

![](/Images/TransferCFT/sec_guide3.png)

![](/Images/TransferCFT/sec_legend.png)

<span id="__RefHeading___Toc473905758"></span>

## Password management

- Every password-type value that needs to be retrieved by the product is AES-256 encrypted. The key used to perform this encryption is derived from the password provided at installation. You can change this password at any time using the **cftcrypt** tool.
- On Unix, you may need to use the XFBADM database. Every XFBADM password that does not need to be retrieved by the product is stored hashed with a salt using the MD5 algorithm.
- Do not start passwords with an indirection character (Unix @/ Windows #).

## Certificate management

<span id="__RefHeading___Toc473905760"></span>

### PassPort PS

The following features are available:

- Can connect to PassPort PS to fully manage certificates. When using PassPort, a full validation of the certificates is done (CRL or OSCP server, full chain validation).
- Local private keys are stored in the AES-256 encrypted database. The key used to perform this encryption is derived from the password provided at installation. You can change this password at any time using the administration UI.
- The local private keys can also be stored in an HSM (nCipher nShield, WebSentry).
- CSR can be imported or generated to create a certificate in association with an external CA.

See the PassPort documentation for a complete list of product features.  

<span id="__RefHeading___Toc473905761"></span>

### Central Governance / Flow Manager

The following features are available:

- Certificate chains are checked and validated.
- The revocation list is not checked.
- Local private keys use AES-256 encryption if the `crypto.key_fname` parameter is defined (otherwise Triple DES encryption is used), and are stored in the Transfer CFT internal PKI database. See the *[UCONF parameters](../../admin_intro/uconf/uconf_directory)* section for details on `crypto.key_fname`.

### When not using PassPort, Central Governance, or Flow Manager

The following features are available:

- Certificate chains are checked and validated.
- The revocation list is not checked.
- Local private keys use AES-256 encryption if the `crypto.key_fname` parameter is defined (otherwise Triple DES encryption is used), and are stored in the Transfer CFT internal PKI database. See the *[UCONF parameters](../../admin_intro/uconf/uconf_directory)* section for details on `crypto.key_fname`.

<span id="__RefHeading___Toc473905763"></span>

## Identity and access management

You can perform identity and access management either internally in the product, or using Central Governance, PassPort AM, or SAML.

<span id="__RefHeading___Toc473905764"></span>

### PassPort AM

PassPort AM features include:

- An RBAC: Role Base Access Control.
- Ability to store users and associated passwords in either a PassPort database, or are retrievable from an LDAP directory or Active Directory. You can also retrieve users/passwords from another third-party product, though some code must be written to access these users.
- An SSO option. PassPort includes a reverse proxy and a token manager to provide an SSO option only for Axway products.

See the PassPort documentation for a complete list of features in this product.

<span id="__RefHeading___Toc473905765"></span>

### Central Governance / Flow Manager

Central Governance provides the same access management features as PassPort AM.

<span id="__RefHeading___Toc473905766"></span>

### When not using PassPort, Central Governance, or Flow Manager

The following features are available:

- An RBAC: Role Base Access Control.
- The users and associated passwords can be stored in either the local OS or in the XFBADM database (Unix only). Additionally, these users/passwords can be retrieved from another third-party product, though you must write some code to access these users.

<span id="SAML"></span>

### SAML and SSO

SAML is an open standard that allows the exchange of authentication and authorization data between an identity provider and a service provider.

The web browser single sign-on, SSO, profile enables users to use the same logging details for Transfer CFT{{< TransferCFT/axwayvariablesComponentLongName  >}} as other Axway products (for example, Flow Manager). See the *[Transfer CFT User Guide](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/AxwayStartPage.htm)* for details and limitations.

## System user control (USERCTRL)

When transferring files, the user account that starts Transfer CFT is used to access those files by default. The system user control (USERCTRL) feature allows Transfer CFT to read and write files and execute processing scripts on behalf of a user other than the one who operates Transfer CFT. By its very essence, this feature has security impacts because it bypasses the file access rights defined at the operating system level.

A malicious user who has rights to create flows and execute transfers can, using Transfer CFT, access files that he is not authorized to per the operating system file rights definition. Additionally, this user can execute a script on behalf of a specific user (as defined in the UCONF `cft.server.exec_as_user` parameter) and cause damage, such as removing important files.

Let's take, for example, two users - Alice and Bob. Alice does not have rights to read files that are in Bob’s home directory. Alice is able, however, to create a flow and execute a transfer on Transfer CFT.

Now, let's consider a scenario where Alice defines the following configuration objects:

- Alice creates a SEND template that allows her to read one of Bob’s files:  
    ```
    CFTSEND id=bob_disclosure,
    impl=yes,
    fname=/home/bob/thesecretfile,
    userid=bob
    ```
- Alice creates a loop partner:  
    ```
    CFTPART id=loop,
    nspart=loop,
    nrpart=loop,
    prot=pesit,
    sap=1761
    CFTTCP id=loop, host=localhost
    ```
- Alice executes a loop transfer:  
    ```
    recv part=loop, idf=bin, nidf=bob_disclosure, fname=/home/alice/thesecretfile
    ```

In this scenario, Transfer CFT reads the` /home/bob/thesecretfile` file on Bob's behalf, transfers it over PeSIT, and stores it in `/home/alice/thesecretfile`. Alice can now read Bob’s secret file.

### Mitigating the threat

Alice can read Bob’s file without Bob’s credentials because she is authorized to create an implicit send template (CFTSEND impl=yes) and to execute a transfer.

For this reason, unless required in your flow definitions, do not authorize anyone to create an implicit send template (the [CONFIGURATION:CFTSENDI](../iam/predefined_privileges) resource manages CFTSEND impl=yes). If you require an implicit send template in your configuration, do not authorize the same user to create this send template and execute transfers.

### Enabling USERCTRL

To enable the USERCTRL feature, set the CFTPARM:USERCTRL parameter to YES noting, the following operating system specificities:

- On UNIX, the CFTSU or CFTSUD executable runs under the root account.
- On Windows, the user operating Transfer CFT requires the "SeCreateTokenPrivilege: Create a token object" privilege, which is a very high privilege that Microsoft does not recommend. Per Microsoft <sup>TM</sup> "Caution: A user account that is given this user right has complete control over the system, and it can lead to the system being compromised. We highly recommend that you do not assign this right to any user accounts. Processes that require this user right should use the Local System account, which already includes it, instead of a separate user account that has this user right assigned." Please refer to the [Microsoft documentation](https://docs.microsoft.com/fr-fr/windows/security/threat-protection/security-policy-settings/create-a-token-object) on token objects for details. Following the Microsoft warning, we recommend running Transfer CFT as a service using the Local System account. However, if you require the use of an account other than the Local System, we recommend that you prevent this account from logging on locally. Use the **Local Security Policy** and choose **Deny log on locally** for that account.
- On z/OS, the user operating Transfer CFT requires the \*ALLOBJ special authority.
