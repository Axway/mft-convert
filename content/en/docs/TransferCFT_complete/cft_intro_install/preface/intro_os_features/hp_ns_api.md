{
    "title": "Transfer CFT APIs",
    "linkTitle": "Transfer CFT APIs",
    "weight": "240"
}You can use Transfer CFT APIs either on HP NonStop, or via the native system. To use APIs with HP NonStop, refer to the services described in the Transfer CFT User Guide for Unix platforms.

On a Guardian platform, {{< TransferCFT/axwayvariablesComponentLongName  >}} APIs are available through a server. However, only one server can access the {{< TransferCFT/axwayvariablesComponentLongName  >}} catalog file at a time. This means that if an application is working with the catalog, the other applications are notified via a report and are set to pending. We therefore recommend using more than one server to avoid this limitation.

A Transfer CFT can be assigned up to nine servers that are named using the convention $xxAPn, where:

- $xx: is the {{< TransferCFT/axwayvariablesComponentLongName  >}} prefix selected at installation
- n: is a number between 1 and 9

These servers use a standard inter-process message exchange mechanism.

For more information on how to use Transfer CFT API services, refer to the *Transfer CFT {{< TransferCFT/headerfootervariableshfversion  >}}{{< TransferCFT/suitevariablesDocTypeUser  >}}*. An additional {{< TransferCFT/axwayvariablesComponentLongName  >}} API service called SRVEND is provided for Transfer CFT Guardian, which requests a server shutdown.

Managing the API servers
------------------------

To start a server, you can use the CFTAPI TACL located in $volume.&lt;subvolume&gt;IX. This TACL starts the next server in the list beginning with 1.

To stop a server, use the {{< TransferCFT/axwayvariablesComponentLongName  >}}-specific SRVEND service, or use the STOP process command.

Both the API01 and API02 samples include the SRVEND service. For example:

```
API02 $xxAP1 SRVEND
```

Messages exchanges
------------------

The message structures are described in DDL and C language:

- The C files is derived from the DDL file
- Both the DDL and C header files are located in `$volume.<subvolume>IH`
- Except for the Guardian-specific message number at the beginning of the structure, the message information is the same as for other platforms

Installation files
------------------

The following files are part of the installation:

- APIDDL: The DDL file describing the message structure
- APIC: The C header file resulting from the APIDDL compilation
- API01, API02: The sample C programs
