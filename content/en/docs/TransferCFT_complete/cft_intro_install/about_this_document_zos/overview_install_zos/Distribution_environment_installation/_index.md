{
    "title": "Non-SMP/E: Create the distribution environment ",
    "linkTitle": "Non-SMP/E: create distribution environment",
    "weight": "170"
}This section describes how to install the **<span id="kanchor56"></span>distribution** **environment**. After installing the distribution environment, you should not need to modify it. Later you will use the installed distribution environment to create a Transfer CFT <span id="kanchor57"></span>[*instance* *environment*](t_install_instance_envr_zos) (runtime). The term instance replaces the former notion of a *target* environment in Transfer CFT.

When you install the Transfer CFT you can create the following environments in a single step:

- Distribution environment
- Transfer CFT run-time instance environment

<span id="kanchor58"></span>

Define a Transfer CFT alias
---------------------------

You can create a Transfer CFT ALIAS in the USER CATALOG if the file names mentioned in the installation JOBs are reserved for operation.

To define an alias, adapt the parameters in bold to suit your environment. Enter:

```
//DEFALIAS EXEC PGM=IDCAMS  
//SYSPRINT DD SYSOUT=sysout
//SYSIN    DD \*     
   DEFINE ALIAS(NAME(CFTV2
) RELATE(USER.CATALOG))   ...................
/\*     
```

Download the installation library from ESD 
-------------------------------------------

### Required configuration

