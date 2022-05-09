{
    "title": "Create a Transfer CFT instance ",
    "linkTitle": "Create a Transfer CFT instance",
    "weight": "180"
}SMP/E FMIDs list
----------------

The Transfer CFT software has a SYSMOD FMID (Function Module ID) that identifies the software and its release number. For example, FMID TCF0300 identifies the Transfer CFT 3.3.2 release.


| FMID | Format |
| --- | --- |
| TCF0100 |   |


Upload the SMP/E package to the mainframe
-----------------------------------------

1. Unzip the package file, for example Transfer_CFT_3.3.2_install_mvs_BNnnnnnnnn.smpe.zip.
1. Run the setup.bat (Windows) or setup.sh (Unix).
1. Enter the following parameters in the console. Upon completion you require an FTP client.


| Question | Default value |
| --- | --- |
| Dataset name for the product SMPE samples | AXWAY.SMPE.CFT332.SMPCNTL |
| USS SMP/E Network Temporary Store | /home/AXWAY/smpnts |
| USS package subdirectory | CFT332 |
| Hostname mainframe address<br/> Login user<br/> Login password |  <br/>  |


- The setup procedure allocates the destination library dataset (default is AXWAY.SMPE.CFT332.SMPCNTL),  
    with the attributes recfm=fb,lrecl=80,blksize=27920,space=(cyl,(1,5)).
    -   To add a volume or device name, edit the &vol= or &unit= field in the silent_smpe_install.conf file.
- The sample member $C\* is transferred to the dataset using FTP in text mode.
- The setup procedure creates the destination USS directory (default is /home/AXWAY/smpnts), and subdirectory (default is CFT332).
- The Transfer_CFT_mvs.pax.Z SMP/E package file is transferred to the subdirectory using FTP in binary mode, and renamed to CFT332.pax.Z  (approximately 150 Mbytes).

### Silent mode

You can run the setup procedure in silent mode, `setup –s`. In this case, you do not have to enter parameters in the console, but prior to starting you must update the s`ilent_smpe_install.conf` file located in the install directory.

Create a new Transfer CFT SMP/E environment
-------------------------------------------

The user ID you use must have read access to the SAF facility class resources GIM.CMD.command and GIM.PGM.program, for example PERMIT GIM.\* CLASS(FACILITY) ID(user ID) ACCESS(READ).

1. Edit the sample jobs in the following table, modifying as necessary; supply a valid JOB JCL card statement, select and modify the dataset qualifier &HLQ&, and the optional device &UNIT& and volume &VOLUME&.  
    
| Member Name | Description |
| --- | --- |
| $C01DCSI | Allocate the SMP/E CSI, and the operational and work data sets used by SMP/E. |
| $C02DZON | Define the SMP/E distribution and target zones. |


1. Submit the jobs in the order listed.

Receive the Transfer CFT SMP/E product
--------------------------------------

1. Edit the sample job listed in the following table, modifying as necessary; supply a valid JOB JCL card statement, select and modify the dataset qualifier &HLQ&.  
    
| Member name | Description |
| --- | --- |
| $C10RECV | UNPAX the archive package and RECEIVE the FMIDs functions. |


1. Submit the job.  
    The following data sets (approximately 300 cylinders) are created and suffixed by their corresponding FMID identifier (TCF0nnn) and RELFILEs number (Fn).  
    
| File Number | Data Set Suffix | Description |
| --- | --- | --- |
|  1 | TCF0nnn.F1 | HFS pax files |
|  2 | TCF0nnn.F2 | Transfer CFT XML files |
|  3 | TCF0nnn.F3 | Transfer CFT exec procedures  |
|  4 | TCF0nnn.F4 | Transfer CFT JCLs |
|  5 | TCF0nnn.F5 | Trusted File messages |
|  6 | TCF0nnn.F6 | Transfer CFT parameter and JCL samples |
|  7 | TCF0nnn.F7 | Transfer CFT general parameters |
|  8 | TCF0nnn.F8 | Link-edit maps |
|  9 | TCF0nnn.F9 | Transfer CFT load modules |
| 10 | TCF0nnn.F10 | Copy COBOL |
| 11 | TCF0nnn.F11 | H headers |
| 12 | TCF0nnn.F12 | Assembler macros |
| 13 | TCF0nnn.F13 | Assembler samples |
| 14 | TCF0nnn.F14 | C samples |
| 15 | TCF0nnn.F15 | COBOL samples |


Install the target libraries with SMP/E
---------------------------------------

1. Edit the sample jobs that are listed in the following table, modifying as necessary; supply a valid JOB JCL card statement, select and modify the dataset qualifier &HLQ& and the optional device &UNIT& and volume &VOLUME&.  
    
| Member name | Description |
| --- | --- |
| $C20ALLT | Allocate the Target libraries. |
| $C30FAPP | Perform an APPLY (with the CHECK operand) to install the SYSMOD functions in the target zone and libraries. |


1. Submit the jobs in the order listed.  
    If the $C30FAPP return code equals zero, edit $C30FAPP, remove the CHECK command, and resubmit the job.  
    The $C20ALLT job creates, and the $C30FAPP job updates, the following data sets (approximately 300 cylinders).  

    
| Data Set Suffix | File Type | Description |
| --- | --- | --- |
| CF0nnnnn.SCR  | PDSE / VB | HFS pax files |
| CF0nnnnn.XMLLIB  | PDSE / VB | Transfer CFT XML files |
| CF0nnnnn.EXEC  | PDS / FB | Transfer CFT exec procedures  |
| CF0nnnnn.INSTALL  | PDS / FB | Transfer CFT JCLs |
| CF0nnnnn.PKIMSG  | PDSE / VB | Trusted Files messages |
| CF0nnnnn.SAMPLE  | PDS / FB | Transfer CFT parameter and JCL samples |
| CF0nnnnn.UPARM  | PDS / VB | Transfer CFT general parameters |
| CF0nnnnn.CNTL  | PDSE/ FB | Link-edit maps |
| CF0nnnnn.LOAD  | PDSE / U | Transfer CFT load modules |
| CF0nnnnn.COPY  | PDSE/ FB | Copy COBOL |
| CF0nnnnn.H  | PDSE / VB | H headers |
| CF0nnnnn.MAC  | PDSE / FB | Assembler macros |
| CF0nnnnn.SAMPLEA  | PDSE / FB | Assembler samples |
| CF0nnnnn.SAMPLEC  | PDSE / VB | C samples |
| CF0nnnnn.SAMPLEO  | PDSE / FB | COBOL samples |


    Create the Transfer CFT instance files
    --------------------------------------

    Edit the sample job listed below, modifying as necessary: supply a valid JOB JCL card statement, choose and modify the dataset qualifier &HLQ& of the SMP/E target libraries, and select the appropriate Transfer CFT instance qualifier &CFT& and Transfer CFT instance device &UNIT& and volume &VOLUME&.

    
| Member Name | Description |
| --- | --- |
| $C40ICFT | Creates the Transfer CFT instance files. |


    Submit the job.

    > **Note**
    >
    > Note: The Transfer CFT dataset files, the instance customization, and build steps are described in the COMMON INSTALLATION chapter.

    ### Additional SMP/E sample jobs

    The following table lists additional sample jobs.

    
| Member Name | Description |
| --- | --- |
| $C10REJT | Performs a REJECT of the SYSMODs in the SMP/E global zone. |
| $C50ALLD | Allocates the distribution libraries. |
| $C60FACC | Performs an ACCEPT (with the CHECK operand) the SYSMOD functions in the distribution zone and libraries. |

