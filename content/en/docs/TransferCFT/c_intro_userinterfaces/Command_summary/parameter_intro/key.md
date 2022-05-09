{
    "title": "key",
    "linkTitle": "key",
    "weight": "1740"
}<span id="key"></span>

### key

#### ABOUT

****[ KEY = { <span class="underline">FIRST</span> &#124; ALL } ]****

Defines the number of keys that display.

- FIRST: Displays the first valid key in the product key file.
- ALL: Displays all of the keys in the product key file, regardless of if they are valid. Note that there is a one second pause between each displayed key in error. Additionally, expired keys display their expiration date.

#### CFTPARM

****[ KEY = { string 80 } ]****

The {{< TransferCFT/axwayvariablesComponentShortName  >}} user key. You can enter a string of up to 80 characters, which is comprised of the indirection character and file name (file containing the key).

> **Note**
>
> Note: You do not directly set the key with this parameter.

Enter the name of the indirection file (preceded by the &lt;file-symb&gt; character specific to each system) that contains the set of keys associated with the {{< TransferCFT/axwayvariablesComponentShortName  >}}. The post-installation default values are:

- Unix and HP NonStop: @conf/cft.key
- Windows: \#conf/cft.key

The key is associated with the contractual conditions for using the software. It is specific to the product host operating system, and the hostname or the CPU ID of the machine (depending on the OS).

The key also limits the following for a {{< TransferCFT/axwayvariablesComponentShortName  >}}:

- Maximum number of simultaneous transfers (MAXTRANS parameter limit)
- Maximum inbound/outbound bandwidth value
- Maximum inbound/outbound transfer activation value

[Return to Command index](../../)
