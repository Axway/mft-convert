{
    "title": "Create a flow to transfer multiple files",
    "linkTitle": "Create a flow to transfer multiple files",
    "weight": "300"
}This flow allows you to send multiple files to a defined application. In this scenario, the Store_66 application sends several files to the MainOffice target.

![](/Images/TransferCFT/TransferCFT_Multiple_send_w_CG.png)


|   | Task  | Description  | Details  |
| --- | --- | --- | --- |
| 1<br/> <br/>  | Create a flow.<br/> <br/> <br />  | In {{< TransferCFT/PrimaryCGorUM  >}} click ****Flows**** &gt; ****Add flow****.<br/> Create a flow named ****multiple_files_flow****, and give it the identifier ****flow02****.<br/> Define Store_66 as the Source, and MainOffice as the Target.<br/> <br />  | [![](/Images/TransferCFT/mapArrow.png)](../intro_cg_task_catalog/t_multiple_filesflow)  |
| 2<br/>  | Enable a multiple files exchange.<br/>  | Select the Source, and then ****File properties****.<br/> Under Filename select ****Multiple****.<br/>  | [![](/Images/TransferCFT/mapArrow.png)](../intro_cg_task_catalog/t_multiple_files)  |
| 3<br/>  | Deploy the flow.<br/>  | In {{< TransferCFT/PrimaryCGorUM  >}} click ****Deploy**** to save and deploy.<br/>  | [![](/Images/TransferCFT/mapArrow.png)](../intro_cg_task_catalog/t_savedeployflow)  |
| 4<br/> <br/>  | Add three files for the exchange.<br/> <br/>  | Create a folder, you can name it ****Store_66****, in the Store_66 {{< TransferCFT/axwayvariablesComponentShortName  >}} <code>runtime/pub </code>directory.<br/> Copy three test files to this folder, for example SALES_report, DAILY_news, and INVENTORY.<br/>  |   |
| 5<br/> <br/> <br/>  | Execute the SEND command.<br/> <br/>  | From the source {{< TransferCFT/axwayvariablesComponentShortName  >}}, run the following command: <code>CFTUTIL SEND part=&lt;instance_target&gt;, idf=flow02, fname=#pub/Store_66/*</code> Remember to replace <code>&lt;instance_target&gt;</code> with the Transfer CFT instance for the target as it displays in your application list.<br/>  | [![](/Images/TransferCFT/mapArrow.png)](../../../../../c_intro_userinterfaces/about_cftutil)  |
| 6  | Monitor the transfer status.  | In the {{< TransferCFT/PrimaryCGorUM  >}} ****Flows**** tab, click ****Monitoring**** to check the status of the file exchange.  | [![](/Images/TransferCFT/mapArrow.png)](../intro_cg_task_catalog/c_flow_monitoring)  |

