{
    "title": "Create a flow and transfer a file",
    "linkTitle": "Create a flow and transfer a file",
    "weight": "290"
}Let's begin by creating a simple flow, and then exchange a file. In this example, Store_66 sends a daily SALES_report to the MainOffice.

![Simplified diagram of a Source Transfer CFT sending a file to a Target](/Images/TransferCFT/TransferCFT_Standard_w_cg.png)

#### In {{< TransferCFT/PrimaryCGorUM  >}}

1. Define a flow.
    -   Select **Flows** &gt; **Add flow**.
    -   ![](/Images/TransferCFT/flow01.png)

    <!-- -->

    -   Create a flow named `Simple_flow` and give it the identifier ****`flow01`****.
    -   ![](/Images/TransferCFT/flow02.png)

    <!-- -->

    -   Select **Source** &gt; **Applications** &gt; then **Transfer CFT**. Choose, for example Store_66 and click **Add source**. Repeat the same steps for **Target**, choosing the MainOffice as the Target. You can leave all other fields set to the default values.
    -   An *application* is the logical representation of a business software application that is the true sender or true receiver in a file exchange. An application can represent a back-end enterprise resource system, such as SAP or PeopleSoft.

    -   ![](/Images/TransferCFT/flow03.png)

1. Click ******Deploy****** to save and deploy. [Details](../intro_cg_task_catalog/t_savedeployflow)

#### In {{< TransferCFT/axwayvariablesComponentShortName  >}}

1. Add a file to exchange. Put a test file, for example ****SALES_report****, in the Store_66 {{< TransferCFT/axwayvariablesComponentShortName  >}}` runtime/pub` folder.
1. From the source {{< TransferCFT/axwayvariablesComponentShortName  >}}, run the SEND command.
    -   Remember to replace `<instance_target>` with your Transfer CFT for the MainOffice target.

    <!-- -->

    -   The flow ****Identifier**** field is equivalent to the {{< TransferCFT/axwayvariablesComponentShortName  >}} IDF parameter.
