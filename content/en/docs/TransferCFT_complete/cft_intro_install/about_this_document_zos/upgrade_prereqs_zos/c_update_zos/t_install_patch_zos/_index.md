{
    "title": "Transfer CFT maintenance (non-SMP/E)",
    "linkTitle": "Transfer CFT maintenance (non-SMP/E)",
    "weight": "250"
}Update or apply a service pack (non-SMP/E)
------------------------------------------

This section describes how to install a patch or service pack on your z/OS Transfer CFT using the non-SMP/E method.

Information includes:

- [Install update libraries (PTF)](#Install%20update%20libraries%20(PTF))
- [Transfer the PTF file to the host machine](#Transfer%20the%20PTF%20file%20to%20the%20host%20machine)
- [Integrate PTF elements using A13PTFLD](#Integrate%20PTF%20elements%C2%A0using%20A13PTFLD) 
- [Apply a PTF using A13PTFLK](#Apply%20a%20PTF%C2%A0using%20A13PTFLK) 

<span id="Install update libraries (PTF)"></span>

Install update libraries
------------------------

A PTF file results from the fixed formatting (80) of an ADRDSSU-type file containing the update libraries. The PTF format is used by all delivery and distribution modes. The PTF files are available at [support.axway.com](http://support.axway.com/).

> **Note**
>
> Note: PTF files are cumulative. A basic Transfer CFT z/OS installation can integrate PTFs. If you use this method, apply the PTFs one at a time.

Libraries taken into account during a DUMP ADRDSSU


| Libraries  | Contents  |
| --- | --- |
| INSTALL | Update the installation JCLs |
| SAMPLE | Update the SAMPLES |
| SAMPLEO | Update the COBOL samples |
| SAMPLEC | Update the C samples |
| SAMPLEA  | Update the ASM samples  |
| PFTOBJ | Update OBJECT modules |
| DOC | Update documentation |
| CNTL | Control files |
| MAC | Macros |
| SCR | Update components for interactive functions, messages, sample SSL |
| UPARM | Update unified configuration parameters definition. |
| OBJ  | Update NEW OBJECT modules.  |
| COPY  | Update Cobol copybook.  |
| H  | Update ‘header’ C.  |
| XMLLIB  | Update XML components.  |
| EXEC  | Update Transfer CFT procedures.  |


<span id="Transfer the PTF file to the host machine"></span>

Transfer the PTF file to the host machine
-----------------------------------------

You can transfer a given PTF file from either the workstation or the host, in binary form, to the central site **distlib.UPLIB** library.

<span id="Transfer file using FTP"></span>

### Transfer file using FTP

The following is an example of commands for the binary transfer of the PTF file from the workstation to the host:

```
open hostname 
userid  
userpsw 
binary
put c:\\mycftpatchs\\patch_from Axway support website 'distlib.UPLIB(CFxxxxxx)'   
```

Where:

- xxxxxx identifies the PTF number

<!-- -->

- distlib indicates the distribution environment

<!-- -->

- distlib.UPLIB library is created during the product installation

<span id="Retrieve file using FTP A13PTFFT"></span>

### Retrieve file using FTP A13PTFFT

If an FTP server is configured on the workstation, you can use the sample JCL, A13PTFFT in target.INSTALL library, to transfer the PTF file in binary format via FTP. In the JCL, specify the address of the workstation and the PTF identifier.

### A13\* JCL descriptions

All of the A13\* JCLs are used to update or apply a Service Pack to Transfer CFT as described here.


| JCL  | Description  |
| --- | --- |
| A13AUTO  | To automatically apply fixes.  |
| A13PTFFT  | To transfer a patch to the distlib.UPLIB in binary mode (using FTP - mode GET).  |
| A13PTFLD  | To update the distribution libraries. (ADRDSSU)  |
| A13PTFLI  | To update the distribution libraries. (XMIT)  |
| A13PTFLK  | To apply a patch in the Transfer CFT loadlib (create a save library/link-edit).  |
| A13RSTOR  | To restore the loadlib from a save library.  |
| A13SDEL  | To delete a save-load library when a patch is validated, or if the loadlib is restored to reapply a patch.  |
|   | **Transfer CFT Copilot update**  |
| A13UCOPA  | To apply a patch to Transfer CFT Copilot (Create a save file).  |
| A13UCOPR  | To restore the Transfer CFT Navigator environment from a save file in USS environment.  |
| A13UCOPD  | To delete a save file when a patch is validated.  |
|   | Transfer CFT - Secure Relay - Master Agent update  |
| A13UXSRA  | To apply a patch to Secure Relay - Master Agent (creates a save file).  |
| A13UXSRR  | To restore the Transfer CFT {{< TransferCFT/suitevariablesSecureRelayName  >}} environment from a save file in USS environment.  |
| A13UXSRD  | To delete a save file when a patch is validated.  |
|   | **Other JCL**  |
| A13JCL  | To customize the patched JCL.  |
| A13UCONF  | To update the unified configuration parameter definitions.  |
| A13XML  | To update the XML library.  |
| A13WDEL  | To delete work files. |
| A13RBACK  | To automatically execute the three jobs A13RSTOR, A13UCOPR, and A13UXSRR.  |


<span id="Integrate PTF elements using A13PTFLD"></span>

Integrate PTF elements using A13PTFLD
-------------------------------------

After loading PTF files, you must integrate the composing elements of the PTF files into the distribution libraries. Only perform this operation though from one of the Transfer CFT instances environments. 

The JCLs used are found in the target.INSTALL libraries below.

Before you submit the JOB, specify the PTF identifier in the EXEC card:

`A13PTFLD: PTF integration in distribution libraries`

`//LOADPTF  EXEC PLOADPU,ID=’xxxxxx’, …    `

This JOB takes place in several stages:

1. Deletes the PTF temporary libraries.
1. Formats the ADRDSSU of the PTF (IKJEFT01).
1. Extracts temporary PTF libraries (ADRDSSU, or IKJEFT01).
1. Copies (with replace) PTF components in the distribution libraries.
1. Deletes the PTF temporary libraries.

> **Note**
>
> Note: This operation is displayed in the file distlib.LOG.

For more information, you can consult the patch documentation located in the distlib.DOC library and named DCxxxxxx (xxxxxx are the patch identifiers). These documents describe known incidents, corrections and PTF specifics (exits, and so on). Additionally the library, PTFINFO member, lists all corrected incidents.

<span id="Apply a PTF using A13PTFLK"></span>

Apply a PTF using A13PTFLK
--------------------------

Usually a patch is applied through a LINK-EDIT. The JCL A13PTFLK is found in the target.INSTALL library.

Before submitting the JOB, specify the LINK EDIT identifier in the EXEC card:

```
//APPLY  EXEC PAPPLY,ID=’xxxxxx’
```

Where:

- xxxxxx: patch identifier

This JOB runs in several phases:

- Backup of LOAD libraries, of which one of the qualifiers is the PTF identifier

<!-- -->

- LINK-EDIT

> **Note**
>
> Note: This operation is displayed in the file distlib.LOG.

Deleting a backup file version
------------------------------

**A13SDEL**

Use this JOB to delete a backup file after you have validated the application of a PTF. Each backup requires about 130 cylinders of the available 3390. Before submitting the JOB, specify the patch identifier on the EXEC card:

`//DELSAV EXEC PDELSAV,ID=’xxxxxx’`

> **Note**
>
> Note: This operation is displayed in the file distlib.LOG.

Updating the Copilot server
---------------------------

When you apply a patch to the Transfer CFT Copilot server, the update is not automatic.

> **Note**
>
> Note: The user applying the PTFs should have write access rights for the coppath directory, where coppath is a customizable parameter (A03PARM member in the target.INSTALL library).

The following three JCLs mange the PTFs for Copilot:

- A13UCOPA: Saves all files (\*) and PTF application (in a sequential file)
- A13UCOPD: Deletes the save file that is associated with a PTF application
- A13UCOPR: Restores files from the save file

> **Note**
>
> Note: All of these operations are displayed in the file distlib.LOG.

Before submitting the JCL, modify the value associated with the ID=, where xxxxxx is the identifier of the PTF to apply:

```
// SET ID='xxxxxx' (JCL A13UPTFA)
//DELSAV EXEC PCDELSAV,ID=’xxxxxx’ (JCL A13UPFTD)
// SET ID='xxxxxx' (JCL A13UPTFR)
```

### Updating the unified configuration definitions A13UCONF

This JOB updates the member DEFAULT in target.UPARM.

Automatically applying fixes A13AUTO
------------------------------------

The use of this JCL is optional. It allows you to submit JOB 'A13\*', which runs as described in the patch readme (Distlib..doc(..)). Check the information concerning the application of the patch or service pack's in the associated readme.

You must configure the following JCLs in automatic mode.

> `..INSTALL(A13PTFLD)   >> ID='AUTO'`
>
> `..INSTALL(A13PTFLK)   >> ID='AUTO'`
>
> `..INSTALL(A13UCOPA)   >> ID='AUTO'  or ID='NONE'`
>
> `..INSTALL(A13UXSRA)   >> ID='AUTO'  or ID='NONE'`

If ID='NONE', this JCL is not submitted.

****Related topics****

- [About migrating Transfer CFT z/OS]()
