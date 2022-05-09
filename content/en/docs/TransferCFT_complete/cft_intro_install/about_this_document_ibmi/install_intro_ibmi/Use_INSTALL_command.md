{
    "title": "About the INSTALL command",
    "linkTitle": "About the INSTALL command",
    "weight": "190"
}The INSTALL command enables you to install:

- Transfer CFT binary objects, such as programs, to the Transfer CFT program library
- Transfer CFT files to production library
- Transfer CFT IFS files to IFS directory

After uploading the Transfer CFT IBM i CFT33XL file to the target location, in the Transfer CFT profile you must add the CFTTMP library to the library list. In command line, execute:

`ADDLIBLE LIB(CFTTMP) POSITION(*FIRST)      `

> **Note**
>
> Note: The Transfer CFT program library and Transfer CFT production library are created if they do not exist prior to performing this procedure.

During the installation you are prompted to configure your installation, for example:

- Install on an independent ASP
- CFT program library
- CFT production library
- User for whom Transfer CFT is installed
- CFT JOBD, CFT JOBQ, CFT Subsystem, CFT class, CFT OUTQ
- Encryption Password
- Confirm Encryption Password
- Key file name
- Salt file name
- Installation and runtime directory
- IDPARM (max length &lt; 9)
- Transfer CFT Copilot server hostname and port
- Catalog size
- Communication file size

You can also enable:

- Multi-node architecture
- Central Governance connectivity
- REST API server
- Sentinel
- SSL

Lastly, you can configure:

- AM type
- PKI type
- cft instance id
- Synchronous transfer port
- PeSITany port
- PeSITssl port

Executing the INSTALL command 
------------------------------

Enter the INSTALL command and press PF4 to display the Transfer CFT IBM i installation screen.

> **Note**
>
> Note: Pressing ENTER at this stage performs an install using the default values. You can override any default value, as needed.

```
INSTALL CFT (INSTALL)
 
Install on an independent ASP. . . . . . . '2' 1:Yes / 2:No
```

> **Note**
>
> Note: To perform a silent installation, use the CFT Update menu as shown in Perform_auto_installation.htm.

> **Note**
>
> Note: The encryption password to enter for an installation contains special characters. To avoid an issue during installation, please ensure that the encoding of your terminal/keyboard is configured with CCSID 37.
