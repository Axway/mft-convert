---

    title: Create an implicit mode flow and transfer a file
    linkTitle: Create an implicit mode flow and transfer a file
    weight: 160

---
You can use the implicit transfer mode to make a file whose content is frequently changing available to other applications. In this case the file is always available, and applications can retrieve it as many time as necessary.

> **Note**
>
> Implicit Mode implies that the Target is requester, and as such it is the Target that pulls the file.

![Simplified diagram of a Target Transfer CFT requesting a file from the Source](/Images/TransferCFT/Implicit_mode_cft_w_cg.png)

QQQ\_QQQ\_QQQ


|   | Task  | Description  | Details  |
| --- | --- | --- | --- |
| 1<br/> <br/> <br/>  | Create a flow where the target is the requester.<br/> <br/> <br/> <br />  | In {{< TransferCFT/PrimaryCGorUM  >}} click <span >****Flows**** </span>&gt; <span >****Add flow****</span>.<br/> Create a flow named <span >****implicit_flow****</span> and define the identifier as <span >****flow03****</span>.<br/> To enable implicit mode, select <span >****Target pulls file****</span>.<br/> Define the MainOffice as the Target, which will pull the file, and Store_89 as the file Source.<br/> <br />  | <a href="../intro_cg_task_catalog/t_defineflow">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 2<br/> <br/>  | Define the path to the file location.<br/> <br/>  | In the File properties of the Source, define the path to the file to be sent.<br/> You can use, for example, the <span ><code>TEST </code></span>file located by default in the source {{< TransferCFT/axwayvariablesComponentShortName  >}}'s <span ><code>runtime/pub</code></span> folder.<br/>  |   |
| 3<br/>  | Deploy the flow.<br/>  | In {{< TransferCFT/PrimaryCGorUM  >}} click <span >****Deploy**** </span>to save and deploy.<br/>  | <a href="../intro_cg_task_catalog/t_savedeployflow">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 4<br/> <br/> <br/>  | Execute the RECV command.<br/> <br/> <br/>  | From the {{< TransferCFT/axwayvariablesComponentShortName  >}} (MainOffice), run the following command: <span ><code>CFTUTIL RECV PART=&lt;instance_source&gt;, IDF=flow03</code></span> Remember in our example the source is Store_89, you should replace <span ><code>&lt;instance_source&gt;</code></span> with the Transfer CFT instance as it appears in your applications.<br/>  | <a href="../../../c_intro_userinterfaces/about_cftutil">![](/Images/TransferCFT/mapArrow.png)</a>  |
| 5  | Monitor the file transfer status.  | In {{< TransferCFT/PrimaryCGorUM  >}} select the <span >****Flows**** </span>tab, and click <span >****Monitoring****</span>.  | <a href="../intro_cg_task_catalog/c_flow_monitoring">![](/Images/TransferCFT/mapArrow.png)</a>  |

