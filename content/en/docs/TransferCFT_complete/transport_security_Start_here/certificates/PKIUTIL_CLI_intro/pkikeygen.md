{
    "title": "Using PKIKEYGEN",
    "linkTitle": "Using PKIKEYGEN",
    "weight": "270"
}The PKIUTIL command PKIKEYGEN can generate a new RSA key pair (PKIKEY) and inserts it in the Transfer CFT PKI database, which you can use when implementing [SFTP](../../../../protocols_start_here/sftp_intro).


| Parameter  | Values  | Description  |
| --- | --- | --- |
| PKIFNAME  | string  | Transfer CFT PKI database file name  |
| ID  | string, 32 char  | PKIKEY identifier  |
| COMMENT  | string  | A free comment  |
| STATE  | ACT, INACT | State of the key |
| KEYLEN  | 1024, 2048, or 4096  | Length of the generated key  |
| MODE  | CREATE, REPLACE an existing PKIKEY  | Action on the database  |


****Procedure****

The following example creates both a private and public key. You can then export the public key to a remote Transfer CFT, or to another product that is using SFTP.

1. Use PKIKEYGEN to generate the key pair.  
    ```
    PKIUTIL PKIKEYGEN
    ID=KEY_2048,
    PKIFNAME=$CFTPKU,
    STATE=ACT,
    KEYLEN=2048,
    MODE=CREATE,
    COMMENT="2048-bits RSA key"
    ```
1. Export the SSH key from the Transfer CFT PKI database. See also [PKIEXT](../pkiext).  
    ```
    PKIUTIL PKIEXT FOUT=KEY_2048.CFG, ID=KEY_2048, TYPE=KEY
    ```
1. Note the IKNAME value (KPRIVxxxx where xxxx is 4 numeric values) found in the KEY_2048.CFG file.
1. Locate the KPUBxxxx file, which is in the same folder as the extracted PKI information (KEY_2048.CFG).
1. Use the same four digits as in the IKNAME to locate the KPUB file. For example, if the IKNAME is KPRIV1234 the KPUB that you need is KPUB1234.  
    This public key is in SSH-RSA format and can be used on other SFTP clients.
1. On the remote {{< TransferCFT/axwayvariablesComponentLongName  >}} import the public key, KPUB1234 in our example, using the PKIKEY command.  
    ```
    PKIUTIL PKIKEY ID= KPUB1234
    , ikname= KPUB1234
    , ikform=SSH, MODE=REPLACE
    ```
