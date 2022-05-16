---

    title: Define a flow to send multiple files
    linkTitle: Define a flow to send multiple files
    weight: 330

---
Click <span class="bold_in_para">****Flows**** </span>on the top toolbar to open the<span class="bold_in_para"> ****Flows / Monitoring**** </span>page.

The following outlines tasks for defining a flow. Optional steps, such as defining source and target properties, have default values in the user interface and are not described in this getting started topic.

In the <span class="bold_in_para">****Flow List****</span> page click <span class="bold_in_para">****Add flow****</span> and perform the following steps:

1. Select **General information** and complete the fields. The following fields are mandatory (\* denotes mandatory):
    -   Name: Enter a user friendly name in our example <span class="bold_in_para">****Simple flow****</span>
    -   Identifier: Enter the id that will be used in the transfer commands <span class="bold_in_para">****flow01**** </span>(Transfer CFT IDF)
1. Select Source and click Edit. Define <span class="bold_in_para">****Store\_66**** </span>as the source in this example. The source is the owner of the data being transferred.
1. Select Target and click Edit. Define <span class="bold_in_para">****MainOffice**** </span>as the <span class="bold_in_para">****Target**** </span>for this example. The target is the receiver of the exchange.
1. Optionally, select Protocol and define. You cannot define a protocol until you have defined both the source and target.
1. Save the flow, or save and deploy the flow.

****Result****

![](/Images/TransferCFT/new_flow_cg_w_store.png)

> **Note**
>
> Tip  
> Existing Transfer CFT users may want to refer to the Transfer CFT to Central Governance parameter mapping in the sections see Flow definition: Target and Flow definition: Source.

<span class="bold_in_para">****&lt;&lt;**** </span><a href="../../" class="bold_in_para MCXref xref xrefbold_in_para"><strong><strong>My first transfer flow</strong></strong></a>