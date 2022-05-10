---
    "title": "Apply a license key ",
    "linkTitle": "Apply a license key ",
    "weight": "200"
---
Check your authorization
------------------------

Verify that you can access Sphere by going to [support.axway.com](https://support.axway.com/) and logging in. If you do not have an account, follow the instructions in your welcome letter.

Log in to download or access:

- The product installation package
- Your product license key
- Product documentation
- Product updates, including patches and service packs
- Product announcements
- The case center, to open a new case or to track opened cases

You can also access other resources, such as articles in the Knowledge Base, and documentation for all {{< TransferCFT/axwayvariablesCompanyName  >}} products.

{{% TransferCFT/snippets/c_assign_key_generic%}}
{{% TransferCFT/snippets/license_key_instances%}}

Obtain a license key
--------------------

{{% TransferCFT/snippets/t_obtain_key_all%}}
<span id="Apply"></span>

Apply a license key
-------------------

To apply the license key from the Axway Fulfillment team, enter the key(s) in the indirection file. Place the character \# before the path name of the indirection file. For example, enter **KEY=\#prefix..UPARM(PRODKEY)** in the CFTPARM definition. Note:

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
KEY = \#%ENVCFT%.UPARM(PRODKEY),
…
```

Access the &lt;TARGET&gt;.INSTALL library, and run the JCL called **CFTABOUT**. Near the bottom of the CFTABOUT output, the **`cpuid `line is displayed.**  

```
\* cpuid = 000000000ABC1234
```

In this example, you would provide the CPU ID **000000000ABC1234**.

> **Note**
>
> Note: Your cpuid will differ from those shown in the examples.
