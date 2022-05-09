{
    "title": "Transfer diagnostic codes",
    "linkTitle": "Transfer error codes",
    "weight": "280"
}This section describes the types of diagnostic codes that may display as the result of an error relative to a transfer.

Diagnostic codes categories
---------------------------

{{< TransferCFT/axwayvariablesComponentLongName  >}} provides three types of diagnostic codes, DIAGI, DIAGP, and DIAGC, that are intended to help you understand encountered issues.

- DIAGI codes provide internal error information for both local and remote problems. If the code is specific to a protocol, this
    is indicated in the code.
- DIAGP codes add protocol diagnostics concerning the error.
- DIAGC codes are complementary diagnostics that provide additional information. DIAGC codes are stored in the catalog record as a string (255 characters) that is accessible using the &diagc symbolic variable. These codes also sometimes displays in log messages.
