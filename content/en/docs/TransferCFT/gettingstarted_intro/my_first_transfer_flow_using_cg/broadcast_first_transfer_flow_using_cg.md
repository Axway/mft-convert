---

    title: Perform broadcast and collect operations
    linkTitle: Perform broadcast and collect operations
    weight: 190

---
Broadcast and collect modes allow you to send files to multiple partners and receiving files from multiple partners using a single command request for each.

## Broadcast

You can use broadcasting to send a file to an entire group of partners in one command, avoiding the task of sending the selected file individually to each of the partners in a flow.

The name of the broadcast list is itself a virtual partner, which contains a list of partners in your flow. This list of partners can be explicit, or it can reference a file that contains the list of partners.

Additionally, you can define what occurs if a partner is unknown, how the scripts are applied to the broadcast list, and so on. [More information](../../../concepts/transfer_command_overview/broadcast_collect)...

![Simplified diagram of a Source Transfer CFT sending a file to multiple Targets](/Images/TransferCFT/Broadcast_w_cg.png)

QQQ\_QQQ\_QQQ


|   | Task  | Description  | Details  |
| --- | --- | --- | --- |
| 1<br/> <br/>  | Create a flow.<br/> <br/> <br />  | In {{< TransferCFT/PrimaryCGorUM  >}} click <span >****Flows**** </span>&gt; <span >****Add flow****</span>.<br/> Create a flow named <span >****Broadcast_flow****</span>, and give it the identifier <span >****flow04****</span>.<br/> In this flow the MainOffice is the Source with the two stores as the Targets. | <a href="../intro_cg_task_catalog/t_defineflow_broadcast">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 2 | Enable broadcasting mode. | In the Source Transfer properties, enable Broadcast list. | <a href="../intro_cg_task_catalog/t_defineflow_broadcast#enable_broadcast_cg">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 3 | Deploy the flow. | In {{< TransferCFT/PrimaryCGorUM  >}} click <span >****Deploy**** </span>to save and deploy the flow. | <a href="../intro_cg_task_catalog/t_savedeployflow">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 4 | Create a file for the flow. | For this example, copy a file SALES_report to transfer in the Store_66 {{< TransferCFT/axwayvariablesComponentShortName  >}}<span ><code> runtime/pub </code></span>folder. |   |
| 5 | Execute the SEND command. | From the source {{< TransferCFT/axwayvariablesComponentShortName  >}}, run the following command:<br/> <span ><code>CFTUTIL SEND PART=DEST_stores, IDF=flow04, FNAME=pub/SALES_report</code></span> | <a href="../../../c_intro_userinterfaces/about_cftutil">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 6  | Monitor the file transfer status.  | In {{< TransferCFT/PrimaryCGorUM  >}} select the <span >****Flows**** </span>tab, and click <span >****Monitoring****</span>.  | <a href="../intro_cg_task_catalog/c_flow_monitoring">![](/Images/TransferCFT/mapArrow.png)</a>  |


## Collect

Collecting files is the inverse of using a broadcast list. In the collect transfer mode you can receive a dedicated file from multiple partners (P*n*). This allows the receiver, or flow initiator, to receive a file from all defined partners using a single request command. More information...

![Simplified diagram of a Target Transfer CFT receiving files from multiple sources](/Images/TransferCFT/TransferCFT_Collect_w_CG.png)

 

QQQ\_QQQ\_QQQ


|   |  Task  | Description  | Details  |
| --- | --- | --- | --- |
| 1<br/>  | Create a flow.<br/> <br />  | In {{< TransferCFT/PrimaryCGorUM  >}} define a flow named <span >****Collect_flow****</span>, and give it the identifier <span >****flow05****</span>.<br/> Use the Source as MainOffice and the stores as the Target.<br />  | <a href="../intro_cg_task_catalog/t_define_simpleflow">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 2 | Enable collect mode. | In the Collect_flow definition, modify so that the Target pulls file. | <a href="../intro_cg_task_catalog/t_defineflow_collect">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 3 | Define the path to the Target file for each store.  | In the Target (each store) select File properties. In Path field, enter the path to the file to send. | <a href="../intro_cg_task_catalog/t_collect_target_properties">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 4 | Deploy the flow. | In {{< TransferCFT/PrimaryCGorUM  >}} you can save and deploy the flow. | <a href="../intro_cg_task_catalog/t_savedeployflow">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 5 | Run the RECV command. | In {{< TransferCFT/axwayvariablesComponentShortName  >}}, run the following command: <span ><code></code></span><span ><code>CFTUTIL RECV PART=DEST_Stores, IDF=flow05</code></span>  | <a href="../../../c_intro_userinterfaces/about_cftutil">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 6 | Monitor the file transfer status. | In {{< TransferCFT/PrimaryCGorUM  >}} select the <span >****Flows**** </span>tab, and click <span >****Monitoring****</span>. | <a href="../intro_cg_task_catalog/c_flow_monitoring">![](/Images/TransferCFT/mapArrow.png)</a>  |

