{
    "title": "Start A00CUSTO to customize installation files",
    "linkTitle": " Customize installation files (A00CUSTO)",
    "weight": "200"
}<span id="Customizing JCL A00CUSTO parameters"></span><span id="kanchor76"></span>

Customize JCL A00CUSTO parameters
---------------------------------

The A00CUSTO JOB customizes the installation files. The customization is done directly in the installation library instance environment. You must quit the file editing program and library before you execute the SUBMIT. If you do not first quit these programs before executing the SUBMIT, the message `Waiting for data set `is returned. If you performed an Installer installation, these parameters are already customized.

### Installed directories

> **Note**
>
> Note: Do NOT customize the distribution environment.


| Variables  | Description  |
| --- | --- |
| CFTV2 | Prefix of the Transfer CFT instance environment. |
| DISTLIB | Three first qualifiers of the distribution environment.<br/> Example: Axway.XFB.CFT00332 |
| DISTLEV | Fourth qualifier of the distribution environment (level of distribution)<br/> Example: CF030000 |


You can repeat A00CUSTO several times to customize any parameters that were not customized.

Advanced parameters
-------------------

To make any modifications to advanced parameters you must do this prior to starting the A05ALL JCL.

The parameters can be modified in the A12OPTSP member. For more information, refer to [Set standard JCL parameters A12OPTSP](../t_customize_instance_zos#Selectin).

Replay the customization
------------------------

After completing the initial A00CUSTO customization, you can use the JCL A04RPLAY to repeat this process. This JCL recopies the members from distribution environment to the target environment (including the INSTALL, EXEC, UPARM and SAMPLE libraries).

Once the JOB is complete, update the A03PARM file in the target environment and resubmit the JCL A00CUSTO.

****Related topics****

- [Installation overview]()
