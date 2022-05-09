{
    "title": "Manage the database ciphering key",
    "linkTitle": "Manage the database ciphering key",
    "weight": "220"
}This topic describes how to manage the ciphering key using the ****cftcrypt**** tool,  which ciphers all passwords. Information includes:

- Overview
- Command description
- Renew a key
- Upgrade a Transfer CFT
    -   Export the configuration    
    -   Generate a key
    -   Import the configuration

Overview
--------

All passwords stored in the UCONF dictionary, or in the {{< TransferCFT/axwayvariablesComponentLongName  >}} databases (for example, CFTPART, CFTPARM) are cyphered using the key generated at installation. If you are performing an upgrade, all passwords are cyphered using a hard-coded key. We recommend that you generate an encryption key as described in [Generate an encryption](#Generate).

> **Note**
>
> Note: The encryption key is mandatory to use the embedded SecureRelay.

cftcrypt command
----------------

****Syntax****

`cftcrypt -- ACTION -- OPTION`

Actions:

- genkey: Generate an encryption key using a password.
- renewkey: Regenerate an encryption key using a new password along with the old password.
- help: Display this help.

Genkey options:

- keyfname: File where the generated key is stored.
- saltfname: File where the computed salt is stored.
- pass: Password.

Renewkey options:

- pkitmp: Temporary file where the PKI configuration is stored.
- pkipref : Prefix for certificates temporary files (z/OS only, on all other systems the non modifiable `cftcrtmp ` value is the prefix, for example, an exported certificate could have a name such as "cftcrtmpROOT0001").
- noremove: Do not remove the temporary file; you can use this to keep track of what has been exported.
- nopki:  Do not export/import the PKI base.
- pass: Password.
- oldpass: Old password.
- tmpfname: Temporary file where the Transfer CFT configuration is stored.

> **Note**
>
> Note: Passwords must contain at least 8 characters, lower case, upper case, numeric and special characters(\*\#$!?+-@).

Use the following command to generate an encryption key using the provided password. This creates the `--keyfname` and `--saltfname` files, and references them in UCONF.

```
cftcrypt --genkey --keyfname FILENAME --saltfname FILENAME --pass PASSWORD
```

****Encryption parameters in UCONF****


| Parameter  | Description  |
| --- | --- |
| crypto.key_fname  | The path to the encryption key, which is required at {{< TransferCFT/axwayvariablesComponentLongName  >}} runtime.<br/> If this parameter is not set (empty), at runtime {{< TransferCFT/headerfootervariableshflongproductname  >}} uses a hard-coded key and operates as in {{< TransferCFT/headerfootervariableshflongproductname  >}} 3.2.4. |
| crypto.salt_fname  | The path to the salt file, which is required to renew the key.  |


Renew the encryption key
------------------------

To renew an encryption key run the `renewkey` command:

```
cftcrypt --renewkey --keyfname FILENAME --saltfname FILENAME --oldpass PASSWORD --pass PASSWORD
```

The command succeeds if the referenced key and salt files exist and the `oldpass `matches the password used to generate the previous key. This command exports the configuration, generates a new encryption key, and imports the configuration.

****Platform specifics****

- z/OS: Use the JCL CFTRNKEY for key renewal.

### Troubleshoot the key renewal

The cftcrypt tool automatically exports/imports the PKI database when you perform a key renewal. However, to avoid potential issues, the process stops and an error is returned if an issue is detected during the PKI export/import.

- To perform a key renewal with an empty PKI base or without a PKI file, you can use: ` cftcrypt --renewkey --nopki `
- To facilitate debugging, you can use the `--noremove` option, so that the temporary files used by cftcrypt are not deleted. See the `cftcrypt --help `for more information.

<span id="Generate"></span>

Generate an encryption key following an upgrade
-----------------------------------------------

It is highly recommended that you generate an encryption key when you upgrade a Transfer CFT 3.2.4 or lower to Transfer CFT 3.3.2 or higher. For example:

1. From the {{< TransferCFT/headerfootervariableshflongproductname  >}} runtime directory, perform the extract to export the configuration.  
    ```
    CFTUTIL cftext fout=cft332.cfg
    ```
1. Export the PKI database.  
    ```
    PKIUTIL PKIEXT FOUT=PKI.EXT
    ```
1. Generate a new encryption key.  
    ```
    cftcrypt --genkey --keyfname data/crypto/cryptkey --saltfname data/crypto/cryptsalt --pass MyPassword1\*
    ```
1. Import the PKI database.  
    ```
    PKIUTIL @ PKI.EXT
    (or PKIUTIL \# PKI.EXT depending on your system)
    ```
1. Import the configuration.  
    ```
    CFTUTIL config type=input,fname=cft332.cfg
    ```

> **Note**
>
> Note: On z/OS platforms, use the JCL CFTGNKEY to generate the key.
