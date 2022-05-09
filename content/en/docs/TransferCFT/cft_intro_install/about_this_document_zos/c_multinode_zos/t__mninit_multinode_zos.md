{
    "title": "Customize MNINIT",
    "linkTitle": "Customize MNINIT",
    "weight": "200"
}The following section is based on implementing an Active/Active High Availability multi-host configuration use case. You should modify accordingly to implement either:

- Mono host, multi-node architecture
- Multi host, multi-node architecture

About MNINIT
------------

The  JCL MNINIT is delivered with the Transfer CFT product. It describes the possible uses cases available for Transfer CFT multi-node configuration. In the following example configuration, you begin by customizing this JCL to transform it from a standalone to a multi-node/host architecture.

In this example you configure two hosts. If your implementation has fewer or more hosts, repeat the host customization steps for each host. Replace the x's and y's with the actual host names in your organization.

- hostname xxxxxxx -host xx.xxx.xx.xx Host 1
- hostname yyyyyyy -host yy.yyy.yy.yy Host 2

Procedure
---------

Edit the MNINIT JCL located in the INSTALL Library as described in the following steps.

> **Note**
>
> Note: You only perform steps 4 and 8 if you are setting up a multi host multi-node configuration (not for a mono host, multi-node configuration).


| Step  | Task  | Command or details  |
| --- | --- | --- |
| 0  | Define file size  |   |
| 1  | Activate multi-node  | PARM=’UCONFSET ID=cft.multi_node.enable,value=yes’  |
| 2  | Define the number of nodes | PARM=’UCONFSET ID=cft.multi_node.nodes,value=2’  |
| 3  | Add hostname 1  | PARM=’add_host –hostname xxxxxxx –host xx.xxx.xx.xx’  |
| 4  | Add hostname 2<br/> (repeat for additional hosts) | PARM=’add_host –hostname yyyyyyy –host yy.yyy.yy.yy’  |
| 5  | Create transfer files for each node | PARM=’cftinit &amp;EXTPARM’<br/> CFTIN DD DISP=(NEW,PASS),DSN=&amp;&amp;TMPU,<br/> SPACE=(TRK,(1)),DCB=(RECFM=VB,LRECL=1024,DSORG=PS) |
| 6  | Enable node 0  | PARM='enable_node -n 0'  |
| 7  | Enable node 1 (repeat for additional nodes) | PARM=’enable_node –n 1’  |
| 8  | Customize the Copilot server address | PARM=uconfset<br/> ID=copilot.GENERAL.ServerHost,value=&amp;COPVIPA' |


> **Note**
>
> Note: JCL variables to customize as described in the table above:

- EXTPARM: Transfer CFT configuration file
- COPVIPA: VIPA address for Copilot
- CFTVIPA: VIPA address for Transfer CFT server

### Additional steps and notes

- Operator commands are 'local' to a node.
- The UCONF `sentinel.trktname` parameter defines the Sentinel overflow file. Configure as follows:
    -   Set the `sentinel.trksharedfile` parameter to YES.
    -   You must use an Event Router to process the overflow file.
    -   For a multi-hosts, multi-node implementation, define the logger file using a CF-Structure.
    -   For a mono-host, multi-node implementation, define the logger file using the DASDONLY parameter.

After completing these steps, you have a configured MNINIT, but note that you do not yet have an installed multi-node Transfer CFT.
