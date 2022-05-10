---
    "title": "System requirements",
    "linkTitle": "System requirements",
    "weight": "180"
---
The installation process lasts approximately 10 to 15 minutes, depending on the system. It is performed from an account that has special privileges.

> **Note**
>
> Note: VAX systems are currently not supported by Transfer CFT 3.10 and higher.

Installation requirements include:

- IA64: approximately 665,000 blocks of free disk space
- At least 3,400 free global pages and two global sections to use {{< TransferCFT/axwayvariablesComponentShortName  >}}
- At least 400 free global pages and two global sections to use Copilot

These values are sufficient if all {{< TransferCFT/axwayvariablesComponentShortName  >}} users belong to the same group, in UIC terms. Otherwise, multiply these values by the number of different user groups to calculate the total system requirement.

To use different networks with {{< TransferCFT/axwayvariablesComponentShortName  >}}, you must install these products.

The privileges, identifiers and quotas required to execute {{< TransferCFT/axwayvariablesComponentShortName  >}} are automatically assigned by the installation procedure to the account associated with the product. Some of these privileges and quotas must be assigned to the various {{< TransferCFT/axwayvariablesComponentShortName  >}} users.

Requirements for uploading the installation kit
-----------------------------------------------

- An installed FTP server on the OpenVMS system
- An FTP user with rights to upload files
- A connection between the local machine and the OpenVMS system
- Alternatively, use the binary mode to transfer the zip file

### Defaults and system values

The following table lists the ports and their uses with {{< TransferCFT/axwayvariablesComponentLongName  >}}. Additionally, you can print and record the values for your own installations.


| Used for...  | Port Number (default)  | Firewall  | Your installation  |
| --- | --- | --- | --- |
| Available  | 1761 to 1768  | N/A  |   |
| PeSIT  | 1761  |   |   |
| Copilot Server  | 1766  |   |   |
| Synchronous API  | 1765  |   |   |
| Single Authentication  | 1762  | Bidirectional  |   |
| Mutual Authentication  | 1763  |   |   |
| Proxy port  |   |   |   |
| Database  |   |   |   |
| HTTPS  |   |   |   |
| Additional information  |   |   |   |
| Transfer CFT IP address  |   |   |   |
| CA and password for root CA  |   |   |   |
| License information  |   |   |   |


Apply a license key
-------------------

You need to apply a valid license key to Transfer CFT in the following situations:

- You perform an initial Transfer CFT installation.
- To replace an expired license key, typically after a year.
- A hardware upgrade can change the CPU ID (CPU serial number); if so, you must reapply the license.

### Obtain a license key

1. Install {{< TransferCFT/axwayvariablesComponentShortName  >}}. You can install {{< TransferCFT/axwayvariablesComponentShortName  >}} without a license key, and enter the key later.
1. After completing the installation, or for an existing installation, use the command **`cftutil about`** to retrieve your system information.
1. Contact the Axway Fulfillment team at the appropriate email address to obtain a key.
    -   For a US key, contact: `fulfillment@us.axway.com`
    -   For an EMEA or APAC key, contact: `product.key@axway.com`
1. Apply the license key(s) that you received from the Axway Fulfillment team as follows:
    -   Enter the key in the default file: CFT_SCEN:CFT.KEY.

### Apply a license key

Apply the license key(s) that you received from the Axway Fulfillment team as follows:

- Enter the key in the default file: CFT_SCEN:CFT.KEY.

{{% TransferCFT/snippets/single_key_multinode%}}
