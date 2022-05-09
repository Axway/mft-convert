{
    "title": "About Windows Application Programming Interfaces (API)",
    "linkTitle": "Building APIs and exits",
    "weight": "230"
}This section describes the application build features of the {{< TransferCFT/axwayvariablesComponentShortName  >}}
Windows Application Programming Interfaces, and introduces concepts
including:

- [Product
    tool kit](#Product_toolkit)
- [Before
    developing your first CFT API application](#Before_developing_your_first_CFT_API_application)
- [Design
    constraints](#Design_constraints)
- [Coding
    constraints](#Coding_constraints)
- [Compilation
    constraints](#Compilation_constraints)
- [Linking
    constraints](#Linking_constraints)
- [Execution
    constraints](#Execution_constraints)

After installation, the` runtime\src` directory contains the API sources developed in C, Visual Basic, or DELPHI (capi, delphi, vb subfolders).

<span id="Product_toolkit"></span>

### Product toolkit

The product comes with a tool kit that enables you to develop an application
using the {{< TransferCFT/axwayvariablesComponentShortName  >}} programming interfaces, {{< TransferCFT/axwayvariablesComponentShortName  >}} API applications.

{{< TransferCFT/axwayvariablesComponentShortName  >}} API applications examples are written in C: Microsoft Visual
C++ 4.1, Delphi 2 or Visual Basic 5.0.

<span id="Before_developing_your_first_CFT_API_application"></span>

### Before developing your first {{< TransferCFT/axwayvariablesComponentShortName  >}} API application

Before beginning to write {{< TransferCFT/axwayvariablesComponentShortName  >}} API applications, you should:

1. Read this topic and the topics in [Using
    APIs,.](../../../about_this_document_zos/using_apis)
1. Familiarize yourself with the
    sample source files.
    -   C: ..\\CFT\\API\\C\\SRC\\APISAMPL.C 
    -   Delphi: ..\\CFT\\API\\DELPHI\\SAMPLE\\CFTAPIDP.DPR 
    -   Visual Basic: ..\\CFT\\API\\VBASIC\\SAMPLE\\CFTAPIVB.VBP
1. Copy
    APISAMPL, CFTAPIDP or CFTAPIVB in the {{< TransferCFT/axwayvariablesComponentShortName  >}} folder.
1. Start
    APISAMPL, CFTAPIDP or CFTAPIVB.
1. Rebuild the sample programs
    from the APISAMPL.C sources and other files (libraries and definition
    files) provided.
1. Run the resulting .EXE to test
    the executables in the sample that you have created.

For issued commands to be interpreted correctly by {{< TransferCFT/axwayvariablesComponentShortName  >}}, you must:

1. Define a CFTPART with an id of PART1.
1. Define a CFTSEND with an id of TEST and fname called TEST.
1. Set up a file called TEST in the CFT\\SEND directory.
1. Start {{< TransferCFT/axwayvariablesComponentShortName  >}}.
1. Change Windows NT console sessions.

<span id="Design_constraints"></span>

Constraints
-----------

### Design

The {{< TransferCFT/axwayvariablesComponentShortName  >}} API functions, which can be called from an application,
are not re-entrant. This means that all {{< TransferCFT/axwayvariablesComponentShortName  >}} API function calls
made by an application must be made in the same thread.

<span id="Coding_constraints"></span>

### Coding

A {{< TransferCFT/axwayvariablesComponentShortName  >}} API application must comply with two requirements:

- When the application
    starts, but before a {{< TransferCFT/axwayvariablesComponentShortName  >}} API is called, the {{< TransferCFT/axwayvariablesComponentShortName  >}} API
    initialization function must be called in:
- Visual Basic:
    Cft_Api_Open (ByVal Version As String) As Integer
- C++ or Delphi:
    CftInitialize of prototype BOOL CftInitialize (void)
- When the application
    terminates, it must inform {{< TransferCFT/axwayvariablesComponentShortName  >}} that it is stopping by calling
    the CftUninitialize function with the following prototype: BOOL
    CftUninitialize ( void )

If they are successful, both functions return TRUE.

<span id="Compilation_constraints"></span>

### Compilation

The apicft.lib dynamic library and APISAMPL.C sample are compiled with
the following options:

- Standard mandatory
    C option:
- Zp1 /\* Structures
    and structure fields aligned on byte boundaries \*/

<!-- -->

- Options specific
    to WIN32:

<!-- -->

- D_X86=1 /\*
    Machine code generation for Intel x 86 processors \*/
- DWIN32 /\* Win
    32 application \*/
- D_MT /\* Multi-thread
    application \*/
- G3 /\* Machine
    code generation compatible with 386 processors and compatibles \*/

<span id="Linking_constraints"></span>

### Linking

#### Key Options

Link-editing for a {{< TransferCFT/axwayvariablesComponentShortName  >}} API application can use all the default
options.

#### Key Libraries

In addition to any application libraries, the key libraries required
to build a {{< TransferCFT/axwayvariablesComponentShortName  >}} API application are as follows:

- APICFT.LIB
- CFTSCP3.LIB

<span id="Execution_constraints"></span>

### Execution

To execute an application using {{< TransferCFT/axwayvariablesComponentShortName  >}}, APIs
need the following files:

- Necessary dynamic
    libraries:
    -   apicft.dll
    -   cftscp3.dll
- Additional dynamic
    libraries for Visual Basic:  
    -   cftvb.dll
- Other files:
    -   profile

These files  must
be installed with the application executables in C or DELPHI on the Windows
workstation, so that the application developed with the {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs
can run correctly.

A Visual Basic program requires an additional DLL to encapsulate calls
to the {{< TransferCFT/axwayvariablesComponentShortName  >}} APIs [cftvb.dll].
