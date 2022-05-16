---

    title: Add a relay (store and forward)
    linkTitle: Add a relay (store and forward)
    weight: 180

---
This page describes how to use a relay in a SEND/RECV operation. You can use a SEND/RECV to perform a transfer with a single partner via an intermediate machine, or relay, using the store and forward mode. Additionally, you can combine the store and forward mode with broadcasting when using the SEND command.

## On {{< TransferCFT/PrimaryCGorUM  >}}

1. In the toolbar, select **Flows** > **Add flow**.  

    ![](/Images/TransferCFT/flow1.png)

1. **Name** the flow `Relay_flow`.  
    ![](/Images/TransferCFT/flow3.png)

1. Select **Source? > Add source**, and click to select the source {{< TransferCFT/axwayvariablesComponentLongName >}}. Choose `Store_66` and then click **Select as source**.  
    You can leave all other fields set to the default values.  
    ![](/Images/TransferCFT/flow4.png)

1. Click **Protocol?** and select **PeSIT**. Enter <span class="code">`flow22 `</span>as the **Flow identifier**.  
    Use the other default values, and remember that the flow identifier is the IDF on Transfer CFT.  
    ![](/Images/TransferCFT/flow5.png)

1. Select **Target? > Add target**. Then click to choose the target {{< TransferCFT/axwayvariablesComponentLongName >}}. Select <span class="code">`MainOffice `</span>and then click **Select as target**.

1. Click **Relay** then click **Edit relay**. Select the relay, and confirm by clicking **Select as relay**.  
    ![](/Images/TransferCFT/flow8.png)

1. The **Enable store and forward** option displays, leave it set to Yes.  
    ![](/Images/TransferCFT/flow9.png)

1. In the newly displayed **Protocol?** and select **PeSIT**. Enter <span class="code">`flow22 `</span>as the **Flow identifier**. Use the other default values.

1. Click <span class="bold_in_para">****Save****</span>.<span class="bold_in_para"> **** </span>You can check that you have correctly selected the source, target, and relay, then click<span class="bold_in_para"> ****Deploy.****</span> [Details](../intro_cg_task_catalog/t_savedeployflow)

#### In {{< TransferCFT/axwayvariablesComponentShortName  >}}

You will need to add a file locally for the transfer exchange and execute the SEND command.

1. Put a test file, for example <span class="bold_in_para"> ****SALES\_report****</span>, in the Store\_66 {{< TransferCFT/axwayvariablesComponentShortName >}}<span class="code">` runtime/pub`</span> folder.
1. From the source {{< TransferCFT/axwayvariablesComponentShortName >}}, run the SEND command. Remember:
    -   Replace <span class="code">`<instance_target>`</span> with your Transfer CFT for the `MainOffice `target.

    <!-- -->

    -   The flow <span class="bold_in_para">****Identifier**** </span>field is equivalent to the {{< TransferCFT/axwayvariablesComponentShortName >}} IDF parameter.

```
<span class="code">`CFTUTIL SEND part=<instance_target>, idf=flow22, fname=pub/SALES_report`</span>
 
CFTUTIL LISTLOG /to check the status/
```

 
