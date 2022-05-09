{
    "title": "Determine the installer and product versions",
    "linkTitle": "Determine the installer and product versions",
    "weight": "140"
}You should determine the product and Installer version and service pack level prior to upgrading or updating for Transfer CFTs having a version lower than 3.4. You can use the following procedure on any version of the Axway Installer.

Start the Axway installer using the version appropriate command:

- Versions lower than 4.5:` setupUNIX.sh update`
- Versions higher than 4.5:` update.sh `

Follow the screen instructions to display information, and check the:

- Axway Installer version and the most recently installed SP level
- Transfer CFT version and the most recently installed SP level

Alternatively, you can use the display command to list information about all installed products.

- Run the command from the root installation directory.
- When you run this command without parameters, the command lists all installed products and versions, and all applied service packs.

```
./display.sh
```

Use the name parameter, - n, to display the installation history of a single product.
