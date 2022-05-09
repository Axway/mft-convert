{
    "title": "File sizes and formats",
    "linkTitle": "File sizes and formats",
    "weight": "160"
}This section describes the installation environment and the space requirements for installation.

A *distribution* environment is created during the product retrieval phase of the installation. This distribution environment contains the transferred files that provide the product contents, as well as any installed upgrades.

The Transfer CFT *instance* is the environment that the user configures for operational use, and is created from the distribution environment. File sizes in the user target environment are defined in the JCL J2IICFT. This JCL is defined twice - in the UPLIB library, and in the INSTALL library of the distribution environment.

The tables in this section provide information about the files and allocation requirements for the distribution and instance environments. Note that an ESD installation requires an additional 1100 disk cylinders (3390 disk) of space.

<span id="Distribution_environment file_formats_and_requirements_"></span><span id="kanchor74"></span>

Distribution environment file formats and requirements
------------------------------------------------------


| File  | Info  | Allocation in cylinders  |
| --- | --- | --- |
| INSTALL | Installation and exploitation<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY | 10 |
| SAMPLE | Sample JCLs, and Transfer CFT sample parameters<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY | 2 |
| SAMPLEO | COBOL samples (Exit and API)<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY | 2 |
| COPY  | Copy book (Cobol)<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY | 2  |
| SAMPLEC | C samples (Exit and API)<br/> PO – VB – 255<br/> DSNTYPE=LIBRARY  | 2 |
| H  | C headers<br/> PO – VB – 255<br/> DSNTYPE=LIBRARY | 2  |
| SAMPLEA  | ASM samples (Exit and API)<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY | 2  |
| OBJ | Object modules<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY<br/> (NON-SMP/E INSTALLATION) | 100 |
| PFTOBJ | Patched object modules<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY<br/> (NON-SMP/E INSTALLATION) | 300 |
| LOAD  | Transfer CFT load modules (SMP/E INSTALLATION)  | 120  |
| DOC | Readme (patch or service pack)<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY | 10 |
| CNTL | Link edit control cards<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY | 3 |
| MAC | ASM macro<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY | 2 |
| SCR | UI components (Copilot, Secure Relay) and patches<br/> PO –VB – 4090<br/> DSNTYPE=LIBRARY | 500 |
| UPLIB | Upload library for the product, service packs, and patches<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY | 100 |
| LOG | Installation log<br/> PS – VB – 255<br/> DSNTYPE=LIBRARY<br/> (NON-SMP/E INSTALLATION) | 1 |
| UPARM | Configuration parameters<br/> PO – VB – 255<br/> DSNTYPE=LIBRARY | 2 |
| EXEC  | Transfer CFT procedures<br/> PO – FB 80<br/> DSNTYPE=LIBRARY | 2  |
| XMLLIB  | XML components (CSD, etc.)<br/> PO – VB - 4090<br/> DSNTYPE=LIBRARY | 2  |
| PKIMSG  | Trusted File messages<br/> PO – FB - 4090<br/> DSNTYPE=LIBRARY | 1  |


<span id="Instance"></span><span id="kanchor75"></span>

Instance environment file formats and requirements
--------------------------------------------------

The following allocations are required per {{< TransferCFT/axwayvariablesComponentShortName  >}}.


| **File** | **Environment** | **Allocation** |
| --- | --- | --- |
| INSTALL | PO – FB – 80<br/> DSNTYPE=PDS | SPACE = (3120,(600,225,40) ) |
| SAMPLE | PO – FB – 80<br/> DSNTYPE=PDS | SPACE = (27920,(100,100,30)) |
| SAMPLEO | PO – FB – 80<br/> DSNTYPE=LIBRARY | SPACE = (27920,(100,100,-)) |
| SAMPLEC | PO – VB - 255<br/> DSNTYPE=LIBRARY | SPACE = (27920,(100,100,-)) |
| SAMPLEA  | PO – FB - 80 DSNTYPE=LIBRARY | SPACE = (27920,(100,100,-))  |
| EXEC | PO – FB – 80<br/> DSNTYPE=PDS | SPACE = (3120,(195,195,30)) |
| XSR  | ZFS directory<br/> (Secure Relay) |   |
| XMLLIB | PO – VB – 4090<br/> DSNTYPE=LIBRARY | SPACE = (27998,(100,50,-)) |
| CERTIF | PO – VB – 4090<br/> DSNTYPE=LIBRARY | SPACE = (27998,(10,5,-))  |
| LOAD | DSNTYPE=LIBRARY | SPACE = (CYL,(120,50,-)) |
| <span id="USER.load"></span>USER.LOAD  | DSNTYPE=LIBRARY  | SPACE = (CYL,(50,20,-))  |
| UCONF | Runtime configuration parameters<br/> PS – VB - 2048 | SPACE = (TRK,(5,2)) |
| UCONFRUN  | Runtime configuration parameters<br/> PS – VB – 2048 | SPACE=(TRK,(5,2))  |
| USER.OBJ  | API and exits objects<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY | SPACE=(3200,(200,100,-))  |
| UPARM | PO – VB - 255<br/> DSNTYPE=PDS | SPACE = (TRK,(5,5,20)) |
| PKIMSG | PO – VB - 4090<br/> DSNTYPE=LIBRARY | SPACE=(TRK,(1,1,-)) |
| XLATE  | PO – FB - 256<br/> DSNTYPE=LIBRARY | SPACE=(TRK,(5,5,-))  |
| FTEST  | Test file<br/> PS – VB – 84 | SPACE = (TRK,(1,1))  |
| INCDLL  | SYSDEFSD – Link-edit IMPORT control statements<br/> PO – FB – 80<br/> DSNTYPE=LIBRARY | SPACE = (TRK,(50,10,-))  |
| MONLOG  | Started Transfer CFT server log for multi-node<br/> PS – VB – 255 | SPACE = (TRK,(20,10))  |
| UPLOAD  | Upload library used by Central Governance<br/> PO – FB – 80 DSNTYPE=LIBRARY | SPACE = (TRK,(20,10))  |
| LOG  | Installation LOG PS – VB - 255  | SPACE = (TRK,(10,50))  |
| CRYPKEY  | File name containing the private key that enciphers data  |   |
| CRYPSALT  | File name containing the salt used to create the private key  |   |


> **Note**

- The *Instance environment* list above does not include files that Transfer CFT creates in its implementation, such as CATALOG, PARM, PART, COM, PKIFILE, LOG, ACCOUNT etc.
- The persistent cache file for PassPort AM (CFTAM, VSAM KSDS) is created when the UCONF AM.type=passport variable is set to ****Yes****.
- To customize INSTALL, SAMPLE, EXEC and UPARM THE library must be PDS and not PDSE.

****Related topics****

- [About Transfer CFT in z/OS](../)
- [Installation overview]()
