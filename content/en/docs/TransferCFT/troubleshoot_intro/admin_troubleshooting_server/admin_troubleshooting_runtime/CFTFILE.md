{
    "title": "Manually create internal data files",
    "linkTitle": "Manually create internal data files",
    "weight": "400"
}The CFTFILE command is used to create, replace, or delete Transfer CFT
files. These initial files define the most basic parameters for the Transfer
CFT. You can further define the Transfer CFT settings after the initial
start, but Transfer CFT must have this CFTFILE as a minimum
configuration.

You can only create the initial environment in the CFTUTIL command line
interface. You must create all files that are handled by the Transfer
CFT. After creating a basic CFTFILE, you can execute the profile
file, and continue either in command line or use the old Transfer CFT UI Configuration
Wizard.

About the CFTFILE command
-------------------------

The CFTFILE command affects the following files:

- PARAMETER - containing
    the Transfer CFT general parameters where TYPE = PARM (or PARAM)
- PARTNER - containing
    the descriptions of the characteristics of partners where TYPE = PART
- CATALOG - containing
    the control information associated with transfers where TYPE = CAT
- STATISTICS - containing
    the information relative to terminated transfers where TYPE = ACCNT
- LOG - used to record
    messages associated with the execution of transfers and CFT operations
    where TYPE = LOG
- COMMUNICATION -
    used to enter transfer requests and Transfer CFT management comm where
    TYPE = COM

To delete a Transfer CFT file, MODE = DELETE, you must declare the parameters
FNAME, the name of the file
to be deleted, and TYPE, the
type of the file to be deleted.


| OS  | Description  |
| --- | --- |
| IBM i | The CFTFILE command is incorporated in Transfer CFT IBM i Manager. It can, however, be activated directly in the log file switching procedures. See the example supplied in the B_EXECLOG member. |


You can use the [CFTCATAL](../../../../cft_intro_install/unix_install_start_here/run_first_time_ux/use_cft_utilities) utility to resize the catalog. In a multi-node environment, this action resizes all nodes.

Use the CFTFILE command to create (MODE = CREATE) empty or delete (MODE
= DELETE) Transfer CFT files.


| Parameter  | Description  |
| --- | --- |
| [see table](../../../../c_intro_userinterfaces/command_summary/parameter_intro/fblksize) | Defines the block size of the file to be created (in bytes).<br/> Depends on the TYPE/OS |
| [FNAME](../../../../c_intro_userinterfaces/command_summary/parameter_intro/fname)  | Name of the file the command applies to. |
| [see table](../../../../c_intro_userinterfaces/command_summary/parameter_intro/fspace) | Primary allocation of the file to be created, expressed in K bytes (1024).<br/> Depends on the TYPE/OS |
| [see table](../../../../c_intro_userinterfaces/command_summary/parameter_intro/fspacex) | Secondary allocation of the file to be created, expressed in K bytes (1024).<br/>  |
| [HABFNAME](../../../../c_intro_userinterfaces/command_summary/parameter_intro/habfname) | Name of the security system initialization file. |
| [TYPE]() = COM | Name of the lock file created in parallel with the communication file and used to manage file access conflicts. |
| [MODE](../../../../c_intro_userinterfaces/command_summary/parameter_intro/mode) | Action requested on the file. |
| [NODE]() | Node identifier.<br/> Available when TYPE=CAT |
| [RECNB](../../../../c_intro_userinterfaces/command_summary/parameter_intro/recnb) <br/> TYPE = {COM &#124; CAT} | Number of records in the file. |
| [TYPE](../../../../c_intro_userinterfaces/command_summary/parameter_intro/type) = {ACCNT &#124; CAT &#124; COM &#124; LOG &#124; PARM (PARMA) &#124; PART} | Type of file concerned by the command.<br/> When TYPE = CAT, COM, PARM or PART, you can use the HABFNAME parameter for security. |


******Syntax******

{{% TransferCFT/snippets/cftfile%}}

****Example****

The following command creates a parameter file.

```
CFTFILE    TYPE
= PARM,
MODE = CREATE,
FNAME = filename
```
