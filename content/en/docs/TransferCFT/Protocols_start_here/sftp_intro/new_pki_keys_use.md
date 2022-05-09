{
    "title": "Generate and manage keys",
    "linkTitle": "Generate and manage keys",
    "weight": "170"
}{{% TransferCFT/snippets/sftp_os_limitation%}}

This section describes how to establish secure sessions and generate keys, import etc. in the context of SFTP.

<span id="Use"></span>

Use PKIKEYGEN to generate and import a key pair
-----------------------------------------------

You can use the `PKIKEYGEN `command to generate a key pair, where it then stores them in the local PKI database.

```
PKIUTIL PKIKEYGEN
ID=KEY_2048,
PKIFNAME=$CFTPKU,
STATE=ACT,
KEYLEN=2048,
MODE=CREATE,
COMMENT="2048-bits RSA key"
```

Use PKIKEY to manage keys
-------------------------

### About PKI formats

The SFTP keys are referenced in the PKI database as a `Keys `identifier.

You can import the following formats in the PKI database:

- Raw DER format
- PEM format for RSA private and public key, beginning with “BEGIN RSA PRIVATE KEY” or “BEGIN RSA PUBLIC KEY”, or X.509 public key, beginning with “BEGIN PUBLIC KEY”
- Encrypted PEM format for RSA private key (PKCS \#5), beginning with "BEGIN RSA PRIVATE KEY " and "Proc-Type: 4,ENCRYPTED"
- PKCS8 format, beginning with “BEGIN PRIVATE KEY” or “BEGIN ENCRYPTED PRIVATE KEY”
- SSH2 format, beginning with “BEGIN SSH2 PUBLIC KEY”
- ssh-rsa format, beginning with “ssh-rsa”

> **Note**
>
> Note: When using the ssh-keygen tool, keys are usually generated in encrypted PEM format, which you can import using the PKIKEY command.

****Restrictions****

{{% TransferCFT/snippets/pkikey_restrictions%}}

### PKIKEY command parameters

{{% TransferCFT/snippets/pkikey_def%}}

### Import existing keys

{{% TransferCFT/snippets/pkikey_examples%}}

Example to create and import a private key
------------------------------------------

This section describes how to create a private key for a user, import the key in the local PKI base, and deploy the associated public key on the SFTP server. In the following examples, the user is called `user2`.

### Example using ssh-keygen

1. Generate a public/private key pair:  
    ```  
     > ssh-keygen -t rsa -b 2048 -f user2
    ```
1. Import the private key in the local PKI base:  
    ```
    PKIUTIL pkikey id='USER2@localhost', ikname='user2', ikpassw='<passphrase>', ikform='pem'
    ```
1. Deploy the public key on the SFTP server. On a {{< TransferCFT/axwayvariablesComponentLongName  >}} server, use the following command to import the key:  
    ```  
     > PKIUTIL pkikey id='USER2', ikname='user2.pub', ikform='ssh'
    ```

### Example using OpenSSL

1. Generate the private key:  
    ```  
     > openssl genrsa -out private.key 2048
    ```
1. Generate the associated public key:  
    ```  
     > openssl rsa -pubout -in private.key -out public.key
    ```
1. Import the private key in the local PKI database:  
    ```  
     > PKIUTIL pkikey id='USER2@localhost', ikname='private.key', ikform='pem'
    ```
1. Deploy the public key on the SFTP server. On a {{< TransferCFT/axwayvariablesComponentLongName  >}} server, use the following command to import the key:  
    ```  
     > PKIUTIL pkikey id='USER2', ikname='public.key', ikform='pem'
    ```

Activate/deactivate a key
-------------------------

Use the `ACT/INACT` commands to activate or deactivate, respectively.

****Example****

Use the LISTPKI command to list available keys:

```
>LISTPKI
Keys:
Id.       S K Bits
---------------- - - ----
USER2@LOCALHOST A x 2048
USER2           A   2048
 
PKIU00I LISTPKI _ Correct ()
```

****Example****

This example demonstrates key deactivation where **I** indicates [INACT] and **A** indicates [ACT].

```
>listpki
Keys:
Id.                 S  K  Bits
---------------- - - ----
USER2@LOCALHOST A
x 2048
USER2             A
     2048
 
>inact type=key
 
>listpki
Keys:
Id.               S  K  Bits
---------------- - - ----
USER2@LOCALHOST I
x  2048
USER2             I
         2048
```
