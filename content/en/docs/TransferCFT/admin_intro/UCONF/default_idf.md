{
    "title": "Deactivate the default IDF ",
    "linkTitle": "Deactivating the default IDF",
    "weight": "330"
}You can deactivate the usage of the default IDF defined in CFTPARM that is used when the IDF in SEND or RECV commands does not exist. In this case, transfers that use nonexistent IDFs will end in error.

To disable the use of the default IDF, set the UCONF value: `cft.default_idf.enable=no`

This value is taken into account after issuing a `reconfig `command or a {{< TransferCFT/axwayvariablesComponentShortName  >}} restart.
