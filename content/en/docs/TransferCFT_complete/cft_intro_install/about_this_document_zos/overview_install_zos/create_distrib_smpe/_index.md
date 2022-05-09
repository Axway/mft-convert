{
    "title": "SMP/E: Create the distribution environment",
    "linkTitle": "SMP/E: create distribution environment",
    "weight": "160"
}System Modification Program/Extended (SMP/E) is a tool developed by IBM to manage the installation of products in z/OS environments, and to apply maintenance to the repository products.

This section is to describe how to use the SMP/E method to manage a Transfer CFT installation. Remember to review the Prerequisites prior to starting this procedure. For SMP/E installations, please review the [SMP/E grant user resource permissions](../../c_about_zos/r_prerequistes_zos#SMP/E) section.

SMP/E distribution file
-----------------------

You can download the SMP/E distribution file `Transfer_CFT_3.3.x_install_mvs_BNnnnnnnn.smpe.zip` to your computer (Windows or Unix) from [Axway Sphere](https://www.support.axway.com/).

The Transfer CFT z/OS 3.3.x SMP/E package Build Number nnnnnnn archive file contains:

In the root folder:  

- Two installation scripts: setup.sh (Unix) and setup.bat (Windows)
- Two license agreement files: EULA.html and EULA.txt

In the `/install` subdirectory:

- A file (silent_smpe_install.conf) to customize the silent installation or default parameters.
- A pax.Z archive file that contains the SMP/E modification control statements (SMPMCS) and the associated relative files (RELFILEs)
- SMP/E model jobs ($C01DCSI, etc.) to extract the Transfer CFT product package, create the corresponding SMP/E Transfer CFT target environment, and create a new Transfer CFT instance.

In the `/bin` subdirectory:

- The installation executables, procedures, and log installation files.
