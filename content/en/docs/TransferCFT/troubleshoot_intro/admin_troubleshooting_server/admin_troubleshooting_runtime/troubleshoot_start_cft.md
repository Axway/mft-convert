{
    "title": "Issues when starting Transfer CFT",
    "linkTitle": "Issues when starting",
    "weight": "300"
}Cannot start Transfer CFT
-------------------------

### Bind failed

The following error messages display if two CFTNETs point to the same [CLASS](../../../../c_intro_userinterfaces/command_summary/parameter_intro/class) value.

`CFTN05E bind() failed: EDC8115I Address already in use.`

`CFTI22F ID= PESITANY Register request failure CS=0000045B NET=`

****Resolution****

When using PeSIT with TCP/IP, you cannot use more than one CFTNET that has the same defined network resource (same CLASS parameter value).

You must use the same CFTNET CLASS value in the CFTTCP object so the expected source IP is used for a specific partner.

### Register request failure

The following error messages display if two CFTPROTs point to the same [SAP](../../../../c_intro_userinterfaces/command_summary/parameter_intro/sap) value

CFTI22F CFTPROT=PESITSSL Register request failure CS=

CFTI16F Error code 00000001 _ Register request
