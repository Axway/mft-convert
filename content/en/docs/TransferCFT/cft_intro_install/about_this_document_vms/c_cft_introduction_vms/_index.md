{
    "title": "Before you start",
    "linkTitle": "Install ",
    "weight": "170"
}If you are installing {{< TransferCFT/axwayvariablesComponentLongName  >}} as part of an {{< TransferCFT/axwayvariablesCompanyName  >}} {{< TransferCFT/axwayvariablesSolutionShortName  >}} solution, you may want to check the installation order and prerequisites. For more information, please refer to the {{< TransferCFT/suitevariablesCentralGovernanceName  >}} documentation.

Before you start the installation, you should:

- Download the installation package from {{< TransferCFT/axwayvariablesCompanyName  >}} Sphere.
- Unzip the package.

Installation package contents
-----------------------------

The installation package is a zip archive. You should unzip it on the OpenVMS machine. Once you unzip it, it contains the product and the installation procedure (installer).

Installation functions
----------------------

The installer is used to install, configure, update and uninstall Transfer CFT, which is part of the Axway 5 Suite. You can run the following installation modes:

- Install
- Configure
- Update
- Uninstall

Installation modes
------------------

Use the standard `SYS$UPDATE:VMSINSTAL` procedure to install the product.

Installed directories
---------------------

Once you install a product, the following sub-directories are installed.

- [.CFT.HOME] contents files of the product.
- [.CFT.RUNTIME] contents mainly configuration files and executables.
