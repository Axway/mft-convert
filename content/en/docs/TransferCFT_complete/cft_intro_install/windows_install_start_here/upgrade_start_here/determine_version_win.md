{
    "title": "Determine the installer and product version",
    "linkTitle": "Determine the installer and product version",
    "weight": "160"
}You should determine the product and Installer version and service pack level prior to upgrading or updating for Transfer CFTs having a version lower than 3.4. You can use the following procedure on any version of the Axway Installer.

Start the Axway Installer. The command depends on the Installer version and your OS, as follows:

- Versions lower than 4.5.x:
    -   setupwin32.exe update
- Version 4.5.x or higher:
    -   update32/64.exe

Accept the license and click **Next** to continue. In the **Product list**, check the:

- Axway Installer version and the most recently installed SP level
- Transfer CFT version and the most recently installed SP level

Check product details
---------------------

The display command lists information about all installed products.

- Run the command from the root installation directory.
- When you run this command without parameters, the command lists all installed products and versions, and all applied service packs.

Use the name parameter to display the installation history of a single product.

```
display.bat
```

### Windows Application Experience recommendation

The "Application Experience" service should be enabled when using Transfer CFT. Otherwise you may encounter issues in accessing files when installing/upgrading the product.

### Before you start

Before beginning the upgrade or update procedure:

- You require the product and Installer version number and SP level in order to choose the appropriate procedure. See the section [Determine the Installer and product version](#Determin).
- Download the Transfer CFT Upgrade Pack, available on Sphere at [support.axway.com](https://support.axway.com/). The Transfer CFT Upgrade Pack name has the general format `Transfer_CFT_3.x_<Install/SP/Patch>_<OS>_<BN>.zip`. Do not unzip the .zip file.

Stop the Transfer CFT server and the Transfer CFT UI server, by entering:

- `cft stop `
- `copstop -f `

<!-- -->

- Determine your Axway installer and product versions. The version dictates which of the following Transfer CFT upgrade procedures is correct for you.

> **Note**
>
> Note: Windows: When upgrading, the same type of user that did the initial installation must start the installer.
