---
title: " Planning"
linkTitle: "Prerequisites"
weight: 160
---This document describes how to prepare for and install {{< TransferCFT/axwayvariablesComponentShortName  >}} in an OpenVMS environment.

## Important information

Each supplied product must be installed or updated using the VMSINSTAL installation procedure. If you are changing systems, use the installation procedure to install {{< TransferCFT/axwayvariablesComponentShortName  >}} VMS on your new environment. Never use a backup of your previous disk, as the procedure updates some of the files in the user account.

## Delivered components

The Transfer CFT is distributed as a zip file from Axway Website. Use the VMSINSTAL utility to install the product.

### Preparation

Use the following list to prepare for installation.

- Check software licenses for installation
- Verify that the zip file is available.
- If installing from an alternate software source, verify that the software zip file is accessible.
- Verify the license keys for each of the hostnames are received.

#### Database

- Verify that the Database server is available.
- Verify the number of connections, and how many are dedicated Axway connections?

#### User accounts and permissions

- Verify that user accounts have been created and username/password are known.
- Are the appropriate permissions in place?

#### Server access and connectivity

- Check that you can log in as system administrator.

## End User License Agreement

You should read and accept the End User License Agreement (EULA) prior to installing Transfer CFT. The EULA file is in the directory where you decompressed the Transfer CFT package.

<span id="Product"></span>

## Product license key

```
Product license key.
Enter the product license key. Alternatively, you can set the Transfer CFT key in the CFT_SCEN:CFT.KEY.
\* Enter the license key:
--> Type M to modify, ENTER to continue
:
Generate Encryption Key
Please specify a seed password to generate the encryption key:
Warning: The password must contains at least 8 characters, contain upper and lower
case characters as well as numeric and special characters (\*#$!?+-@
\* Password:
\* Confirm password:
Choose a location to store the generated key and salt files:
\* Key file .......... [CRYPKEY.KEY]:
\* Salt file .......... [CRYPSALT.KEY]:
--> Type M to modify, ENTER to continue
:
```

 
