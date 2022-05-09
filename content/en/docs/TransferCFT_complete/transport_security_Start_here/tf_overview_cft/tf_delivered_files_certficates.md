{
    "title": "Delivered files, samples, and certificates",
    "linkTitle": "Delivered files and samples ",
    "weight": "220"
}The samples that are delivered with Transfer CFT use the Transfer CFT preprocessing utility to encode/decode a plaintext file before sending it. This topic describes the various delivered files, samples, and certificates that you can use to complete your {{< TransferCFT/suitevariablesTrustedFileName  >}} implementation.

Content described in this topic:

- [Scripts](#Scripts)
- [Trusted File configuration file](#Trusted)
- [Encoding/decoding samples](#Sample)
- [Sample certificates](#Sample)
- [Trusted File messages](#Messages)
- [Transcoding conversion tables](#Transcod)

Conventions for Transfer CFT with Trusted File content includes:

- $CFTDIRRUNTIME variable
    -   Unix: &lt;CFTDIRRUNTIME&gt;
    -   Windows: %CFTDIRRUNTIME%
- $CFTDIRINSTAL variable
    -   Unix: &lt;CFTDIRINSTALL&gt;
    -   Windows: %CFTDIRINSTALL%

<span id="Scripts"></span>

Scripts
-------

Unix scripts


| Script  | Description  |
| --- | --- |
| &lt;CFTDIRRUNTIME&gt;/exec/tf_decipher.cmd  | Trusted File deciphering script  |
| &lt;CFTDIRRUNTIME&gt;/exec/tf_cipher.cmd  | Trusted File ciphering script  |
| &lt;CFTDIRRUNTIME&gt;/exec/tf_delfile.cmd  | End of transfer procedure to delete the sent ciphered file  |


Windows scripts


| Script  | Description  |
| --- | --- |
| &lt;CFTDIRRUNTIME&gt;/exec/tf_decipher.bat  | Trusted File deciphering script  |
| &lt;CFTDIRRUNTIME&gt;/exec/tf_cipher.bat  | Trusted File ciphering script  |
| &lt;CFTDIRRUNTIME&gt;/exec/tf_delfile.bat  | End of transfer procedure to delete the sent ciphered file  |


<span id="Trusted"></span>

{{< TransferCFT/suitevariablesTrustedFileName  >}} configuration file
--------------------------------------------------------------------------

The ****entities.xml**** file is the Trusted File configuration file containing the certificates for Trusted File. This configuration file is customized during Transfer CFT installation with the supplied default private and public certificates, “user1” and “user2”. If you intent to use your own certificates, update the entities.xml file, located at &lt;CFTDIRRUNTIME&gt;/conf/tf/entities.xml.

If you want to use your own certificates and if the option tf.enablepasswordcipher=yes you have to generate a phassphrase using cfttf function:

passPhrase: For PKCS\#12 files, the password required to access the file.

 

General Usage: CFTTF -pcfg conffile [-plain plainFilename] [-entitiesLocation entitiesLocation]

[-envelope envelopeName] [-plainFileCharset plainCharset] [-plainEncCharset encCharset]

[-messagesPath messagesPath] [-template xmlFilename]

To generate a passphrase, use the command: `CFTTF -pw [password]`

****Example****

```
CFTTF –pw user1OUTPUT: OGrplhngkBLeiazMyPkAdcLnd5jlNOnMoGYKaI2WfAw=
```

See also [How to generate a certificate for Trusted File](../tf_generate_cert).

<span id="Sample"></span>

Sample encoding files
---------------------

The following files refer to “user1” and “user2”, which are used in the supplied private certificate. You must update these users if you intend to use your own certificates. These files are located in: &lt;CFTDIRRUNTIME&gt;/conf/tf/.

Sample file descriptions


| File  | Description  |
| --- | --- |
| encfile_cms.xml  | Sample file used for encoding CMS format  |
| encfile_pgp.xml  | Sample file used for encoding PGP format  |
| encfile_smime.xml  | Sample file used for encoding S/MIME format  |
| decfile_cms.xml  | Sample file used decoding CMS format  |
| decfile_pgp.xml  | Sample file used for decoding PGP format  |
| decfile_smime.xml  | Sample file used for decoding S/MIME format  |


Sample certificates
-------------------

The following certificates are located in: &lt;CFTDIRRUNTIME&gt;/conf/tf/.


| Certificate  | Description  |
| --- | --- |
| &lt;CFTDIRRUNTIME&gt;/conf/tf/certs/priv/xppuser1.p12  | Private delivered “user1” certificate  |
| &lt;CFTDIRRUNTIME&gt;/conf/tf/certs/priv/xppuser2.p12  | Private delivered “user2” certificate  |
| &lt;CFTDIRRUNTIME&gt;/conf/tf/certs/pub/xppuser1.pem  | Public delivered “user1” certificate  |
| &lt;CFTDIRRUNTIME&gt;/conf/tf/certs/pub/xppuser2.pem  | Public delivered “user2” certificate  |


<span id="Messages"></span>

Trusted File messages
---------------------

The following messages are used by Trusted File, and are located in: `$CFTDIRINSTALL/distrib/tf/english/`****.**** Each file contains a set of error message associated with the type of encoding used.

- xasn.msg
- xp3.msg
- xpppki.msg
- pgp.msg
- smime.msg
- xppadm.msg
- xppconf.msg
- xppgen.msg
- xpp.msg
- xppsrv.msg
- xppwrap.msg

Refer to the **Trusted File 3.6 Reference Guide** for details, available on [support.axway.com](https://support.axway.com/).

<span id="Transcod"></span>

Transcoding table
-----------------

The **** `<CFTDIRRUNTIME>/conf/tf/transcoding.tbl` **** file contains all available transcoding tables.


| Table  | Description  |
| --- | --- |
| &lt;CFTDIRINSTALL&gt;/distrib/tf/tables/iso_atoe.tbl  | Converts Latin ASCII to French EBCDIC  |
| &lt;CFTDIRINSTALL&gt;/distrib/tf/tables/iso_etoa.tbl  | Converts French EBCDIC to Latin ASCII  |
| &lt;CFTDIRINSTALL&gt;/distrib/tf/tables/std_atoe.tbl  | Converts IBM-PC850 to French EBCDIC  |
| &lt;CFTDIRINSTALL&gt;/distrib/tf/tables/std_etoa.tbl  | Converts French EBCDIC to IBM-PC850  |

