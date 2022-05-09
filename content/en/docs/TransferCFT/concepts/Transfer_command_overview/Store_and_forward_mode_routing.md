{
    "title": "Store  and forward concepts",
    "linkTitle": "Store and forward modes",
    "weight": "260"
}The store and forward mode enables you to route files from one computer
to another via one or more intermediate relays (computers or communication sites). For example, when sending a file from computer A to computer C, where A and C are either not directly connected or have a connection problem, the file is routed via computer B.

In the same way, a file to be sent to clients that are dependent on
the final receiver may transmit via intermediate systems and be sent on. This transfer is accomplished using the same protocol from one end to
another.

> **Note**
>
> Note: This document describes the relay process as it relates to Transfer CFT. You can perform relay transfers using other Axway products as the intermediary site.

> **Note**
>
> Note: If you are using access management, you must define the CFTAPPL with the ID=COMMUT.

The following illustration features 3 Transfer CFTs, where the protocol may be the same or different between relay points:

- Source [Initial Sender A]
- Relay [Store and Forward B]
- Target [Final Receiver C]

**Routing a transfer via a relay**

![](/Images/TransferCFT/temp_store.png)

Restrictions
------------

- You cannot use a distribution list on the relay site when using {{< TransferCFT/suitevariablesFlowManager  >}} or {{< TransferCFT/suitevariablesCentralGovernanceName  >}}.

<!-- -->

- {{< TransferCFT/axwayvariablesComponentLongName  >}} store and forward mode is only possible from a requester/sender (write transfers only, not read).

Store and forward mode protocols
--------------------------------

To route a file transfer, you must identify the partners involved
in the transfer, the initial sender and final receiver of the transfer.

{{< TransferCFT/axwayvariablesComponentLongName  >}} supports the following protocols for store and forward operations:

- PeSIT
- Odette (OFTP)

For these protocols, the objects transferred can be files or messages,
in write mode only, for sender requester mode.

These protocols manage the acknowledgement messages following
the reception of a file. See the SEND TYPE = [REPLY](../../send_command/send_replies) command. The PeSIT
profile protocol also manages the sending of messages.
See the SEND TYPE = [MESSAGE](../../send_command/send_messages_cl) command.

Using store and forward with Flow Manager
-----------------------------------------

