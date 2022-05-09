{
    "title": "Transfer CFT environment directory",
    "linkTitle": "Transfer CFT environment directory",
    "weight": "240"
}The D$CFT_RUN:[DATA] =&gt; CFT_DAT logical directory is used to store the files needed for the Transfer CFT environment, as well as the data files used by the XLATE.COM utility.

As described in [Generating a test Transfer CFT](../t_generate_test_cft), the following files are created by running CFTDAT in CFTUTIL:

```
cftinit cft_scen: cft-tcp.conf cft_scen: cft-tcp-part.conf
```


| File  | Contents  |
| --- | --- |
| CFTUCONF.DAT  | The Transfer CFT client configuration file. This file is delivered in the installation, and then customized by the user during installation. |
| CFTCATA.REL | The Transfer CFT CATALOG file.<br />  |
| CFTCOM.REL | The Transfer CFT COMMUNICATION file.<br />  |
| CFTPARM.INX | The Transfer CFT PARAMETER file.<br />  |
| CFTPART.INX | The Transfer CFT PARTNER file.<br />  |


### Certificate directory

This directory contains the certificates (\*.DER, \*.P12) is: D$CFT_RUN:[CONF.PKI] =&gt; CFT_PKI logical directory.

### {{< TransferCFT/axwayvariablesComponentShortName  >}} log file directory

The D$CFT_RUN:[LOG] =&gt;CFT_LOG logical directory is used to store Transfer CFT log files.


| File  | Contents  |
| --- | --- |
| CFTLOG.LOG | The Transfer CFT LOG file. This file is not supplied. It is created by CFTUTIL in the CFTDAT procedure. |


### Transfer CFT log account directory

The D$CFT_RUN:[ACCNT]=&gt; CFT_ACCNT logical directory is used to store Transfer CFT account files.


| File  | Contents  |
| --- | --- |
| CFTACCNT.LOG  | Transfer CFT ACCOUNT file.<br/> This file is not supplied. It is created by CFTUTIL in the CFTDAT procedure. |
| CFTACCNT.LOGA  | Transfer CFT ACCOUNT file.<br/> This file is not supplied. It is created by CFTUTIL in the CFTDAT procedure. |


### Files to send directory

The D$CFT_RUN:[PUB] =&gt; CFT_SEND logical directory used to store the files to be sent in the sample configurations.


| File  | Contents  |
| --- | --- |
| TEST.SND | Variable sequential file. |
| FTEST.SND  | Variable sequential file. |


### Rebuild {{< TransferCFT/axwayvariablesComponentShortName  >}} directory

The D$CFT_INST:[LIB]=&gt; XMKT_BDIR:[LIB] directory contains the libraries and objects required to rebuild Transfer CFT.
