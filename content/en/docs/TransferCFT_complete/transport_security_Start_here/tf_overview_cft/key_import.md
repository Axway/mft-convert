{
    "title": "Import and export keys and certificates",
    "linkTitle": "Import and export keys and certificates",
    "weight": "240"
}Import keys and certificates
----------------------------

You can use the import feature to convert your OpenPGP certificates
and private keys or a trading partner's certificates to the X.509/PKCS\#12
format.

You can convert:

- PGP Public
    Keyrings to X.509 Certificates
- PGP Secret
    Keyrings to PKCS\#12 packages

### Import procedure

You can convert a PGP public Keyring file to one or several X.509certificate(s) and a PGP secret Keyring file to one or several PKCS\#12 package(s). If no options are provided, the .p12 file or the cert file is stored in the current directory. The private keys are assumed not password protected and a default subject DN is selected for you.

When you import a private keyring file containing ElGamal subkey(s), you must provide the PKCS\#12 file for signing (as in public keyring import), because the ElGamal keys cannot be used for signing when you create a certificate.

1. Use CFTUTIL to set the full path to Java executable`:`  
    ```
    UCONFSET id=cft.jre.java_binary_path ,value=/bin/java
    ```
1. Enter the import command:

- UNIX: ImportPGPKey.sh
- Windows: ImportPGPKey.bat

There are several options you can add to the command line:


| Options  | Description  |
| --- | --- |
| -h or -help  | Displays the help with all available options.  |
| -dn  | Subject DN of the generated certificate. If this option is not provided, the default subject DN is CN=PGP.  |
| -out  | Output path of the generated certificate or PKCS#12 file. If this option is not provided, the file is stored in the current directory. It must contain the tailing file separator („\‟ on Windows and „/‟ on UNIX). |
| -passwd  | Private key PassPhrase of the secret Keyring located in the input file.  |
| -newpasswd  | Private key PassPhrase of the generated PKCS#12 file. This option is mandatory for imported secret Keyrings.  |
| -pkcs12  | PKCS#12 file used to sign the new generated certificate.  |
| -passPkcs12  | Passphrase of the PKCS#12 file used to sign the new generated certificate.  |


### Example commands

The following examples use the UNIX command extension (.sh). On Windows systems replace .sh with .bat in your command.

`ImportPGPKey.sh -dn "cn=Axway,c=fr" -pkcs12 /home/user/pkcs12.p12 -passPkcs12 "passphrasePKCS12" -out /home/user/output/ /home/user/pubring.pkr`

This example stores the certificate provided by the public Keyring `home/user/pubring.pkr` in the `/home/user/output/` directory. The subject DN of the certificate generated will be "cn=Axway,c=fr".

The private key used to sign the certificate is stored in the PKCS\#12 file located in the `/home/user/pkcs12.p12` directory.

The PassPhrase of this PKCS\#12 file is "passphrasePKCS12".

`ImportPGPKey.sh -passwd "pass" -newpasswd "newpasswd" -out /home/user/output/`

`/home/user/secring.skr`

This example stores the private key provided by the secret Keyring `home/user/secring.skr` in the` /home/user/output/` directory.

The PassPhrase of the secret Keyring is "pass". The PassPhrase of the generated PKCS\#12 file is "newpasswd".

Export keys and certificates
----------------------------

This feature allows you to convert a PKCS\#12 package to a PGP Public Keyring file and/or a PGP Secret Keyring file.

For the ElGamal keys stored in a PKCS\#12 file, it is mandatory to provide a signing certificate and corresponding password. ElGamal keys cannot be used for signing or as a master key for a created keyring.

TrustedFile exports keys and certificates with the
following file names.


| Key/certificate standard  | Exported file name  |
| --- | --- |
| PGP Public Keyring | **useridpacket**_pub.asc |
| PGP Secret Keyring and Public Keyring | **useridpacket**_sec.asc and **useridpacket**_pub.asc |
| X.509 Certificate | **certificate alias.**der |


### Export procedure

1. Use CFTUTIL to set the full path to Java executable`:`  
    ```
    UCONFSET id=cft.jre.java_binary_path ,value=/bin/java
    ```
1. Enter the import command:

- UNIX: ExportPGPKey.sh
- Windows: ExportPGPKey.bat

There are several options you can add to the command line:


| Option | Description |
| --- | --- |
| -help &#124; -h | Displays the help. |
| -out | Output path (with the trailing path separator). If this option is not provided, the file is stored in the current directory. |
| -passSecRing | PassPhrase of the generated secret Keyring. |
| -userIdPacket | User ID packet of the generated secret Keyring and public Keyring. |
| -passPkcs12 | PassPhrase of the PKCS#12 file to be converted. |
| -secRing | With the argument &quot;no&quot;, the secret Keyring will not generate.<br/> Do<br/> not use this option or use an alternative argument. |
| -pubRing | With the argument &quot;no&quot;, the public Keyring will not generate. Do not use this option or use an alternative argument. |
| -masterPkcs12 | (only for ElGamal) PKCS#12 file used to sign the new generated certificate. |
| -masterPassPkcs12 | (only for ElGamal) Passphrase of the PKCS#12 file used to sign the new generated certificate. |


> **Note**
>
> Note: The following options are required: -userIdPacket and -passSecRing. By default (without -secRing or -pubRing options), the feature exports Public Keyring and Secret Keyring.If you do not need to export the Secret Keyring, add - secRing no to the command line.If you do not need to export the Public Keyring, add - pubRing no to the command line.

### Example commands

The following examples use the UNIX command extension (.sh). On Windows systems replace .sh with .bat in your command.

`ExportPGPKey.sh -userIdPacket "userIdPacket" -passPkcs12 "passPkcs12"`

`-passSecRing "passSecRing" -out /home/user/output/`

`/home/user/Pkcs12ToConvert.p12`

This example stores the secret Keyring and the public Keyring provided by the file `Pkcs12ToConvert.p12` in the `/home/user/output/` directory.

The PassPhrase of `Pkcs12ToConvert.p12` is "passPkcs12". The user ID packet in the generated files is "userIdPacket". The generated secret Keyring is protected by the PassPhrase "passSecRing".

`ExportPGPKey.sh -userIdPacket "userIdPacket" -passPkcs12 "passPkcs12"`

`-passSecRing "passSecRing" -out /home/user/output/ -secRing no`

`/home/user/Pkcs12ToConvert.p12`

The example is the same as above, with the `-secRing no` option. Only the public Keyring is generated.
