{
    "title": "UNIX specific requirements",
    "linkTitle": "Unix-specific prerequisies",
    "weight": "160"
}UNIX users are required to have Korn Shell (ksh) installed on all UNIX machines.

### UNIX user for starting the products

For Transfer CFT you can use any user to install and start the product except the user **root**, which is forbidden. Make sure the user has the standard read and write permissions on the installation directory.

### Umask information

The various processes create files during the runtime execution. To ensure that these files are properly used by all the processes, you must make sure that the user has all the rights on the created files. Use the command, umask -S u=rwx.

Depending on your information system architecture and whether or not you need to share files with other products, you can grant rights on these files to users from group or others. For this option use the command, umask -S go=rx.

### Red Hat 6 platform

Axway Installer supports installation on a Red Hat 6 platform however:

- On a 64-bit Linux operating system, the installer starts with the 64-bit JRE by default. If it cannot find the 64-bit JRE it will look for the 32-bit JRE.
- If you want your installation to start smoothly on a Red Hat 6 operating system you must make sure you have the following packages installed on your system because they may not have been installed by default.
    -   glibc.i686
    -   nss-softokn-freebl.i686

### HP-UX requirements

*For HP-UX ia64 and HP-UX PA-RISC*

HP-UX Password Hashing Infrastructure, PHI, enables the use of SHA512-based algorithms for user password hashes (rather than DES). This package is required for Transfer CFT, as it is more secure and provides a new function, crypt2(), for user control.

For more information, refer to [HP-UX Password Hashing Infrastructure Release Notes](https://support.hpe.com/hpsc/doc/public/display?docId=emr_na-c02038049).

### Global environment on a Unix machine

To install on a UNIX machine, you need a specific Unix user ID and enough free space to install in the users home directory.

The default installation path is the users home directory, but you can change the path and install all products in a specific file system.

The installation directory must not contain any sub-folders or files that are owned by another user.

### Using a temporary directory

The installer needs a temporary directory when it starts to unzip and prepare the environment it requires for product or update installation.

By default, it will try one of the following directories: /tmp , /var/tmp , /usr/tmp , $HOME , $PWD.

You can force the use of another temporary directory by setting the following environment variable, TEMPORARY_DIR.

If you do this, make sure the temporary directory has:

- Enough disk space
- Read/write access for starting the installer
