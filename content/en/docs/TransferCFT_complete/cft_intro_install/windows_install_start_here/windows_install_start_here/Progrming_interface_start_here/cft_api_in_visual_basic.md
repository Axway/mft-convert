{
    "title": "Building  API in Visual Basic",
    "linkTitle": "Building API in Visual Basic",
    "weight": "250"
}The {{< TransferCFT/axwayvariablesComponentShortName  >}} API toolkit contains a dynamic library, which enables
users to develop a {{< TransferCFT/axwayvariablesComponentShortName  >}} API application, that was itself developed
in Visual Basic. This library and the sample supplied have been validated
for Visual Basic version 4.0 and higher.

The {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs enable you to implement the facilities
described in [About API](../../../../about_this_document_zos/using_apis).

- Cftai:
    catalog consultation
- Cftau:
    transfer commands with syntactical analysis
- Cftac:
    transfer commands without syntactical analysis

The {{< TransferCFT/axwayvariablesComponentShortName  >}} API Visual Basic call structures and return
codes are the same as for the equivalent functions for the {{< TransferCFT/axwayvariablesComponentShortName  >}}
API functions in C. For more information, refer to [Using {{< TransferCFT/axwayvariablesComponentShortName  >}} services in C](../../../../about_this_document_ibmi/using_apis/using_cft_services_in_c).

### Visual Basic toolkit

{{< TransferCFT/axwayvariablesComponentShortName  >}} Windows provides:

- a sample of a Visual
    Basic program
- a dynamic library
    cftvb.dll to allow interfacing between {{< TransferCFT/axwayvariablesComponentShortName  >}}/Windows
    XXX with a Visual Basic application
- a file
    cftapivb.bas containing the statements necessary to use the cftvb.dll
    dynamic library

The cftvb.dll dynamic library:

- must be visible
    to all Visual Basic applications using the {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs
- itself uses
    the dynamic library apicft.dll (programming interface in C) supplied
    with {{< TransferCFT/axwayvariablesComponentShortName  >}}/Windows, and must therefore be visible to that
- exports the
    functions set out below

The source Visual Basic file cftapivb.bas:

- contains the
    statements of structure and of functions required to use the Transfer
    CFT APIs in a Visual Basic program
- must be incorporated
    into all projects using the Visual Basic {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs

Available Visual Basic functions
--------------------------------

### Initializing and closing a {{< TransferCFT/axwayvariablesComponentShortName  >}} API application in Visual Basic

****Cft_Api_Open (ByVal Version As String) As Integer****

API initialization.

This function must be called before any other {{< TransferCFT/axwayvariablesComponentShortName  >}} API function.

The parameter is the constant CFT_API_Version as defined in cftapivb.bas.

The return code is 0 if the APIs have correctly initialized.

****Cft_Api_Close () As Integer****

API close.

This function must be the last {{< TransferCFT/axwayvariablesComponentShortName  >}} API function called before
the application closes.

The return code is 0 if the APIs have correctly closed.

### Interrogating the catalogue in Visual Basic

****Cft_Cat_Open (ByVal Catalog As String) As Integer****

To open the catalog.

The parameter is the name of the catalog file.

The return code is 0 if the catalog is opened. If it does not, see
the cftai function (OPEN).

****Cft_Clear_Sel (CftSel As CftSelT) As Integer****

Initializes a CftSelT structure by filling this structure with binary
zeros.

This function must be called before defining the selection criteria
to call the Cft_Cat_Select function.

The parameter is the CftSelT structure that requires cleaning.

The return code is 0.

****Cft_Cat_Select (CftSel As CftSelT) As
Integer****

Selection of records within the open catalog.

The parameter is a correctly filled CftSelT structure (see the Cft_Clear_Cat
function).

The return code is 0 if everything is correct. If not, refer to the
cftai C function (SELECT).

****Cft_Clear_Cat (CftCat As CftCatT) As Integer****

Initializes a CftCatT structure by filling this structure with binary
zeros.

This function must be called after every call to Cft_Cat_Next.

The parameter is the CftCat structure that will be used to call Cft_Cat_Next.

The return code is 0.

****Cft_Cat_Next (CftCat As CftCatT) As Integer****

To read a selected record in the catalogue.

The parameter is an empty CftSelT structure (see the Cft_Clear_Cat function).

The return code is 0 if a record has been raised. If it is not, see
the cftai C function (NEXT).

****Cft_Cat_Modify (ByVal CatState As String)
As Integer****

To modify the status of the last record read in the catalogue.

The parameter is a character designating the new status.

The return code is 0 if the status of the record has been modified,
otherwise see the cftai C function (MODIFY).

****Cft_Cat_Close () As Integer****

To close the catalogue.

The return code is 0 if the catalog has closed. If not, refer to
the cftai C function ("CLOSE").

### Transfer commands in Visual Basic

****Cft_Cmd_Analyse (ByVal Cmd As String, ByVal Parameter As String) As
Integer****

To send a Cft command with syntactical analysis.

For parameters and return codes (see the cftau function).

****Cft_Cmd (ByVal Cmd As String, ByVal Parameter
As String) As Integer****

To send a Cft command without syntactical analysis.

For parameters and return codes (see the cftac function).