- An FTP client that permits the transfer of files to the z/OS host
- Download the ESD file from {{< TransferCFT/axwayvariablesCompanyName  >}} Support at [https://support.axway.com](https://support.axway.com/)

> **Note**
>
> Note: ISO files were deprecated in version 3.0.1.

To install the Transfer CFT z/OS product, you need approximately:

- 200 cylinders 3390 of disk space on z/OS to transfer the delivery files from another system using FTP

<!-- -->

- 450 additional cylinders of disk space to unpack the installation files

<span id="Installa"></span><span id="kanchor59"></span>

Installation files 
-------------------

The installation package is a zip archive that contains the product and installer program files.

### Installation modes

Once you unzip the files, locate and run the setup file in the root folder of the installation package. Two installation modes are available:

- Installation (console mode)
    -   UNIX/Linux: setup.sh
    -   Windows: setup.bat
- Silent installation
    -   UNIX/Linux: setup.sh --silent
    -   Windows: setup.bat --silent

Upload Transfer CFT z/OS using the installer
--------------------------------------------

This section describes how to prepare the distribution environment necessary to create a target environment. The procedure consists of:

- Installing the product
- Creating the distribution environment

```
Unix Installation example:
./setup.sh
 
/home/cft/Transfer_CFT_3.3.2_Install_mvs_BN10687000/bin/axwaykit_linux Version 1.0 ===
-------------------------------------------------------
>> Start the configuration installation - Transfer CFT ZOS with /home/cft/Transfer_CFT_3.3.2_Install_mvs_BN10687000/bin/axwaykit_linux on Fri Apr 06 13:40:41 2018
-------------------------------------------------------
>Enter the local working directory where you want to save the configuration [Default:/home/cft/Transfer_CFT_3.3.2_Install_mvs_BN10687000/bin][Mandatory]
--------------------------------
>Enter the dataset name for the product installation (4 qualifiers min.)
[Default:AXWAY.XFB.D332.CF030000][Mandatory]
Number of qualifiers=4
>Enter File Format ADRDSSU(A) or XMIT(X):
[Default:A][Mandatory]
 
 
Installation runtime:
--------------------------------
>Enter the dataset name for the runtime environment (as either a value or NO). [Default:AXWAY.V332][Mandatory]
 
Mainframe address (for FTP):
--------------------------------
>Enter hostname address: [No default][Mandatory]
axway.hostname.mvs
 
>Enter login TSO: [No default][Mandatory]
TSOUSER
 
>Enter password: [\*\*\*\*\*\*][Mandatory]
\*\*\*\*
 
>Enter Y if you want to change the user and password? (Y/N) [Default:N][Mandatory]
\*\*\*\*
 
>Enter Y if you want to change the user and password? (Y/N) [Default:N][Mandatory]
 
>Enter Y/N to define if you want submit the JCL(Note: JESINTERFACELEVEL should be set to 2) [Default:Y]
 
>Enter the execution class (JCL) [Default:A][Mandatory]
 
>Enter the account parameter (JCL) [Default:()]
Configuration summary: ---------------------
Local parameters ----------------
Local work PATH : /home/cft/Transfer_CFT_3.3.2_Install_mvs_BN10687000/bin
Installation PATH : /home/cft/Transfer_CFT_3.3.2_Install_mvs_BN10687000/bin
 
 
Host parameters
---------------
Host IP address : axway.hostname.mvs
User : TSOUSER
Upload library : AXWAY.XFB.D332.CF030000.UPLIB
>> this library will be created
>> member : CFT332A
Transfer CFT runtime envir.: AXWAY.V332
JCL to be submitted : J1IDISTA
jobname : TSOUSERI
 
>Enter Y if you agree with these parameters, or N to Exit installation (Y/N)
[No default][Mandatory]
```

Silent installation
-------------------

Silent mode enables you to perform an installation or configuration in a non-interactive mode. You do not have to enter any parameters in the console. To use this mode, you must install the product or run the installer program and perform the configuration until just before you execute setup.sh or setup.bat.

Before you start the silent installation you must update the silent_install.conf installation file located in the install directory.


| Value  | Default value  | Description  |
| --- | --- | --- |
| &amp;cftinstall | AXWAY.XFB.D332  | Library prefix qualifiers for distribution environment  |
| &amp;distlev | CF030000  | Distribution prefix level  |
| &amp;hostuplib | AXWAY.XFB.D332.CF030000.UPLIB | Library prefix to upload product  |
| &amp;cftruntime | AXWAY.V332  | Library for the runtime creation of the target environment  |
| &amp;patch | N/A  | Information about last patch applied  |
| &amp;format | A  | Type of restoration product:<br/> • A (ADRDSSU)<br/> • X (XMIT) |
| &amp;hostname | N/A  | z/OS hostname  |
| &amp;user | N/A  | z/OS user  |
| &amp;password | N/A  | z/OS password  |
| &amp;installvol | N/A  | Volume Serial for distribution environment  |
| &amp;runtimevol | N/A  | Volume Serial for runtime environment  |
| &amp;account | ()  | Accounting  |
| &amp;class | A  | Class  |
| &amp;unit | 3390  | Unit  |
| &amp;submit | Y  | Submit JCL for the runtime creation  |


Once you have configured and saved the file for silent installation, run the following command to start the installation:

- UNIX/Linux: setup.sh --silent
- Windows: setup.bat --silent

Decompress the installation files
---------------------------------

Use one of the following methods to unpack the installation files:

- Decompress using the ADRDSSU format, *or*
- Decompress using double Xmit format

<span id="kanchor60"></span>

### Decompress using the ADRDSSU format

From the transferred distlib.UPLIB library, customize and submit the J1IDISTA JCL. Refer to the Note below for details.

This procedure:

- Transforms the product file into an ADRDSSU file type (via IKJEFT01)

<!-- -->

- Restores the Transfer CFT distribution files via ADRDSSU

<!-- -->

- Creates a distribution environment

<!-- -->

- Creates a Transfer CFT instance environment (if required)

To customize the JCL, apply a `change all` command on the following parameters:

- distlib: distribution environment prefix

<!-- -->

- volser: volume serial number, used to override the default value. If used, the statement marked as comment must be activated

<!-- -->

- storclass: SMS Storage class, used to override the default value. If used, the statement marked as a comment must be activated

> **Note**
>
> Note: The distribution files are restored with 5 qualifiers (ADRDSSU). You can modify 4 of these qualifiers, for example AXWAY.XFB.CFT332.CF030000, using the ADRDSSU parameter, step ADRD020 in J1IDISTA, but the fifth qualifier is hard coded.

<span id="kanchor61"></span>

### Decompress using double Xmit

From the transferred distlib.UPLIB library, customize and submit the J1IDISTX JCL.

This procedure:

- Performs two successive orders RECEIVE (via IKJEFT01)
- Creates a distribution environment
- Creates a Transfer CFT instance environment (if needed)
- Creates a Composer connector environment for Transfer CFT (if needed)

To customize the JCL, apply a change all command to the following parameters:

- distlib: distribution environment prefix
- Volse: volume serial number, used to override the default value. If used, the statement marked as comment must be activated
