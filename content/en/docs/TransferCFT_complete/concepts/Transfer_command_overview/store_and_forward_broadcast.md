{
    "title": "Broadcasting on a store and forward site",
    "linkTitle": "Store and forward broadcasts",
    "weight": "270"
}This section describes how to use a partner broadcasting list with store and forward.

To broadcast a file from a store and forward site:

- The initial sender must define a virtual partner with an ID that corresponds to the CFTDEST ID command managed on the store and forward site. Set the CFTPART's OMINTIME and OMAXTIME to zero to force routing to the intermediate partner (IPART). The SEND PART=ID, ... command sends the file to broadcast.
- You must have a CFTDEST command with the ID set to the network name of the broadcasting list indicated by the initial partner (SEND PART=ID).
- You can use FOR=COMMUT as described in the [FOR](../../../c_intro_userinterfaces/command_summary/parameter_intro/for) parameter.
- The final receivers know the initial file sender and the store and forward partner (relay).
- The CFTPART connections must comply with network nodes.

#### Processing performed

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

#### Example

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
 
cftpart id=c,nspart=b,prot=pesitssl,sap=31762
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
