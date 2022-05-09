{
    "title": "Connection and maximum transfer troubleshooting",
    "linkTitle": "Connection and maximum transfer troubleshooting",
    "weight": "240"
}This section provides transfer examples that demonstrate parameter dependencies and typical log outputs, including errors, which you may encounter when using similar values. The goal is to understand the effect of parameter combinations, and be able to adapt settings to your particular needs.

The following examples show the output from both the server and requester sides.

- [Example: the number of transfers exceeds the CNXINOUT](#Example:4)
- [Example: MAXCNX is greater than MAXTRANS](#Example:3)
- [Example: the number of transfers exceeds the CNXINOUT](#Example:4)
- [Example: MAXCNX greater than MAXTRANS with session limit](#Example:5)

See [Session related troubleshooting](../session_troubleshooting) for session related output.

See the [Frequently asked questions](../faq) for common questions and answers on parameter usage, license keys, and so on.

Remote (partner) diagnostic codes
---------------------------------

When troubleshooting, remember that if the [DIAG](../../../troubleshoot_intro/about_error_codes/about_diagnostic_codes/diagi_diagnostic_codes) is greater than 500, it refers to a remote issue. To find the actual DIAG, subtract 500 from the displayed code. If the DIAG is 962, for example, the issue is a remote problem corresponding to DIAG 462 (no data sent on network).

<span id="Example:"></span>

Examples
--------

<span id="Example:4"></span>

### Example: the number of transfers exceeds the CNXINOUT

##### **Scenario 1 - Requester configuration**

In this example, the number of transfers exceeds the CNXINOUT value defined on the requester side.

**Requester output**

```
CFTT09E _ Maximum cv Affected <IDTU=A00000FL PART=SUN35 IDF=T2 IDT=D2918112 PROT=PESIT>
```

**Server output**

No message on server side.

##### **Scenario 2**

Here the number of transfers exceeds the CNXINOUT value defined on the server side.

**Server output**

```
CFTT94I <IDTU=A00000FM PART=SUN35 IDF=T1 IDT=D2918192 FCHARSET= NCHARSET=>
CFTH13E FPDU Remote reject <PART=SUN35 DIAGI=916 DIAGP=RCO 309>
CFTT75E connect reject <IDTU=A00000FN PART=SUN35 IDF=T2
IDT=D2918193 916 RCO 309>
```

**Requester output**

```
CFTT09E _ Maximum cv Affected <IDTU= PART=WINZZ PROT=PESITANY>
CFTH22E NPART=WINZZ rejected DIAGI=418
```
<span id="Example:3"></span>

### Example: MAXCNX is greater than MAXTRANS

MAXCNX is greater than MAXTRANS on the requester side, and there are more than MAXTRANS transfers to be processed.

**Incoming session**

For the server's incoming session the extra transfer(s) is rejected with an error.

**Server output**

```
CFTH22E NPART=NEWYORK rejected DIAGI=416
```

**Requester output**

```
CFTH13E FPDU Remote reject <PART=PARIS DIAGI=916 DIAGP=RCO 201>
```

**Outgoing session**

When the requester wants to perform more than MAXTRANS transfers:

```
The extra transfer(s) is set to an available state (D), but there is no error message in LOG.
```
<span id="Example:4"></span>

### Example: MAXCNX is less than MAXTRANS

MAXCNX is less than the MAXTRANS value on the requester side, and you want to perform more than MAXCNX transfers (for example MAXCNX = 2, MAXTRANS= 5).

**Scenario 1**

**Requester output**

```
CFTH09E Network connect request local error <PART=SUN35-5 NCR=416 NCS=MAXCNX NET=TCP>
CFTT75E connect reject <IDTU=A00000GE PART=SUN35-5 IDF=T1 IDT=D3011193 416 MAXCNX>
```

**Server output**

```
No message.
```

**Scenario 2**

MAXCNX is less than the MAXTRANS value on the server side, and you want to perform more than MAXCNX transfers on the requester side (for example MAXCNX = 2, MAXTRANS= 5).

**Requester output**

```
CFTH11E Error Opening session <PART=WINZZ-4 EV=VNRELI ST=CN0022>
CFTT75E connect reject <IDTU=A000007D PART=WINZZ-4 IDF=T2 IDT=D3011264 302 R 0 2f2>
```

**Server output**

```
No notification on server side.
```
<span id="Example:5"></span>

### Example: MAXCNX greater than MAXTRANS with session limit

In this example, we have a distribution list with 5 partners, which shows an example of when you may want to have a MAXCNX greater than the MAXTRANS value.

**Scenario 1**

The transfers are executed quickly, in rapid succession because {{< TransferCFT/axwayvariablesComponentLongName  >}} is not limited by the session. As soon as the first 3 transfers are completed, the 2 remaining are executed immediately.

> **Note**
>
> Note: Remember one session is kept for incoming connections, so in this case only two sessions were used.

`MAXTRANS=3, MAXCNX=6, DISCTD=120 (seconds session is still open)`

```
...
15/06/23 <span class="underline">17:40:46</span> CFTT53I Requester file selected <IDTU=A0000VKQ PART=SUN35-1 IDF=BIN IDT=F2402472>
15/06/23 17:40:46 CFTT55I Requester file opened <IDTU=A0000VKQ PART=SUN35-1 IDF=BIN IDT=F2402472>
...
15/06/23 17:40:47 CFTH56I PESIT Requester session opened <PART=SUN35-2 IDS=00004 pi7=03:10240 HOST=127.0.0.1>
15/06/23 17:40:47 CFTH56I PESIT Requester session opened <PART=SUN35-1 IDS=00003 pi7=03:10240 HOST=127.0.0.1>
15/06/23 17:40:47 CFTT57I Requester transfer started <IDTU=A0000VKR PART=SUN35-2 IDF=BIN IDT=F2402473 >
...
15/06/23 17:40:47 CFTT56I Requester file closed <IDTU=A0000VKQ PART=SUN35-1 IDF=BIN IDT=F2402472>
15/06/23 17:40:47 CFTT54I Requester file deselected <IDTU=A0000VKQ PART=SUN35-1 IDF=BIN IDT=F2402472>
...
 
15/06/23 <span class="underline">17:40:47</span> CFTH56I PESIT Requester session opened <PART=SUN35-3 IDS=00006 pi7=03:10240 HOST=127.0.0.1>
15/06/23 17:40:47 CFTH56I PESIT Requester session opened <PART=SUN35-4 IDS=00005 pi7=03:10240 HOST=127.0.0.1>
15/06/23 17:40:47 CFTT57I Requester transfer started <IDTU=A0000VKS PART=SUN35-3 IDF=BIN IDT=F2402474 >
...
15/06/23 17:40:47 CFTT56I Requester file closed <IDTU=A0000VKU PART=SUN35-5 IDF=BIN IDT=F2402480>
15/06/23 17:40:47 CFTT54I Requester file deselected <IDTU=A0000VKU PART=SUN35-5 IDF=BIN IDT=F2402480>
15/06/23 <span class="underline">17:40:47</span> CFTT88I+<IDTU=A0000VKU WORKINGDIR= FNAME=pub/FTEST NBC=7104>
```

**Scenario 2**

In Scenario 2 Transfer CFT is limited by the session, meaning that the same 5 partner transfers as in Scenario 1 now take over 120 seconds (the DISCTD time defined to keep the session open).

> **Note**
>
> Note: DISCTD has an effect on latency.

`MAXTRANS=6, MAXCNX=3, DISCTD=120 (seconds session is still open)`

```
...
15/06/23 <span class="underline">17:58:21</span> CFTI34I PID=10956 CFTTFIL Task started successfully
15/06/23 17:58:21 CFTT53I Requester file selected <IDTU=A0000VL2 PART=SUN35-1 IDF=T1 IDT=F2402492>
15/06/23 17:58:21 CFTT55I Requester file opened <IDTU=A0000VL2 PART=SUN35-1 IDF=T1 IDT=F2402492>
...
15/06/23 17:58:21 CFTT13I Session parameters <IDTU=A0000VL3 PART=SUN35-2 IDF=T1 IDT=F2402493 _ PROT=PESIT SAP=21761 HOST=sun35.lab1.lab.ptx.axway.int>
15/06/23 17:58:21 CFTI34I PID=6160 CFTTFIL Task started successfully
...
15/06/23 17:58:51 CFTH09E Network connect request local error <PART=SUN35-4 NCR=416 NCS=MAXCNX NET=TCP>
15/06/23 17:58:51 CFTT75E connect reject <IDTU=A0000VL5 PART=SUN35-4 IDF=T1 IDT=F2402495 416 MAXCNX>
...
15/06/23 18:00:43 CFTT56I Requester file closed <IDTU=A0000VL6 PART=SUN35-5 IDF=T1 IDT=F2402500>
15/06/23 18:00:43 CFTT54I Requester file deselected <IDTU=A0000VL6 PART=SUN35-5 IDF=T1 IDT=F2402500>
15/06/23 <span class="underline">18:00:48</span> CFTH61I PESIT Server session closed <PART=SUN35-5 IDS=00012>
```

Related topics

- [About parallel transfers](../)
- [FAQ and troubleshooting](../faq)
