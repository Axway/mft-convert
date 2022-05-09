{
    "title": "Microsoft Azure Blob Storage",
    "linkTitle": "Azure Blob Storage",
    "weight": "190"
}You can use Transfer CFT with Microsoft Azure Blob Storage to store and retrieve large numbers of files to better manage enterprise data.

Transfer CFT implements Microsoft Azure Blob Storage services using ...

Limitations
-----------

 

<span id="Set"></span>

Set up the connection
---------------------

Parameter description
---------------------

The following table describes Transfer CFT's Microsoft Azure Blob Storage-related parameters.


| Parameter  | Type  | Description  |
| --- | --- | --- |
| ssl.certificates.ca_cert_bundle  | string  | Path to the CA certificate bundle.<br/> This path can point to either a file containing the CA certificates (for example, <code>/etc/ssl/certs/ca-certificates.crt</code>) or to a directory containing the CA certificates (for example, <code>/etc/ssl/certs/</code>), which are stored individually with their filenames in a hash format.<br/> <blockquote> **Note**<br/> Note: Please refer to the cURL man page for information on the cacert and capath options. If the certificate bundle is not available on your system, you can download it from: curl.haxx.se/docs/caextract.html (download from cacert.pem).<br/> </blockquote>  |


Creating send and receive definitions
-------------------------------------

You must include the following parameters in your Google Cloud Storage [CFTSEND/CFTRECV](../../c_intro_userinterfaces/command_summary) definitions:


| Parameter<span id="storageaccount"></span>  | Type  | Description  |
| --- | --- | --- |
| fname  | string  |   |
| workingdir  | string  |   |
| wfname  | string  |   |


Use case examples
-----------------

### Configure the connection

### Including subdirectories

espect the same behavior than the classic functionnally with classic files systems.

The UCONF parameter cft.dirdepth impacts the behavior:

- yes (default value), CFT + Group of files take all subdirectories in the transfer operation
- no , CFT + Group of files don't take subdirectories in the transfer operation

### The Transfer CFT stores a received file on Microsoft ABS

Transfer CFT  receives a file from a partner over the PeSIT protocol and stores it on Microsoft ABS.

 

Configure the CFTRECV object to write to the ABS:

```
CFTRECV
```

After the partner sends a file, you can check the log for transfer details.

### Transfer CFT reads a file from ABS and sends it to a partner

Here, Transfer CFT reads a file from ABS and sends it over PeSIT to a partner.

 

Create the CFTSEND template, and send a file that ABS to a Transfer CFT partner.

```
CFTSEND id=G
```

After sending a file to the partner, you can check the log for transfer details.

Troubleshooting
---------------

This section provides information on how to troubleshoot errors that you may encounter when implementing ABS with Transfer CFT.

### Transfer CFT logs and diagnostic codes
