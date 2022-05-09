{
    "title": "Verify your installation",
    "linkTitle": "Verify your installation",
    "weight": "150"
}See the installation if you encounter problems when starting {{< TransferCFT/axwayvariablesComponentLongName  >}}.

Installed directories
---------------------

Once you install the product, the following subdirectories are available:

- Bin: Contains the installation procedures. This directory is unique to Axway. This directory also contains trace files after installation.
- Install: Contains the Transfer CFT product and the configuration file for silent installation.


| File Name  | Description  |
| --- | --- |
| silent_install.conf  | File to customize for silent installation.  |
| Transfer_CFT_mvs_adrdssu.bin  | Compressed Transfer CFT files in ADRDSSU format. Name: distlib.UPLIB(CFT332A) |
| Transfer_CFT_mvs_xmit.bin  | Transfer CFT files in double XMIT format.<br/> Name: distlib.UPLIB(CFT332X) |
| Transfer_CFT_mvs_adrdssu_J1IDIST.txt  | JCL that executes the extraction (next step).<br/> Name: distlib.UPLIB(J1IDISTA) |
| Transfer_CFT_mvs_xmit_J1IDIST.txt  | JCL that executes the extraction (next step).<br/> Name: distlib.UPLIB(J1IDISTX) |
| Transfer_CFT_mvs_J2IICFT.txt  | JCL that installs Transfer CFT instance environment.<br/> Name: distlib.UPLIB(J2IICFT)mode |


> **Note**
>
> Note: The act of installing Transfer CFT creates a distribution environment that contains product binaries and templates. Do no store any personal files in this environment, or modify existing files in this environment, as they are erased during updates.

- EULA.html: License agreement (html format)
- EULA.txt: License agreement (text format)
- setup.sh: Installation script for UNIX/Linux
- setup.bat: Installation script for Windows
