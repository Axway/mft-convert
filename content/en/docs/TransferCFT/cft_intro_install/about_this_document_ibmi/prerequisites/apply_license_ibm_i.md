---
    title: "Apply a license key"
    linkTitle: "Apply a license key"
    weight: 170
---You need to apply a valid license key to Transfer CFT in the following situations:

- You perform an initial Transfer CFT installation.
- A hardware upgrade changes the CPU ID (CPU serial number).
- After a year passes, to replace an expired license key.
- To ramp up a Transfer CFT Disaster Recovery instance (for example, on a DR LPAR for z/OS systems).
- If you are migrating from a version 2.x {{< TransferCFT/axwayvariablesComponentShortName >}} to a version 3.x.

## Key management

### Obtain a license key

1. For a new installation, install {{< TransferCFT/axwayvariablesComponentShortName >}}.
1. After completing the installation, or for an existing installation, use the command **`cftutil about`** to retrieve your system information. For details see the examples below.
1. Contact the Axway Fulfillment team at the appropriate email address to obtain a valid key.
    -   For a US key, contact: **`fulfillment@us.axway.com`**
    -   For an EMEA or APAC key, contact: **`product.key@axway.com`**
1. Provide the hostname where Transfer CFT is to be installed or updated.
1. Provide the list of characters in the CPU ID.

### Apply a license key

Apply the license key(s) that you received from the Axway Fulfillment team as follows:

- Navigate to the CFTPROD library, and edit the 'KEY' file.
- Edit the CFTPROD/KEY(KEY) member by entering (writing in) your key.

****Examples****

Use the CFTUTIL utility to execute the ABOUT command to find the CPU ID.

```
CFTUTIL PARAM(ABOUT)
Host information :
\* model = 525 \*
cpuid
= 10A16B2
```

In this example, you would provide the CPU ID 10A16B2.

Use the display system value command to get the serial number, known as QSRLNBR:

```
DSPSYSVAL SYSVAL(QSRLNBR)
System value . . . . . : QSRLNBR
Description . . . . . : System serial number
Serial number . . . .
: 06890AP
```

In this example, you would provide the CPU ID` 06890AP`.

> **Note**
>
> Your values will differ from those shown in the examples.