If you are using {{< TransferCFT/suitevariablesFlowManager  >}} or {{< TransferCFT/suitevariablesCentralGovernanceName  >}} to manage your Transfer CFT flows, the store and forward functionality may also be referred to as a relay in the flow. Please refer to the [Transfer CFT store and forward](https://docs.axway.com/bundle/FlowManager_20_allOS_en_HTML5/page/transfer_cft_store_and_forward.html) page in the {{< TransferCFT/suitevariablesFlowManager  >}} {{< TransferCFT/suitevariablesDocTypeUser  >}}.

Using store and forward with standalone {{< TransferCFT/PrimaryTransferCFTplural  >}}
------------------------------------------------------------------------------------------

If you are using {{< TransferCFT/suitevariablesTransferCFTName  >}} without additional governance, you can manage store and forward as described below.

### Setting the COMMUT value

The processing possibilities for a store and forward site depend on
the value of the COMMUT parameter in the CFTPART object. This parameter
is associated with the sending partner that is defined on the store and
forward site.

Depending on the value of this parameter, the processing performed by
the {{< TransferCFT/axwayvariablesComponentShortName  >}} on the store and forward site is as follows:


| COMMUT value  | File is sent to partner  | Details  |
| --- | --- | --- |
| YES  | Yes, immediately  | The a file is immediately sent to the intended partner. Default value.  |
| NO  | No, no file forwarding  | The file transfer is refused because the partner is not able to perform the store and forward.  |
| SERVER  | Yes, after processing  | Sending the file occurs at the initiative of the store and forward site. This mode is also known as [Store and forward with a VAN server](#VAN_server_Store_and_forward_processing).  |
| PART  | Yes, immediately  | This forced store and forward occurs in server mode. If the recipient that is defined in the IPART parameter is not the final recipient, the received file is immediately sent on to the target partner.  |


There are two ways for the sender to initiate a store and forward transfer:

- Define the final partner in the sender's configuration and use only the partner in the SEND command. Using this method, the final partner definition includes both the relay  (IPART) and has the OMINTIME and OMAXTIME values set to 0.  

    `cftpart id=<FINAL PARTNER>,  ipart=<RELAY>, omintime=0, omaxtime=0,...`

    `send part=<FINAL PARTNER>,...`

<!-- -->

- Do not define the final partner in the sender's configuration. Using this method, you declare the final partner (ID) and the relay (IPART) when you execute the SEND command.

<!-- -->

- `send part=<FINAL PARTNER>, ipart=<RELAY>,...`

<span id="Store_and_forward_sites"></span>

### Store and forward processing in standalone mode COMMUT=YES/PART

This section details the processing steps as described above in *Setting the COMMUT value.*

#### On the store and forward site

The following actions apply to the store and forward site:

- During the file transfer (forwarding), Transfer CFT does not
    activate an end of transfer procedure or an error procedure.
- If the transfer IDF is set to the value COMMUT, there must be a command CFTRECV ID =
    COMMUT defined on this site. The transfer is then
    saved in the catalog with the IDF = COMMUT file identifier.
- When forwarding the file, the compression factor may be transformed
    as a function of the parameter settings of the parties (sender, store
    and forward site, receiver) according to the values of the RCOMP/SCOMP
    parameters of the corresponding CFTPROT commands. However, no translation is
    performed.
- The file created on the store and forward site is "temporary"
    and is automatically deleted by Transfer CFT once it has been correctly
    sent to the next recipient (or to the next intermediate computer if applicable).

#### On the final receiver site

The following actions apply to the final recipient site:

- Transfer CFT receives the following application parameters,
    conveyed without modification from the initial sender:
    -   PARM, SUSER, RUSER,
        SAPPL, RAPPL
    -   IDT
    -   Received file characteristics
        (NTYPE, NBLKSIZE, NLRECL, for example)
    -   File date and time
        (FDATE, FTIME without the hundredths of seconds)
- The physical file name on the receiving site can be specified by the
    initial sender, through the SEND PART = C, NFNAME = filename command,
    if the recipient in question accepts open mode operation. The store and
    forward site is not involved in this mechanism.
- Once the file or message is received, an acknowledgement message
    (SEND TYPE = REPLY) can be sent to the initial sender.

#### On the initial sender and final receiver

On completion of transfer,
or in the event of error, Transfer CFT offers the possibility of executing
procedures. You can use the typical [symbolic variables](../../../c_intro_userinterfaces/command_summary/symbolic_variables) for these types of procedures.

****Processing possibilities in the store and forward mode****

****![](/Images/TransferCFT/s_and_f_processing.PNG)****

### Forced store and forward processing with COMMUT=PART

****PeSIT protocol only****

You can use the use this option to force a store and forward on an intermediate site without knowing the final partner.

On the store and forward site there is a partner definition to receive the incoming connection and a second partner establishes the connection with the following site (also the intermediate or the final site). The link between the partner receiving the connection and the sending partner is established via the following parameter settings:

```
CFTPART ID=IDRECEPT, COMMUT=PART, IPART=IDEMET,...
CFTPART ID=IDEMET,...
```

The initial site establishes the connection with the immediate store and forward site (CFTPART ID=IDNAT, NSPART=NINTNAT, NRPART=NNAT, and so on). The immediate store and forward site receives the connection from the initial site and forces the store and forward (COMMUT=PART) to the indicated partner (IPART=IDDEP).

This process repeats as many times as needed until reaching the final site.

![](/Images/TransferCFT/temp_commut_part.png)

The REPLY command can be sent when the end-of-transfer procedure is executed (EXECRF). An acknowledgement message can be transferred also via all the intermediate sites until it reaches the initial partner (IPART).

<span id="VAN_server_Store_and_forward_processing"></span>

### Store and forward processing by a VAN server with COMMUT=SERVER

Once the file is received, {{< TransferCFT/axwayvariablesComponentShortName  >}} does not immediately forward
the file. User processing may first be performed on the file on the Value Added Network (VAN server). In
particular, {{< TransferCFT/axwayvariablesComponentShortName  >}} activates the end of transfer procedure if one is defined.

The IDF of the received transfer is deduced from the NIDF sent by the
sender.

The store and forward site (VAN server) is responsible for forwarding the file to
the final receiver. This may, for example, be accomplished during the
end of transfer procedure, after the associated processing and checks.
The file is forwarded using the SEND
command (coupled with a CFTSEND command). The SPART parameter of the SEND
command sets the value of the sender NSPART parameter to the value of
the INITIAL sender of the file (refer to the [SEND]() command).

Instead of returning the file, the store and forward site (VAN server) can make
the file available to the final receiver (SEND command ... , STATE = HOLD).

The final receiver takes the file:

- Either after sending
    the CD in ODETTE protocol
- Or through a receive
    command (RECV) for other protocols

If the final recipient requests the reception of the file, the following
must be indicated as a partner in the RECV command:

- The store and forward
    site in ODETTE protocol
- The initial sender
    in PeSIT protocol

The difference lies in the type of protocols.

> **Note**
>
> Note: entries and that these entries are linked. In the example above, there would be two entries for the single "report" transfer.

<span id="Store"></span>

Broadcasting on a store and forward site
----------------------------------------

This section describes how to use a partner broadcasting list with store and forward.

To broadcast a file from a store and forward site:

- The initial sender must define a virtual partner with an ID that corresponds to the CFTDEST ID command managed on the store and forward site. Set the CFTPART's OMINTIME and OMAXTIME to zero to force routing to the intermediate partner (IPART). The SEND PART=ID, ... command sends the file to broadcast.
- You must have a CFTDEST command with the ID set to the network name of the broadcasting list indicated by the initial partner (SEND PART=ID).
- You can use FOR=COMMUT as described in the [FOR](../../../c_intro_userinterfaces/command_summary/parameter_intro/for) parameter.
- The final receivers know the initial file sender and the store and forward partner (relay).
- The CFTPART connections must comply with network nodes.

### Processing performed

On the store and forward site, Transfer CFT does not activate an end of transfer procedure (or error) when the file transfer of the file to be broadcast is performed. The transfer IDF is set to COMMUT, where the CFTRECV ID=COMMUT must be defined on the site as the transfer is saved in the catalog with this file identifier.

If the transferred data code (NCODE) differs from the store and forward site's data code (for example, NCODE = EBCDIC and ASCII internal code on the store and forward computer), the data is not translated on the store and forward computer (in “store and forward” mode, the data is only translated “end to end” and not “next computer to next computer”).

When all the transfers have been correctly completed, the generic transfer (virtual) associated with the broadcast (entry designated in the catalog by a “DIAGP” code equal to “DIFFUS”) changes to the T state. Transfer CFT then activates any end of transfer procedure associated with this generic transfer.

> **Note**
>
> Caution  
> Unlike a simple transfer in store and forward mode, the file created on the intermediate site is not deleted. This deletion may be handled by the end of transfer procedure, since the &DIAGP variable is used to determine whether the transfer is a broadcast (DIAGP = DIFFUS).

On the final receiver site: the CFT monitor receives the same application parameters as those indicated for a simple transfer in “store and forward” mode. The store and forward site does not affect the transfer mode (open or closed).

On completion of file or message reception, an acknowledgement message (SEND TYPE = REPLY) may be sent to the initial sender.

On the initial sender and final receiver sites: end of transfer or error procedures may be initiated during a transfer.

The usual symbolic variables for these types of procedures may be used (see the “Symbolic variables” paragraph).

### Example

This example shows a broadcast store and forward from the initiator A to relay B on to multiple partners C and D. We will use @A, @B, @C and @D to represent their respective host addresses.

On the initiating site A, define:

```
cftpart id=cd, nspart=a, ipart=b, omintime=0, omaxtime=0,prot=pesitssl
cftpart id=b,nspart=a,prot=pesitssl,sap=1762
cfttcp id=b,host=@B
```

Set up the intermediate partner B as follows:

```
cftpart id=a,nspart=b,prot=pesitssl,sap=1762
cfttcp id=a,host=@A
 
cftpart id=c,nspart=b,prot=pesitssl,sap=1762
cfttcp id=c,host=@C
 
cftpart id=d,nspart=b,prot=pesitssl,sap=1762
cfttcp id=d,host=@D
 
cftdest id=cd,part=(c,d),for=commut
 
cftappl id=commut,userid=&userid,groupid=&groupid NOTE: If you are using access management, you must define the CFTAPPL with the ID=COMMUT.
```

Execute the following partner C definition:

```
cftpart id=b,nspart=c,prot=pesitssl,sap=1762
cfttcp id=b,host= @B
cftrecv id=broadcast,fname=pub/broadcast.rcv,faction=delete
 
cftpart id=a,nspart=c, ipart=b, omintime=0, omaxtime=0,prot=pesitssl,sap=1762
```

Execute the following partner D definition:

```
cftpart id=b,nspart=d,prot=pesitssl,sap=1762
cfttcp id=b,host=@B
cftrecv id=broadcast,fname=pub/broadcast.rcv,faction=delete
 
cftpart id=a,nspart=c, ipart=b, omintime=0, omaxtime=0,prot=pesitssl,sap=1762
 
```

****Testing the use case****

From the initiator site A, execute:

```
send part=cd,idf=broadcast,fname=pub/FTEST
```

Broadcast list acknowledgements
-------------------------------

To acknowledge a store and forward file (or message) transfer, the final partner (or partners) sends(send) a TYPE=REPLY message to the initial partner (the zero values of the OMINTIME and OMAXTIME parameters of the associated CFTPART command force the routing of the REPLY via the intermediate partner B IPART=B).

To acknowledge a broadcasting list, the following conditions are then required to route the acknowledgement to the initial partner:

- The connections established between partners must be complied with at each of the network nodes (CFTPART ID =...),
- At the store and forward node, all the catalog records corresponding to the previously performed broadcasting, are present,
- At the store and forward node, all the catalog records corresponding to the previously performed broadcasting, indicate that the receiving partners have correctly received the file or message (SFT transfer state).

If these conditions are fulfilled, the initial sender of the file (or message) receives a single acknowledgement message. The message comes arbitrarily from one of the final partners (the last one to have sent a REPLY message).
