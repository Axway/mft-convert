{
    "title": "Upload certificates",
    "linkTitle": "Upload certificates",
    "weight": "270"
}This procedure describes how to import certificates (PEM, DER, P12, KEY) to your OpenVMS system.

1. Use FTP to upload your certificates to the OpenVMS system in binary mode.
1. Set the attributes associated with certificates and keys using the command in the following example:  
    ```
    SET FILE/ATTRIB=
    (LRL:4096,RFM:UDF) ROOT.der
    ```
1. Once you have converted all of the certificates and keys, you can create your PKI database as described in the next section, *Creating an internal datafile for certificates.*
