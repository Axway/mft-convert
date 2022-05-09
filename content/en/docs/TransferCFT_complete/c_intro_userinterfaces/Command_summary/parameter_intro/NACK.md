{
    "title": "nack",
    "linkTitle": "nack",
    "weight": "2070"
}### nack

#### SEND

****[SEND TYPE = NACK]****

Via negative acknowledgments sent in a PeSIT Hors SIT message, the
final partner signals to the initial sender of the file that application
errors were detected.

If the initial sender does not support this function, the final partner does not transmit the
negative acknowledgement and the Transfer CFT log file displays:

`CFTT93W PART=XFB1 IDS=00008 Negative ack not supported by server`

#### CFTPROT, CFTPART

****[ NACK = { YES &#124; <span class="underline">NO</span> } ]****

This parameter enables or disables the NACK feature in either a partner or protocol definition for a non Transfer CFT product.

To enable the use of NACK when connecting to products other than {{< TransferCFT/axwayvariablesComponentLongName  >}}, set the parameter NACK to YES in the CFTPROT or CFTPART objects.

However, note that the CFTPART NACK value overrides the CFTPROT NACK value. If however, this product does not support a negative acknowledgment, the following error message displays in the log: ` CFTH13E FPDU Remote reject <PART=STREFSSL DIAGI=909 DIAGP=RCO 301>`

> **Note**
>
> Note: When performing file transfers between two Transfer CFTs, negative acknowledgments are sent regardless of the NACK setting.

[Return to Command index](../../)
