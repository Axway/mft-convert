{
    "title": "Start/stop Transfer CFT",
    "linkTitle": "Post-installation",
    "weight": "180"
}This section describes the procedures for using {{< TransferCFT/axwayvariablesComponentShortName  >}} in an OpenVMS environment.

A full installation performs all the tasks needed to adapt the required files to your system.

The CFTLOGIN.COM procedure in the D$CFT_RUN:[PROFILE] directory is ready for use. You can customize or update this procedure if changes have been made to your operating environment. You are strongly advised to review this procedure so that you can familiarize yourself with the Transfer CFT environment.

If you have installed the product on an existing account, you must add a call to the CFTLOGIN.COM procedure in the login procedure of this account.

If the account does not already exist, the installation sequence gives you the option of using CFTLOGIN.COM as the login procedure.

The installation generates sample configuration files that provide information on {{< TransferCFT/axwayvariablesComponentShortName  >}} operations.
