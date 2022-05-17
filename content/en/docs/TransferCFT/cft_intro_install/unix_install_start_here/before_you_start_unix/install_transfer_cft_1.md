---
    title: "Start the installation"
    linkTitle: "4. Perform a standard installation"
    weight: 160
---<span id="Make"></span>

## Make the file executable

Before executing the scripts in this section, run the change mode command:

`chmod +x Transfer_CFT_{{< TransferCFT/axwayvariablesReleaseNumber >}}_Install_<OS>_<BN>.run`

## Install

Use the following command to begin the Transfer CFT installation in graphical mode:

```
./Transfer_CFT_3.9_<Install>_<OS>_<BN>.run
```

The installation wizard displays.

> **Note**
>
> You must accept the License Agreement’s General Terms and Conditions regardless of the installation mode - silent, interactive, or Docker image.

After completing each screen, click **Next** to continue. Click **Cancel** at any time to end the installation setup.

Use the following command to install Transfer CFT in text mode. The screen text is similar to the graphical mode prompts below.

```
./Transfer_CFT_3.9_<Install>_<OS>_<BN>.run --mode text
```

Use the following command to install Transfer CFT in silent mode.

```
./Transfer_CFT_3.9_<Install>_<OS>_<BN>.run --mode unattended --conf-file initialize.properties
```

> **Note**
>
> You cannot install Transfer CFT if you have a profile already loaded.


| Screen  | Description  |
| --- | --- |
| Welcome  | Welcome to Transfer CFT landing page.  |
| License agreement  | Select the check-box "<code>I accept...</code>" to continue with the installation.  |
| Installation architecture  | Select to install on a single machine, a Cluster - first host, or cluster - additional host.<br/> • Single - Installs a single instance of Transfer CFT on a machine.<br/> • Cluster - Installs Transfer CFT on several machines. Select this option if you want to install Transfer CFT in multi-host/multi-node or in active/passive mode.<br/> • Cluster - First host: Install on a first machine before adding additional machines (nodes). You must install on a first node before you can select the option to install on additional nodes.<br/> • Cluster - Additional host: After installing on the first machine, you can select this option to install on an additional machine(s).<br/> • If you are performing a cluster installation and you enable the multi-node option in the configuration file, this creates an active/active Transfer CFT installation. Otherwise, the installation is active/passive. |
| Installation Directory  | Specify the directory where you want to install Transfer CFT. This is the directory where the Transfer CFT product files are installed. <blockquote> **Note**<br/> Directory paths cannot contain spaces.<br/> </blockquote>  |
| Configuration filename  | Enter the path, or navigate, to the configuration file (initialize.properties file) containing details for the Transfer CFT installation. This file defines settings such as hostname, license key, governance options, and so on.<br/> If you do not specify a file, you can continue with the installation, but the installation procedure does not create the runtime directory. (Run the <code>initialize </code>command post installation if you opt to create the runtime directory at a later date.) |
| Generate Encryption Key  | Enter your password and reenter it to confirm. |
| Ready to install | Click ****Next****to complete the installation process, or ****Back****to review or modify installation options. |


 
