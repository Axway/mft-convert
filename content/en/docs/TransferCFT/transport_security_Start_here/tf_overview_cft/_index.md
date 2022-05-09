{
    "title": "About Trusted File for file encryption",
    "linkTitle": "Using TrustedFile for file encryption",
    "weight": "200"
}Encryption concepts
-------------------

This topic describes how to implement Axway Trusted File encoding. {{< TransferCFT/axwayvariablesComponentShortName  >}} in conjunction with TrustedFile enables you to send encrypted files in S/MIME, CMS, and OpenPGP format, for increased security for data exchanges. To initiate this additional security, Transfer CFT delivers a set of samples and certificates to implement TrustedFile in your environment. To get started using TrustedFile with Transfer CFT, read the following sections:

- [Before you start](#Before)
- [About the Transfer CFT configuration file](#Transfer)
- [Defining the unified configuration parameters](#Defining)
- [Command example (to encode/decode a file)](#Command)

The topic [Delivered files and certificates](tf_delivered_files_certficates) describes the scripts, conversion tables, files and samples delivered with Transfer CFT. And for more information on TrustedFile functionality, please refer to the [TrustedFile Reference Guide]() .

#### Limitations

- You cannot use {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} to manage {{< TransferCFT/suitevariablesTrustedFileName  >}} functionality with {{< TransferCFT/axwayvariablesComponentShortName  >}}.

<span id="Before"></span>

Before you start
----------------

In Transfer CFT you use the CFTTF utility, referred to as XPPTF in TrustedFile, to perform secured exchanges. To use this functionality your Transfer CFT key must include the Trusted File option. The Transfer CFT key is located in the file: `$CFTDIRRUNTIME/conf/cft.key`. If your product key does not include the Trusted File option if you try to execute the CFTTF program, an error will occur and an error message is displayed in the CFTLOG file: `CFTR19E XPPCFG_Error_#20:_Invalid_product_key`.

Transfer CFT delivers useable examples that automatically implement TrustedFile in your preprocessing and post processing flow. The next section describes the delivered samples.

<span id="Transfer"></span>

Understanding the delivered sample configuration file
-----------------------------------------------------

The Transfer CFT sample configuration file `runtime/conf/cft-tf-smp.conf` includes the TrustedFile IDF as shown here.

- The delivered procedures are called during the preprocessing phase to encode the file (tf_cipher.cmd), and delete the encoded file after sending (tf_delfile.cmd).
- The post processing script decodes on the receiving side (tf_decipher.cmd).

****Sending****


| cftsend id = trusted_file, &lt;br/&gt;ftype = B, &lt;br/&gt;fcode = BINARY, &lt;br/&gt;fname = pub/FTEST, &lt;br/&gt;EXEC = $CFTDIREXEC/tf/tf_delfile.cmd, &lt;br/&gt;PREEXEC = $CFTDIREXEC/tf/tf_cipher.cmd,<br /> mode = replace  |
| --- |


****Receiving****


| cftrecv id = trusted_file, &lt;br/&gt;ftype = B,<br /> fcode = BINARY, &lt;br/&gt;fname = $CFTDIRPUB/TF_&amp;part_&amp;idtu,<br /> EXEC = $CFTDIREXEC/tf/tf_decipher.cmd,<br /> faction = delete ,<br /> fdisp = both, mode = replace  |
| --- |


<span id="Defining"></span>

Defining the unified configuration parameters
---------------------------------------------

The Transfer CFT installation process automatically sets the following Transfer CFT unified configuration parameters to enable {{< TransferCFT/suitevariablesTrustedFileName  >}} functioning. For information on uconf, see [About the Unified Configuration](../../admin_intro/uconf).


| Parameter (uconf)  | Default values  | Description  |
| --- | --- | --- |
| tf.proofslocation  | &lt;HOME&gt;/Axway/Transfer_CFT/runtime/data/tf  | References the absolute path to the directory that the product uses to generate proofs  |
| tf.proofsenabled  | yes  | Indicates whether proofs are enabled or not. This field takes the value yes or no (yes by default). If the value is set to no, the generation of proofs is deactivated  |
| tf.messageslocation  | &lt;HOME&gt;/Axway/Transfer_CFT/home/distrib/tf/english  | Transfer CFT runtime directory  |
| tf.entitieslocation  | $HOME/Axway/Transfer_CFT/runtime/conf/tf/entities.xml  | Indicates the TrustedFile configuration path.<br/> If the ****tf.entitieslocationtype**** is:<br/> • Local: Points locally to the entities.xml file by default |
| tf.entitieslocationtype  | local  | Defines the type of TrustedFile configuration. The configuration path is defined in ****tf.entitieslocation****.<br/> • Local: Indicates that Trusted File is configured in standalone mode (locally) |
| tf.defaultlocalcharset  | ISO-8859-1  | Default character set for the platform  |
| tf.transcodingtablelocation  | &lt;HOME&gt;/Axway/Transfer_CFT/runtime/conf/tf/transcoding.tbl  | Absolute path to the character set conversion reference table  |
| tf.overwritemode  | enable  | Defines how Axway TrustedFile behaves when it must open an existing plain file, acknowledgement or envelope in write mode. If this element is set to the value yes or enable, Axway TrustedFile overwrites the existing output files. Otherwise, it does not open the files and interrupts the current operation with an error message. Its default value is enable  |
| tf.enablepasswordcipher  | yes  | Indicates that entities passphrases, either in the entities definition file (entities.xml) or in the operation description file, are stored in a ciphered format.  |


<span id="Command"></span>

Command example
---------------

Use the SAPPL variable to define the type of security format to use to encode the file. Note that you must
use the same security format as your partner (possible values are CMS, PGP, and S/MIME).

****Example****

To encode/decode messages using PGP, use the format:

```
CFTUTIL send part=loop,idf=trusted_file, SAPPL=pgp
```

Not supported
-------------

When using {{< TransferCFT/suitevariablesTrustedFileName  >}} with Transfer CFT, some {{< TransferCFT/suitevariablesTrustedFileName  >}} functionalities are not delivered or available as described in the {{< TransferCFT/suitevariablesTrustedFileName  >}} documentation. These include:

- Overview: Graphical user interface (not delivered in the package)
- Configuration sample: Encoding and decoding files with Java API
- C-API

****Related topics****

- [SAPPL](../../c_intro_userinterfaces/command_summary/parameter_intro/sappl)
- [Delivered files and certificates](tf_delivered_files_certficates)
- [How to generate a certificate for {{< TransferCFT/suitevariablesTrustedFileName  >}}](tf_generate_cert)
