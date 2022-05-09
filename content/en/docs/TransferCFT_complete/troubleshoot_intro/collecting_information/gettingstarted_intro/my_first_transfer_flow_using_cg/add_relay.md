{
    "title": "Add a relay (store and forward)",
    "linkTitle": "Add a relay (store and forward)",
    "weight": "330"
}This page describes how to use a relay in a SEND/RECV operation. You can use a SEND/RECV to perform a transfer with a single partner via an intermediate machine, or relay, using the store and forward mode. Additionally, you can combine the store and forward mode with broadcasting when using the SEND command.

On {{< TransferCFT/PrimaryCGorUM  >}}
------------------------------------------

1. In the toolbar, select **Flows** &gt; **Add flow**.  

    ![](/Images/TransferCFT/flow1.png)

1. **Name** the flow `Relay_flow`.  
    ![](/Images/TransferCFT/flow3.png)
1. Select **Source? &gt; Add source**, and click to select the source {{< TransferCFT/axwayvariablesComponentLongName  >}}. Choose `Store_66` and then click **Select as source**.  
    You can leave all other fields set to the default values.  
    ![](/Images/TransferCFT/flow4.png)
1. Click **Protocol?** and select **PeSIT**. Enter `flow22 `as the **Flow identifier**.  
    Use the other default values, and remember that the flow identifier is the IDF on Transfer CFT.  
    ![](/Images/TransferCFT/flow5.png)
1. Select **Target? &gt; Add target**. Then click to choose the target {{< TransferCFT/axwayvariablesComponentLongName  >}}. Select `MainOffice `and then click **Select as target**.
1. Click **Relay** then click **Edit relay**. Select the relay, and confirm by clicking **Select as relay**.  
    ![](/Images/TransferCFT/flow8.png)
1. The **Enable store and forward** option displays, leave it set to Yes.  
    ![](/Images/TransferCFT/flow9.png)
1. In the newly displayed **Protocol?** and select **PeSIT**. Enter `flow22 `as the **Flow identifier**. Use the other default values.
1. Click ****Save****. **** You can check that you have correctly selected the source, target, and relay, then click ****Deploy.**** [Details](../intro_cg_task_catalog/t_savedeployflow)

#### In {{< TransferCFT/axwayvariablesComponentShortName  >}}

You will need to add a file locally for the transfer exchange and execute the SEND command.

1. Put a test file, for example ****SALES_report****, in the Store_66 {{< TransferCFT/axwayvariablesComponentShortName  >}}` runtime/pub` folder.
1. From the source {{< TransferCFT/axwayvariablesComponentShortName  >}}, run the SEND command. Remember:
    -   Replace `<instance_target>` with your Transfer CFT for the `MainOffice `target.

    <!-- -->

    -   The flow ****Identifier**** field is equivalent to the {{< TransferCFT/axwayvariablesComponentShortName  >}} IDF parameter.

```
CFTUTIL SEND part=<instance_target>, idf=flow22, fname=pub/SALES_report
 
CFTUTIL LISTLOG /to check the status/
```
