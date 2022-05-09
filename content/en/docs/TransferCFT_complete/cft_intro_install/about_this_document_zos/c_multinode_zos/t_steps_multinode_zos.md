{
    "title": "Multi-node installation overview",
    "linkTitle": "Procedure overview ",
    "weight": "180"
}**Installing Transfer CFT**

If you have not done so already, begin by performing a standard Transfer CFT z/OS installation. See the [Transfer CFT {{< TransferCFT/axwayvariablesComponentVersion  >}} z/OS Installation Overview](../../c_about_zos) for information on how to get started. You can set up Transfer CFT multi-node either with multiple nodes and hosts, or multi-node mono-host. This documentation primarily focuses on the multi-node, multi-host scenario.

If you create a multi-node, mulit-host installation, and the USS Environment for Transfer CFT is not shared between all the hosts, you must install a separate USS Environment on each host. To do this, execute the member COPA010, in the INSTALL library, on each host that is not shared.

If an APF mode is defined on one of the hosts, then you must configure the APF mode for every host.

**After installing**

After installing Transfer CFT, you must customize this Transfer CFT from a standard standalone installation to a multi-node enabled Transfer CFT. The following sections, which describe how to customize MNINIT and the VIPA programs, are based on a multi-node and multi-host example implementation. If your organization requires a mono-host installation, you can modify the example procedure accordingly.

Steps
-----

The process consists of customizing the z/OS specific programs and members, checking the Transfer CFT configuration, and then executing the customized programs.

> **Note**
>
> Note: To execute the multi-node customization you require administrator rights.


| Step  | Task  |
| --- | --- |
| 1  | Customize [VIPA](../t_vipa_multinode_zos) programs and execute the customized members, including the following tasks.<br/> For each LPAR:<br/> • Enable the port sharing (to execute on each LPAR) - Share Listening port: (protocol port (e.g. PESIT and port PESIT SSL))<br/> • Identified all ports (protocol ports) to share on the LPAR<br/> • Define the LPAR dynamic XCF address (DYNAMICXCF)<br/> On the first LPAR<br/> • Define the cluster address (VIPA)<br/> • Define the port balancing between the LPAR |
| 2  | Customize [MNINIT]().  |
| 3  | [Execute](../t_submit_mninit_zos) the customized MNINIT. |


> **Note**
>
> Note: Depending on the type of installation you are performing, a Transfer CFT CATALOG file may be created during installation. Additionally, files having the format CATALOG.Nxx (where Nxx represents the node identifier) are created by the MNINIT JCL. The CATALOG file without a suffix is not used, so you can delete this file (the one without the .Nxx suffix). We recommend that you comment the //CFTCAT DD cards in the JCL and procedures. For example, comment the cards in the PCFTUTIL procedure and CFTINC member.
