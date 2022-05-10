---
    "title": "Apply a license key",
    "linkTitle": "Apply a license key",
    "weight": "170"
---
You need to apply a valid license key to Transfer CFT in the following situations:

- When you perform an initial Transfer CFT installation.
- To replace an expired license key (typically after a year).

Obtain a license key
--------------------

1. After completing the installation, or for an existing installation, use the command **`cftutil about`** to retrieve your system information. {{% TransferCFT/snippets/t_about_command%}}
1. Contact the Axway Fulfillment team at the appropriate email address to obtain a valid key.
    -   For a US key, contact: `fulfillment@us.axway.com`
    -   For an EMEA or APAC key, contact: `product.key@axway.com`
1. Provide the hostname and system information for the installed or updated Transfer CFT.

Apply a license key
-------------------

To apply the license key, enter the Transfer CFT key in the indirection file, which is referred to in the CFTPARM KEY parameter. The file is located at `<installation_directory>/runtime/conf/cft.key` .

- The file can contain one or multiple license keys, but it must have one key per line.
- On start up the first valid key is used.

> **Note**
>
> See the KEY parameter for an example and details.

About command
-------------

Use the CFTUTIL utility to execute the **`about `**command to find the CPU ID and general system information as demonstrated in this example.

```
Host information :
\* model =
\* hostname = Cgnac1
\* cpuid = XXXXXXXXXXX
\* sysname = NONSTOP_KERNEL
\* machine = NSX-D
\* version = 02
\* release = L18
\* distrib = unknown
```

The CPUID is fixed, and as demonstrated in this example is typically ****XXXXXXXXXXX****.
