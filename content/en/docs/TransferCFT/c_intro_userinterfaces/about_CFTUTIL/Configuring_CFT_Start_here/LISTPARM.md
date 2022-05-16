---

    title: Listing parameters - LISTPARM 
    linkTitle: LISTPARM - List parameters
    weight: 480

---
This page describes the LISTPARM command. You can use this command
to query Transfer CFT parameters.

****Command syntax: [LISTPARM](../../../command_summary#LISTPARM)****

Use this command to query Transfer CFT parameters. The TYPE parameter selects the object type.

In the absence of a previous CONFIG TYPE = OUTPUT command,
the execution report is written on the standard CFTUTIL program output.


| Parameters  | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/id">ID</a>  | Identifier of the Transfer CFT command selected using the TYPE parameter.<br/> Used to limit the query to this identifier. |
| <a href="../../../command_summary/parameter_intro/part">PART</a> <br/> For TYPE = IDF | Partner identifier.<br/> Used to limit the search to the IDFs defined in the CFTIDF objects, relative to this partner. |
| <a href="../../../command_summary/parameter_intro/type">TYPE</a> | Defines the type of object to be selected.<br/> TYPE can take the predefined values indicated in the <a href="#Type_table">Type table</a>. |


****Example****

```
LISTPARM       TYPE
= ALL
```

Displays all the parameters contained in the PARAMETER file.

```
LISTPARM         TYPE
= SEND
```

Displays the parameters of all the CFTSEND objects configured.

<span id="Type_table"></span>

#### Type table

TYPE can take the predefined values indicated in the table below.


| Parameter  | Description  |
| --- | --- |
| ACCNT  | Used to query statistical file parameters.<br /> These parameters are submitted when CFTACCNT objects are entered. |
| ALL  | Used to query all the parameters indicated in the PARAMETER file . |
| AUTH  | Used to query file authorization lists.<br /> These lists are customized by the CFTAUTH objects.  |
| CAT  | Used to query catalog parameters.<br /> These parameters are submitted when CFTCAT objects are entered. |
| COM  | Used to query communication media parameters.<br /> These parameters are submitted when CFTCOM objects are entered  |
| IDF  | Used to query file "network" identifiers.<br /> Identifiers are customized by the CFTIDF objects.  |
| LOG  | Used to query log file parameters.<br /> These parameters are submitted when CFTLOG objects are entered.  |
| NET  | Used to query network characteristic parameters.<br /> These parameters are submitted when CFTNET objects are entered and differ according to the type of network configured.  |
| PARM  | Used to query general parameters.<br /> These parameters are submitted when CFTPARM objects are entered.  |
| PROT  | Used to query protocol parameters.<br /> These parameters are submitted when CFTPROT objects are entered and differ according to the protocol configured.  |
| RECV  | Used to query the parameters of the files to be received.<br /> These parameters are submitted when CFTRECV objects are entered.  |
| SEND  | Used to query the parameters of the files to be sent.<br /> These parameters are submitted when CFTSEND objects are entered. |
| XLATE  | Used to query translation tables.<br /> Translation tables are customized by the CFTXLATE objects.  |

