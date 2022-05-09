{
    "title": "Determine or change the CCSID",
    "linkTitle": "Determine or change the CCSID ",
    "weight": "250"
}A coded character set identifier (CCSID) is a value that allows you to define a specific page code for your IBM i system. This value can be defined in your system, user profile, or subsystem. The value 65535 is an IBM i specific value that enables automatic resolution by the system.

You should pay particular attention to this value as it may impact {{< TransferCFT/suitevariablesTransferCFTName  >}} depending on the language installed on the system. Note that Axway uses CCSID 37 when generating the product.

If you want to know the CCSID used in your current environment or if you want to change it, perform the following steps:

1. Connect with the Transfer CFT profile.
1. Start {{< TransferCFT/suitevariablesTransferCFTName  >}}: `CFTSTART`
1. Enter: `WRKACTJOB`
1. Before the CFTMAIN process, enter the value `5` and then ENTER.
1. Enter the value `2` and then ENTER.

**Example**

The screen displays the coded character set identifier. In the following example, you can see the values to apply in production are CHGUSRPRF USRPRF(CFT) LANGID(EN_US) CCSID(37).

![](/Images/TransferCFT/temp_ccsid.png)
