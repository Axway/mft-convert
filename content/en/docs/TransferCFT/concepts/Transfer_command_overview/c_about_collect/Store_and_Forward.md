{
    "title": "Store  and forward ",
    "linkTitle": "Store and forward relays",
    "weight": "340"
}Store and forward, or transfer routing, allows you to define and automate file transfer using an intermediate site. This page describes using store-and-forward services either in a {{< TransferCFT/suitevariablesCentralGovernanceName  >}} context or using a standalone {{< TransferCFT/suitevariablesTransferCFTName  >}}.

- [Store and forward with {{< TransferCFT/suitevariablesCentralGovernanceName  >}}](#Store)
- [Store and forward standalone {{< TransferCFT/suitevariablesTransferCFTName  >}}](#Store2)
    -   [Intentional
        store and forward](#Intentional_Store_and_Forward)
    -   [Intentional VAN store and forward](#Intentional_VAN_store_and_forward)
    -   [Forced
        store and forward](#Forced_Store_and_Forward)

<span id="Store"></span>

Store and forward with {{< TransferCFT/suitevariablesCentralGovernanceName  >}}
------------------------------------------------------------------------------------

When implementing a {{< TransferCFT/suitevariablesTransferCFTName  >}} relay in {{< TransferCFT/suitevariablesCentralGovernanceName  >}}, the only relay option is the equivalent of COMMUT = YES, where a file is immediately sent to the intended final partner.

To implement a relay in conjunction with {{< TransferCFT/PrimaryCGorUM  >}}:

1. Create the flow as described in the [Central Governance User Guide](https://docs.axway.com/bundle/CentralGovernance_113_UsersGuide_allOS_en_HTML5/page/Content/AxwayStartPage.htm).
1. In the SEND command include at a minimum:
    -   The name of the final receiver, for example TARGET_APPLICATION.
    -   The name of the flow, for example TEST_RELAY.
    -   The file to be transferred, for example `report`.

    ```
    cftutil send part=target_application, idf=test_relay, fname=report
    ```
1. Optionally you can configure target post-processing to automatically send a reply.

> **Note**
>
> Tip  
> You can use the script delivered with Transfer CFT in the runtime/exec directory/BIN_re.cmd as a basis for your ACK reply from the target application.

> **Note**
>
> Note: When viewing the final transfer in the CG View Cycle Graph, you see the number of Transfer CFTs involved minus 1.

<span id="Store2"></span>

Store and forward with standalone {{< TransferCFT/suitevariablesTransferCFTName  >}}
-----------------------------------------------------------------------------------------

The following sections describe various store and forward options when using standalone {{< TransferCFT/suitevariablesTransferCFTName  >}}s.

<span id="Intentional_Store_and_Forward"></span>

### Intentional store and forward

The following descriptions correspond with the
parameter setting example in the
figure below.

****Configure the sender****

Configure the following for the initial sender (Site A):

- Define the final receiver CFTPART with both the OMINTIME and OMAXTIME parameters equal to zero.
- Define the first relay (Site B).
- In the final receiver partner definition, make a reference to the first relay with IPART=&lt;relay&gt; .

****Configure the relay****

Configure the following for the store and forward (Site B):

- Define both the initiator and the receiver CFTPART partner definitions.
- Set COMMUT=YES (default).

****Configure the receiver****

Configure the following for the final receiver (Site C):

- Define both the initiator and the relay CFTPART partner definitions.

> **Note**
>
> Note: To enable acknowledgments from the receiver C to the sender A, in the receiver (C) configuration you must set the sender (A) CFTPART with OMINTIME and OMAXTIME parameters equal to zero, and define the relay (B) as the IPART.

> **Note**
>
> Note: If the initial sender A is not
> required to establish any direct physical connection with the final receiver
> C, then the command CFTTCP ID=ID_C has no impact. Similarly, there is no need to set CFTTCP ID=ID_A
> for the final receiver C.

********Intentional store and forward********

![](/Images/TransferCFT/Intentional_store_and_forward.gif)

<span id="Intentional_VAN_store_and_forward"></span>

### Intentional VAN store and forward

Use the same conditions as indicated in the [Intentional STORE and FORWARD](#Intentional_Store_and_Forward)
to establish the routing. For the store and forward site to be in VAN mode, you must additionally complete the store
and forward parameters as follows: `CFTPART ID=ID_A,COMMUT=SERVER,...`

The following descriptions correspond with the
parameter setting example in the
figure below.

****Configure the sender****

Configure the following for the initial sender (Site A):

- Define the final receiver CFTPART with OMINTIME and OMAXTIME parameters equal to zero.
- Define the first relay.
- Make a reference to the first relay with IPART=&lt;relay&gt; in the final receiver partner definition.

****Configure the relay****

Configure the following for the store and forward (Site B):

- Define both the initiator and the receiver CFTPART partner definitions.
- Set COMMUT=SERVER.
- Define a procedure to execute and reference in the CFTPARM (in this example).
    -   In the store and forward example below, the procedure identified by `myproc `
        includes the following command on completion of processing: `CFTUTIL SEND PART= &RPART, SPART= &SPART, FNAME= &FNAME, IDF=   &IDF`
    -   When the symbolic variables are replaced: `CFTUTIL SEND PART=ID_C,SPART=ID_A,FNAME=frecv, IDF=test`

****Configure the receiver****

Configure the following for the final receiver (Site C):

- Define both the initiator and the relay CFTPART partner definitions.

> **Note**
>
> Note: In the example, the receiver (C) configuration defines the sender (Site A) CFTPART with OMINTIME and OMAXTIME parameters equal to zero, and defines the relay (Site B) as the IPART, which enables acknowledgments from the receiver C to the sender A.

********Intentional VAN store and forward********

![](/Images/TransferCFT/Intentional_VAN_store_and_forward.gif)

<span id="Forced_Store_and_Forward"></span>

### Forced store and forward

This feature, available only in the PeSIT protocol, is
used on an intermediate site to force the store and forward mode to another
site without knowing the final partner.

The following descriptions correspond with the
parameter setting example in the
figure below.

****Configure the sender****

- Define the first relay (Intermediate Site 1).

****Configure the first relay****

Configure the following for the store and forward Intermediate Site 1:

- Define the sender and the second relay (Site 2) = 2 CFTPARTs.
- Set COMMUT=PART.

<!-- -->

- In the CFTPART for the sender site, make a reference to the second relay using the IPART=&lt;Site 2&gt;.

****Configure the second relay****

Configure the following for the store and forward Intermediate Site 2:

- Define both the first relay (Intermediate Site 1) and the receiver partner definitions (CFTPARTs).
- Set COMMUT=PART.
- In the CFTPART for the first relay (Intermediate Site 1), make a reference to the final site.

****Configure the final receiver****

- Define the CFTPART partner definition for the second relay (Intermediate Site 2).

> **Note**
>
> Note: To enable acknowledgments from the receiver to the sender, add the IPART for each CFTPART definition in each relay. For example:

- For the Intermediate Site 2 in the CFTPART ID=IDGWAY, set the IPART=IDDEP1, which refers to the Intermediate Site 1.
- For the Intermediate Site 1 in the CFTPART ID=IDDEP, set the IPART=IDNAT1, which refers to the sender site.

****Store and forward SEND command****

In the SEND command you must specify the final network partner (the NSPART of the final partner) as well as the ID of the first intermediate partner.

```
CFTUTIL SEND PART=NFINAL, IPART=IDNAT, ...
```

********Forced store and forward********

![](/Images/TransferCFT/Forced_Store_and_forward.gif)

> **Note**
>
> Note: The REPLY command may be sent when the end of transfer procedure is
> executed (EXECRF). The acknowledgement message is then transferred in the same way, via
> all the intermediate sites until it reaches the initial site (IPART =
> …).
