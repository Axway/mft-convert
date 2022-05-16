---

    title: Apply a license key 
    linkTitle: Apply a license key 
    weight: 200

---
## Check your authorization

Verify that you can access Sphere by going to [support.axway.com](https://support.axway.com/) and logging in. If you do not have an account, follow the instructions in your welcome letter.

Log in to download or access:

- The product installation package
- Your product license key
- Product documentation
- Product updates, including patches and service packs
- Product announcements
- The case center, to open a new case or to track opened cases

You can also access other resources, such as articles in the Knowledge Base, and documentation for all {{< TransferCFT/axwayvariablesCompanyName  >}} products.

You need to apply a valid license key to Transfer CFT in the following situations:

- You perform an initial Transfer CFT installation.
- A hardware upgrade changes the CPU ID (CPU serial number).
- After a year passes, to replace an expired license key.
- To ramp up a Transfer CFT Disaster Recovery instance (for example, on a DR LPAR for z/OS systems).
- If you are migrating from a version 2.x {{< TransferCFT/axwayvariablesComponentShortName >}} to a version 3.x.

> **Note**
>
> You require as many keys as instances of Transfer CFT running at same time, including when running in multi-node. For example, two Transfer CFT instances cannot run at the same time, on the same server, using the same license key.

## Obtain a license key

1. For a new installation, install {{< TransferCFT/axwayvariablesComponentShortName >}}.
1. After completing the installation, or for an existing installation, use the command **`cftutil about`** to retrieve your system information. For details see the examples below.
1. Contact the Axway Fulfillment team at the appropriate email address to obtain a valid key.
    -   For a US key, contact: **`fulfillment@us.axway.com`**
    -   For an EMEA or APAC key, contact: **`product.key@axway.com`**
1. Provide the hostname where Transfer CFT is to be installed or updated.
1. Provide the list of characters in the CPU ID.

<span id="Apply"></span>

## Apply a license key

To apply the license key from the Axway Fulfillment team, enter the key(s) in the indirection file. Place the character # before the path name of the indirection file. For example, enter **KEY=#prefix..UPARM(PRODKEY)** in the CFTPARM definition. Note:

- The file can contain one or multiple license keys, but there must be one key per line.
- On start up the first valid key is used.

Transfer CFT in multi-node architecture can use a single key for a multi-node installation; as either:

- The hostname must not be defined for the key, or
- The hostname defined for the key matches the hostname of one of the hosts that composes the multi-node instance

Additionally, the key must have the cluster option.

### Examples

Enter the license key in the following format.

```
CFTPARM ID = IDPARM0 ,
…
KEY = #%ENVCFT%.UPARM(PRODKEY),
…
```

Access the &lt;TARGET>.INSTALL library, and run the JCL called **CFTABOUT**. Near the bottom of the CFTABOUT output, the **`cpuid `line is displayed.**  

```
\* cpuid = 000000000ABC1234
```

In this example, you would provide the CPU ID **000000000ABC1234**.

> **Note**
>
> Your cpuid will differ from those shown in the examples.
