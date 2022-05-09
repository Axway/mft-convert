{
    "title": "Creating an internal datafile for certificates ",
    "linkTitle": "Creating a datafile for certificates",
    "weight": "280"
}An indexed file, CFTPKI.INX certificate database, is included in the product delivery.

To create another database, you must activate the PKIFILE.EXE program with the following command, which can create the envelope of the indexed file containing the certificates:

```
PKIFILEMODE=CREATE,
          FNAME= '&PKIVOL:&PKIDVC:&PKIFIC'
```

Generating the certificate internal datafile
--------------------------------------------

You can generate the certificate internal datafile from the delivered \*.DER files. This utility interprets a command file. An example is delivered in the CFT_PKI: directory.

```
PKIUTIL \#CFT-PKI.CONF --> creates PKI base and inserts the certificates
PKIUTIL LISTPKI --> lists the inserted certificates
```
