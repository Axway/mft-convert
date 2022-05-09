{
    "title": "Apply a license key",
    "linkTitle": "Apply a license key",
    "weight": "170"
}{{% TransferCFT/snippets/c_assign_key_generic%}}

Key management
--------------

### Obtain a license key

{{% TransferCFT/snippets/t_obtain_key_all%}}

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
> Note: Your values will differ from those shown in the examples.
