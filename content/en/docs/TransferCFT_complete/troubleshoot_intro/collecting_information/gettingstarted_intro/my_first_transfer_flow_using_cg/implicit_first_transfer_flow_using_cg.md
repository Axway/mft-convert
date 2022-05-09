{
    "title": "Create an implicit mode flow and transfer a file",
    "linkTitle": "Create an implicit mode flow and transfer a file",
    "weight": "310"
}You can use the implicit transfer mode to make a file whose content is frequently changing available to other applications. In this case the file is always available, and applications can retrieve it as many time as necessary.

> **Note**
>
> Note: Implicit Mode implies that the Target is requester, and as such it is the Target that pulls the file.

![Simplified diagram of a Target Transfer CFT requesting a file from the Source](/Images/TransferCFT/Implicit_mode_cft_w_cg.png)


|   |  Task  | Description  | Details  |
| --- | --- | --- | --- |
| 1<br/> <br/> <br/>  | Create a flow where the target is the requester.<br/> <br/> <br/> <br />  | In {{< TransferCFT/PrimaryCGorUM  >}} click ****Flows**** &gt; ****Add flow****.<br/> Create a flow named ****implicit_flow**** and define the identifier as ****flow03****.<br/> To enable implicit mode, select ****Target pulls file****.<br/> Define the MainOffice as the Target, which will pull the file, and Store_89 as the file Source.<br/> <br />  | [![](/Images/TransferCFT/mapArrow.png)](../intro_cg_task_catalog/t_defineflow)  |
| 2<br/> <br/>  | Define the path to the file location.<br/> <br/>  | In the File properties of the Source, define the path to the file to be sent.<br/> You can use, for example, the <code>TEST </code>file located by default in the source {{< TransferCFT/axwayvariablesComponentShortName  >}}'s <code>runtime/pub</code> folder.<br/>  |   |
| 3<br/>  | Deploy the flow.<br/>  | In {{< TransferCFT/PrimaryCGorUM  >}} click ****Deploy**** to save and deploy.<br/>  | [![](/Images/TransferCFT/mapArrow.png)](../intro_cg_task_catalog/t_savedeployflow)  |
| 4<br/> <br/> <br/>  | Execute the RECV command.<br/> <br/> <br/>  | From the {{< TransferCFT/axwayvariablesComponentShortName  >}} (MainOffice), run the following command: <code>CFTUTIL RECV PART=&lt;instance_source&gt;, IDF=flow03</code> Remember in our example the source is Store_89, you should replace <code>&lt;instance_source&gt;</code> with the Transfer CFT instance as it appears in your applications.<br/>  | [![](/Images/TransferCFT/mapArrow.png)](../../../../../c_intro_userinterfaces/about_cftutil)  |
| 5  | Monitor the file transfer status.  | In {{< TransferCFT/PrimaryCGorUM  >}} select the ****Flows**** tab, and click ****Monitoring****.  | [![](/Images/TransferCFT/mapArrow.png)](../intro_cg_task_catalog/c_flow_monitoring)  |

