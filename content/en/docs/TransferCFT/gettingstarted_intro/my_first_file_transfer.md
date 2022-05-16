---

    title: My first file transfer (CL)
    linkTitle: First file transfer with standalone Transfer CFT
    weight: 100

---
After installing your Transfer CFT, you can use the delivered configuration samples and default values to quickly and easily perform your first transfer.

For more information on starting your {{< TransferCFT/axwayvariablesComponentShortName  >}}, and basic operational commands, see the section [Start, stop, and {{< TransferCFT/axwayvariablesComponentShortName  >}} administrative scripts.](../../admin_intro/start_stop_cft)

This topic describes how to:

- [Perform a file transfer using the sample configuration](#Perform)
- [Perform a standard mode transfer](#Perform2)
- [Use the explicit mode to get a file](#Use)
- [Receive multiple files from a partner](#Receive)
- [Use the implicit transfer mode](#Use2)
- [Send using open mode](#Send)
- [Receive using open mode](#Receive2)
- [Perform broadcast and collect operations](#Perform3)

<span id="Perform"></span>

## Perform a file transfer using the sample configuration

The sample file features two default partners, NEWYORK and PARIS. Each of these partners is pre-configured to communicate using the [PeSIT](../../protocols_start_here/about_pesit) protocol. Additionally, both of these partners listen on the localhost interface. You can begin with a simple transfer, and then check the catalog for transfer details.

To send a file from PARIS to NEWYORK, run the following command:

```
CFTUTIL send part=NEWYORK, idf=txt
```

To check in the Transfer CFT catalog to confirm that the transfer completed successfully, enter:

```
CFTUTIL listcat
```

You should see that the default file was sent from NEWYORK to PARIS. There are two entries are displayed, this is due to the fact that you performed a loop transfer using the localhost interface.

```
Partner  DTSAPP File     Transfer         Records       Diags        Appli.   Appstate.
                Id.      Id.       Transmit     Total   CFT Protocol Id.
-------- ------ -------- -------- ---------- ---------- --- -------- -------- ---------
NEWYORK  SFX XX TXT      J2220582        112        112   0 CP NONE
PARIS    RFX XX TXT      J2220582        112        112   0 CP NONE
```

To display exhaustive transfer details, enter the command:

```
CFTUTIL listcat content=debug
```

The purpose of the My first file transfer section is to help you feel comfortable with basic {{< TransferCFT/axwayvariablesComponentShortName  >}} file transfer commands. Once you understand core file transfer concepts, you can delve into the rich array of parameters that allow you to customize your application integrations and data flows. Additional commands and options are available to help you define the monitoring granularity  for your executed transfers.

## What's next?

In the following sections, we'll take a look at additional {{< TransferCFT/axwayvariablesComponentShortName  >}} transfer modes, as well as some useful configuration parameters. With Transfer CFT, the transfer initiator can be either the sender of the file or the receiver, as indicated in the examples below. Additionally, in these examples we will use the convention that the requester is the client, so the transfer description may read <span class="bold_in_para">****Requester/Sender****</span> if the client is supplying the file.

> **Note**
>
> For more information on a command and a list of available parameters, enter CFTUTIL help cmd=&lt;name of command>.

<span id="Perform2"></span>

### Perform a standard mode transfer

Let's start by performing the same type of transfer as in the sample configuration above, but without using the samples. First, you need to create two partners, and then exchange a file.

![In this example, the Requester Paris site sends file to the Server New York site](/Images/TransferCFT/2013_g_TransferCFT_Standard_mode.png)

 


| Requester/Sender  | Server/Receiver  |
| --- | --- |
| **Configuration**<br/> • Create a partner.<br/> • Create an IDF.<br/> **Runtime**<br/> • Run the transfer.<br/> • Check the catalog. | **Configuration**<br/> • Create a partner.<br/> • Create an IDF.<br/> **Runtime**<br/> • Retrieve the sent file.<br/> • Check the catalog. |


****View an example****

```
**/\*REQUESTER/SENDER\*/**
CFTPART
ID = NEWYORK,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for NewYork\*/</span>
NRPART = NEWYORK,
NSPART = PARIS,
 
CFTTCP
ID=NEWYORK,
HOST = @<NEWYORK address>
 
CFTSEND ID=INVOICE,...
 
SEND PART=NEWYORK, IDF=INVOICE
 
LISTCAT
```

 

```
**/\*SERVER/RECEIVER\*/**
CFTPART
ID = PARIS,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for Paris\*/</span>
NRPART = PARIS,
NSPART = NEWYORK,
 
CFTTCP
ID=PARIS,
HOST = @<PARIS address>
 
CFTRECV ID=INVOICE, FNAME=MYFILE
 
LISTCAT
```
<span id="Use"></span>

### Use the explicit mode to get a file

In explicit mode, an application makes a specific file available for a defined partner. Then when that particular remote partner is ready, it can retrieve the specified file, which only needed to be made available once.

![In this example, the Sender site Phoenix has a file available for the Requester Paris site](/Images/TransferCFT/2013_g_TransferCFT_Explicit_mode.png)

 


| Server/Sender  | Requester/Receiver  |
| --- | --- |
| Configuration<br/> • Create a partner.<br/> • Create an IDF.<br/> <br/> <br/> Runtime<br/> • Make your file available.<br/> The send command is set with the ‘state=HOLD’ parameter. The HOLD attribute puts the file in the Transfer CFT catalog, and indicates that it is available for the partner.<br/><br/> • Check the catalog. | Configuration<br/> • Create a partner.<br/> • Create an IDF.<br/> <br/> <br/> Runtime<br/> • Receive the available file.<br/> • Check the catalog. |


****View an example****

```
**/\*SERVER/SENDER\*/**
CFTPART
ID = PARIS,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for Phoenix\*/</span>
NSPART = PHOENIX,
NRPART = PARIS,
CFTTCP
ID=PARIS,
HOST = @<Paris address>
 
CFTSEND ID=INVOICE, IMPL=NO,....
 
SEND PART=PARIS, IDF=INVOICE, STATE=HOLD
 
LISTCAT <span style="font-size: 8pt;">/\*show transfers in hold state\*/</span>
```
```
**/\*REQUESTER/RECEIVER\*/**
CFTPART
ID = PHOENIX,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;">/\* remote listening port for Paris \*/</span>
NSPART = PARIS,
NRPART = PHOENIX,
CFTTCP
ID=PHOENIX,
HOST = @<Phoenix address>
 
CFTRECV ID=INVOICE,....
 
RECV PART=PHOENIX, IDF=INVOICE, FNAME=MY_FILE
 
LISTCAT
```
<span id="Receive"></span>

### Receive multiple files from a partner

This transfer mode is the same as the previously described explicit mode but provides multiple files for a defined partner. So an application might create several files and set them to an available state, and the remote partner can then retrieve these when ready, for example at a scheduled time.

![In this example, the Sender Phoenix site has multiple files availalbe for the Requester site](/Images/TransferCFT/2013_g_TransferCFT_Multiple_receive.png)

 


| Server/Sender  | Requester/Receiver |
| --- | --- |
| Configuration<br/> • Create a partner.<br/> • Create an IDF.<br/> Runtime<br/> • Make a file available SEND part=PARIS, idf=INVOICE, state=HOLD.<br/> • Repeat with another file.<br/> • Repeat again with a third file. This ‘HOLD’ attribute puts the files in the Transfer CFT catalog and makes them available for the partner.<br/> • Check the catalog for 1 entry per transfer. A generic entry is set. | Configuration<br/> • Create a partner.<br/> • Create an IDF.<br/> Runtime<br/> • Receive all of the available files:<br /> RECV part=PHOENIX, idf=INVOICE, file=ALL.<br/> • Check the catalog for 1 entry per transfer. A generic entry is set. |


****View an example****

```
**/\*SERVER/SENDER\*/**
CFTPART
ID = PARIS,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for Phoenix\*/</span>
NSPART = PHOENIX,
NRPART = PARIS,
CFTTCP
ID=PARIS,
HOST = @<Paris address>
 
CFTSEND ID=INVOICE, IMPL=NO,....
 
SEND PART=PARIS, IDF=INVOICE, STATE=HOLD, FNAME=FILE_1
SEND PART=PARIS, IDF=INVOICE, STATE=HOLD, FNAME=FILE_2
SEND PART=PARIS, IDF=INVOICE, STATE=HOLD, FNAME=FILE_n
 
LISTCAT <span style="font-size: 8pt;">/\*would show transfer in hold state\*/</span>
```
```
**/\*REQUESTER/RECEIVER\*/**
CFTPART
ID = PHOENIX,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;">/\* remote listening port for Paris \*/</span>
NSPART = PARIS,
NRPART = PHOENIX,
CFTTCP
ID=PHOENIX,
HOST = @<Phoenix address>
 
CFTRECV ID=INVOICE, FNAME=@IDTU.RCV,...
 
RECV PART=PHOENIX, IDF=INVOICE, FILE=ALL
 
LISTCAT
```
<span id="Use2"></span>

### Use the implicit transfer mode

The implicit transfer mode is often used to make a file whose content is frequently changing available to partners. The file is always available and partners can retrieve it as many time as necessary.

![In this example, the Sender Phoenix site has a file whose contents may be frequently updated availbe](/Images/TransferCFT/2013_g_TransferCFT_Implicit_mode.png)

 


| Server/Sender  | Requester/Receiver  |
| --- | --- |
| Conf<br/> • Create a partner.<br/> • Create an IDF where impl=yes.<br/> Runtime<br/> • The CFTSEND is set with ‘impl=yes’.<br/> • Check the catalog. | Conf<br/> • Create a partner.<br/> • Create an IDF.<br/> Runtime<br/> • Receive the file.<br/> • Check the catalog. |


****View an example****

```
**/\*PHOENIX\*/**
CFTPART
ID = PARIS,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for Phoenix\*/</span>
NSPART = PHOENIX,
NRPART = PARIS,
CFTTCP
ID=PARIS,
HOST = @<Paris address>
 
CFTSEND ID=ORDER, IMPL=YES, FNAME=FILE_TO_SEND....
 
 
LISTCAT <span style="font-size: 8pt;">/\*would show transfer in hold state\*/</span>
```

 

```
**/\*PARIS\*/**
CFTPART
ID = PHOENIX,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;">/\* remote listening port for Paris \*/</span>
NSPART = PARIS,
NRPART = PHOENIX,
CFTTCP
ID=PHOENIX,
HOST = @<Phoenix address>
 
CFTRECV ID=ORDER,....
 
RECV PART=PHOENIX, IDF=ORDER, FNAME=MY_FILE
 
LISTCAT
```
<span id="Send"></span>

### Send using open mode

This mode is similar to the FTP put command. It allows you to define the file name in the remote partner. The receiver must accept the open mode. Then, using a simple SEND command allows you to send a file to a partner in a dedicated directory with a dedicated file name.

![In this example, the Sender New York site is sending a file to a partner that receives in open mode](/Images/TransferCFT/2013_g_TransferCFT_Open_mode_send.png)

 


| Requester/Sender  | Server/Receiver  |
| --- | --- |
| Conf<br/> • Create a partner.<br/> • Create a CFTSEND with fname=&lt;FILE_TO_SEND&gt;, nfname=cft/filpub/fic.txt …<br/> Runtime<br/> • Run the transfer.<br/> • Check the catalog. | Conf<br/> • Create a partner.<br/> • Create an CFTRECV fname=&amp;nfname…<br/> The syntax ‘fname=&amp;nfname’ means that the receiver allows the open mode.<br/><br/> Runtime<br/> • The file has been stored in the path defined by the client/sender, in this example: cft/filpub/fic.txt.<br/> • Check the catalog. |


****View an example****

```
**/\*REQUESTER/SENDER\*/**
CFTPART
ID = NEWYORK,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for NewYork\*/</span>
NRPART = NEWYORK,
NSPART = PARIS,
 
CFTTCP
ID=NEWYORK,
HOST = @<NEWYORK address>
 
CFTSEND ID=INVOICE,FNAME=FILE_TO_SEND, NFNAME=<remote_location_pathname>
 
SEND PART=NEWYORK, IDF=INVOICE
 
LISTCAT
```

 

```
**/\*RECEIVER/SERVER\*/**
 
CFTPART
ID = PARIS,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for Paris\*/</span>
NRPART = PARIS,
NSPART = NEWYORK,
 
CFTTCP
ID=PARIS,
HOST = @<PARIS address>
 
CFTRECV ID=INVOICE, FNAME=MYFILE,FNAME=&NFNAME
 
LISTCAT
```
<span id="Receive2"></span>

### Receive using open mode

This mode is similar to FTP get command. It allows the receiver to get a file from the sender in a dedicated location.  The receiver must accept the open mode. Then, a simple RECV command allows to get a file from a partner from a dedicated directory with a dedicated file name.

![In this example, the Receiver site gets a file from the Sender](/Images/TransferCFT/2013_g_TransferCFT_Open_mode_receive.png)

 


| Requester/Receiver | Server/Sender  |
| --- | --- |
| Configuration<br/> • Create a partner.<br/> • Create an CFTRECV that defines the fname.<br/> Runtime<br/> • Request the file stored on the remote server. Run the command: CFTUTIL RECV… nfname=&lt;remote_requested_file&gt;.<br/> • The file retrieved is stored in the fname location.<br/> • Check the catalog. | Configuration<br/> • Create a partner.<br/> • Create an CFTSEND where impl=yes, fname=&amp;nfname.<br/> The syntax ‘fname=&amp;nfname’ means that the server/sender allows the open mode.<br/><br/> Runtime<br/> • The file stored locally in &lt;remote_requested_file&gt; is sent to the client.<br/> • Verify the catalog |


****View an example****

```
**/\*REQUESTER/RECEIVER\*/**
CFTPART
ID = NEWYORK,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for NewYork\*/</span>
NRPART = NEWYORK,
NSPART = PARIS,
 
CFTTCP
ID=NEWYORK,
HOST = @<NEWYORK address>
 
CFTRECV ID=INVOICE, FNAME=<local_download_location>
 
RECV PART=NEWYORK, IDF=INVOICE, NFNAME=<remote_requested_file>
 
LISTCAT
```

 

```
**/\*SERVER/SENDER\*/**
CFTPART
ID = PARIS,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for Paris\*/</span>
NRPART = PARIS,
NSPART = NEWYORK,
 
CFTTCP
ID=PARIS,
HOST = @<PARIS address>
 
CFTSEND ID=INVOICE, IMP=YES,FNAME=&NFNAME
 
LISTCAT
```
<span id="Perform3"></span>

### Perform broadcast and collect operations

Broadcast and collect modes allow you to send files to multiple partners and receiving files from multiple partners using a single command request for each.

#### Broadcasting

You can use broadcasting to send a file to an entire group of partners in one command, avoiding the task of sending the selected file individually to each of the partners in a flow.

The name of the broadcast list is itself a virtual partner, which contains a list of partners in your flow. This list of partners can be explicit, or it can reference a file that contains the list of partners.

Additionally, you can define what occurs if a partner is unknown, how the scripts are applied to the broadcast list, and so on.

[More information](../../concepts/transfer_command_overview/broadcast_collect)...

![In this example, the Sender Paris site sends a file to an entire group of partners in one command](/Images/TransferCFT/2013_g_TransferCFT_Broadcast1.png)

 


| Requester/Sender  | Server/Receiver – PHOENIX  | Server/Receiver – NEWYORK  |
| --- | --- | --- |
| Configuration<br/> • Create a first partner NEWYORK.<br/> • Create a second partner PHOENIX.<br/> • Create a broadcast list that contains both part1 and part2.<br/> • Create a CFTSEND.<br/> Runtime<br/> • Using one command, send a file to the broadcast list.<br/> • The file is sent to both partners at the same time.<br/> • Check the catalog. You should have 3 entries in the catalog, one generic entry and one entry per partner. | Configuration<br/> • Create a first partner.<br/> • Create an CFTRECV.<br/> Runtime<br/> • The first partner receives the file.<br/> • Check the catalog, there should be get one entry for this partner. | Configuration<br/> • Create a second partner.<br/> • Create an CFTRECV.<br/> Runtime<br/> • The second partner receives the file.<br/> • Check the catalog, there should be get one entry for this partner.<br/> <br/>  |


****View an example****

```
/\*SENDER\*/
CFTPART
ID = PHOENIX,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for Phoenix\*/</span>
NSPART = PARIS,
NRPART = PHOENIX,
CFTTCP
ID=PHOENIX,
HOST = @<PHOENIX address>
 
CFTPART
ID = NEWYORK,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for New York\*/</span>
NSPART = PARIS,
NRPART = NEWYORK,
CFTTCP
ID=NEWYORK,
HOST = @<NEWYORK address>
 
CFTDEST ID=DEST01, PART=NEWYORK, PHOENIX
 
CFTSEND ID=ORDER
 
SEND PART=DEST01, IDF=ORDER
 
LISTCAT
```

 

```
**/\*RECEIVER 1\*/**
CFTPART
ID = PARIS,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;">/\* remote listening port for Paris \*/</span>
NSPART = PHOENIX,
NRPART = PARIS,
CFTTCP
ID=PARIS,
HOST = @<PARIS address>
 
CFTRECV ID=ORDER,....
 
LISTCAT
```

 

```
**/\*RECEIVER 2\*/**
CFTPART
ID = PARIS,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;">/\* remote listening port for Paris \*/</span>
NSPART = NEWYORK,
NRPART = PARIS,
CFTTCP
ID=PARIS,
HOST = @<PARIS address>
 
CFTRECV ID=ORDER,....
 
LISTCAT
```

#### Collecting

Collecting files is the inverse of using a broadcast list. In the collect transfer mode you can receive a dedicated file from multiple partners (P*n*). This allows the receiver, or flow initiator, to receive a file from all defined partners using a single request command.

More information...

![In this example, the Receiver Paris site collects or receives a dedicated file from multiple partners](/Images/TransferCFT/2013_g_TransferCFT_Collect1.png)


| Client/Receiver  | Server/Sender<br/> PHOENIX | Server/Sender<br/> NEW YORK |
| --- | --- | --- |
| Configuration<br/> • Create a first partner.<br/> • Create a second partner.<br/> • Create a collect list containing both NEW YORK and PHOENIX.<br/> • Create a CFTRECV.<br/> Runtime<br/> • Using a single command, receive a file from both (all) partners using the defined collect list.<br/> • Both files are received from both partners at the same time<br/> • Check the catalog. You should have 3 entries in the catalog, one generic entry and one entry per partner. | Configuration<br/> • Create this partner.<br/> • Create an CFTSEND.<br/> Runtime<br/> • This partner makes a file available for the client SEND state=HOLD.<br/> • The first partner sends the file.<br/> • Check the catalog, there should be one entry. | Configuration<br/> • Create a this partner.<br/> • Create an CFTSEND.<br/> Runtime<br/> • This partner makes a file available for the client SEND state=HOLD.<br/> • The second partner sends the file.<br/> • Check the catalog, there should be one entry. |


****View an example****

```
**/\*RECEIVER\*/**
CFTPART
ID = PHOENIX,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for Phoenix\*/</span>
NSPART = PARIS,
NRPART = PHOENIX,
CFTTCP
ID=PHOENIX,
HOST = @<PHOENIX address>
 
CFTPART
ID = NEWYORK,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;"><span style="font-size: 8pt;">/\*</span>remote listening port for New York\*/</span>
NSPART = PARIS,
NRPART = NEWYORK,
CFTTCP
ID=NEWYORK,
HOST = @<NEWYORK address>
 
CFTDEST ID=DEST01, PART=NEWYORK, PHOENIX
 
CFTRECV ID=ORDER
 
RECV PART=DEST01, IDF=ORDER, FNAME=<...>&part
 
LISTCAT
```

 

```
**/\*SENDER 1\*/**
CFTPART
ID = PARIS,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;">/\* remote listening port for Paris \*/</span>
NSPART = PHOENIX,
NRPART = PARIS,
CFTTCP
ID=PARIS,
HOST = @<PARIS address>
 
CFTSEND ID=ORDER,...
 
SEND IDF=ORDER, STATE=HOLD
 
LISTCAT
```

 

```
**/\*SENDER 2\*/**
CFTPART
ID = PARIS,
PROT = PESITANY,
SAP = 1761, <span style="font-size: 8pt;">/\* remote listening port for Paris \*/</span>
NSPART = NEWYORK,
NRPART = PARIS,
CFTTCP
ID=PARIS,
HOST = @<PARIS address>
 
CFTSEND ID=ORDER,...
 
SEND IDF=ORDER, STATE=HOLD
 
LISTCAT
```

> **Note**
>
> In these examples we created partners using the default MODE value, which is REPLACE. You can also use the MODE=CREATE, to create a new Transfer CFT partner.

#### Additional information

Once you understand the basic modes and concepts described in this topic, you can then add processing, symbolic variables, scripts and more to your transfers using other {{< TransferCFT/axwayvariablesComponentShortName  >}} options and features. See the dedicated sections in this document for details on customizing your transfer flows. A good place to start is [Transfer Concepts](../../concepts/transfer_command_overview), which presents high-level transfer processing concepts, transfer mode details, and procedural topics.
