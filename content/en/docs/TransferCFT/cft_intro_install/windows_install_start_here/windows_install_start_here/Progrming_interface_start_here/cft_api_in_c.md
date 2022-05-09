{
    "title": "Building  an API in C",
    "linkTitle": "Building API in C",
    "weight": "260"
}This section explains how to build Transfer CFT API samples in C. You can also refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} *User Guide [About APIs](../../../../about_this_document_zos/using_apis) topics* for information such as links to sample files.

The API samples and the makefile used to build them are located in your {{< TransferCFT/axwayvariablesComponentLongName  >}} `<installdir>\runtime\src\capi` directory.

Prerequisites
-------------

The following steps require that you have Microsoft Visual Studio (VS) and a compiler, such as Visual C++, installed on your computer.

Procedure
---------

1. From the Windows Start menu, select **All programs &gt; Axway software &gt; [Transfer CFT name] &gt; Transfer CFT &gt; Command Prompt**.  
    This opens a command window, which executes profile.bat.
1. In the same command window, initialize the VS environment by executing the appropriate vcvarsall.bat based on your system architecture.
    -   Windows 32 bits: `<ProgramFiles>\<VS directory>\VC\vcvarsall.bat x86`
    -   Windows 64 bits: `<ProgramFiles>\<VS directory>\VC\vcvarsall.bat amd64`
    -   Windows Itanium: `<ProgramFiles>\<VS directory>\VC\vcvarsall.bat ia64`
1. To set the CPU environment variable enter the following command based on your Transfer CFT architecture.
    -   Windows 32 bits:     `set CPU=X86`
    -   Windows 64 bits:     `set CPU=AMD64`
    -   Windows Itanium: ` set CPU=IA64`
1. Go to the directory containing the C source files and makefile:
1. Enter the command:

Results
-------

The `nmake `command generates the following as either 32-bit or 64-bit executable programs, depending on the Transfer CFT architecture. The current directory contains the generated object files and executable programs:

- api2xmp1.exe
- api2xmp2.exe
- apisampl.exe
- tcftsyn.exe
- utisyn.exe

> **Note**
>
> Note: The capi.makfile contains a variable called LIB_BUFFEROVERFLOWU whose value may be set to bufferoverflowU.lib, or left empty depending on the compiler version. You may have to manually change this variable if it is incompatible with your compiler. Normally this variable is empty in 64-bit environments.

Remove generated files
----------------------

To remove all of the generated .obj and .exe files, enter the command:

```
nmake /f capi.mak clean
```
