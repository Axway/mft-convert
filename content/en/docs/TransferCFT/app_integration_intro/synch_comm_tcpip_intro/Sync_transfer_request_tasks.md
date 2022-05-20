---
title: "Using WSTATES, WPHASES, and WPHASESTEPS in the SEND/RECV command"
linkTitle: "Using WSTATES, WPHASES, and WPHASESTEPS"
weight: 280
---This topic presents synchronous transfer request examples using the WSTATES or WPHASES/WPHASESTEPS parameters with the SEND/RECV command.

## Using WPHASES and WPHASESTEPS

The WPHASES and WPHASESTEPS parameters allow for more precision and possibilities than using WSTATES alone.

****Example of a transfer request where the wphases is used in conjunction with wphasesteps****

```
config type=com,mediacom=tcpip,fname=xhttp://localhost:1765
 
send part=loop,idf=t2,wphases=X,wphasesteps=X,wtimeout=40
CFTU00I SEND _ Correct (SWAITCAT_OK: reached phase X, phasestep X, state X)
CFTU00I SEND _ Correct (part=loop,idf=t2,wphases=X,wphasesteps=X,wtimeout=40)
```

****Example of a simple transfer request using only wphases****

```
config type=com,mediacom=tcpip,fname=xhttp://localhost:1765
 
send part=paris,idf=t2,wphases=Z,wtimeout=40,ackstate=require
CFTU00I SEND _ Correct (SWAITCAT_OK: reached phase Z, phasestep H, state Z)
CFTU00I SEND _ Correct (part=paris,idf=t2,wphases=Z,wtimeout=40,ackstate=require)
```

## Using WSTATES and WTIMEOUT

* Example 1: Transfer request using WSTATES
* Example 2: Use a SEND with WSTATES and WTIMEOUT

**Example 1: Transfer request using WSTATES**

Trigger a command pending that a transfer has reached a certain defined state. See also [WSTATES](). This task resembles an FTP transfer.

```
config type=com,mediacom=tcpip,fname=xhttp://localhost:1765
 
send part=paris,idf=bin,wstates=tx
 
PRINT MSG='transfer idtu=%_CAT_IDTU% completed with state=%_CAT_STATE% and diagi=%_CAT_DIAGI%'
 
Output
CONFIG _ Correct (type=com,mediacom=tcpip,fname=xhttp://localhost:1765)
CFTU00I SEND _ Correct (SWAITCA,IDTU=A000008B)
CFTU00I SEND _ Correct (part=paris,idf=bin,wstates=tx)
CFTU00I PRINT _ Correct (MSG='')transfer idtu=A000008B completed with state=X and diagi=0
```

**Example 2: Use a SEND with WSTATES and WTIMEOUT**

In this example, the transfer fails because it did not complete prior to the timeout, which leads to an error code of 81.

```
config type=com,mediacom=tcpip,fname=xhttp://localhost:1765
 
send part=paris,idf=bin,wstates=tx,wtimeout=2,fname=pub/BIG
 
PRINT MSG='ERROR = %_CMDRET%'
 
Output
CFTU20I Communication TCPIP:xhttp://localhost:1765
CFTU00I CONFIG _ Correct (type=com,mediacom=tcpip,fname=xhttp://localhost:1765)
 
send part=paris,idf=bin,wstates=tx,wtimeout=2,fname=pub/BIG
CFTU25E SEND _ Error (TIMEOUT reached,(IDTU=A0000095,IDT=B1817282))
CFTU00I SEND _ Failed (part=paris,idf=bin,wstates=tx,wtimeout=2,fname=pub/ BIG)
 
PRINT MSG='ERROR = %_CMDRET%'
ERROR = 81
CFTU00I PRINT _ Correct (MSG='ERROR = 81')
```

**Example 3: Serialization using WSTATES**

In this example the transfers are executed sequentially depending on the defined WSTATES.

```
config type=com,mediacom=tcpip,fname=xhttp://localhost:1765
 
send part=paris,idf=bin,ida=1,wstates=tx
char name=idt1,init=%_CAT_IDT%
send part=paris,idf=bin,ida=2,wstates=tx
char name=idt2,init=%_CAT_IDT%
send part=paris,idf=bin,ida=3,wstates=tx
char name=idt3,init=%_CAT_IDT%
send part=paris,idf=bin,ida=4,wstates=tx
char name=idt4,init=%_CAT_IDT%
PRINT MSG='transfers: idt1=%idt1% - idt2=%idt2% - idt3=%idt3% - idt4=%idt4%'
 
Output
CFTU00I SEND _ Correct (SWAITCA,IDTU=A000008F)
CFTU00I SEND _ Correct (part=paris,idf=bin,ida=1,wstates=tx)
CFTU00I CHAR _ Correct (name=IDT1,init=B1817075)
CFTU00I SEND _ Correct (SWAITCA,IDTU=A000008H)
CFTU00I SEND _ Correct (part=paris,idf=bin,ida=2,wstates=tx)
CFTU00I CHAR _ Correct (name=IDT2,init=B1817080)
CFTU00I SEND _ Correct (SWAITCA,IDTU=A000008J)
CFTU00I SEND _ Correct (part=paris,idf=bin,ida=3,wstates=tx)
CFTU00I CHAR _ Correct (name=IDT3,init=B1817081)
CFTU00I SEND _ Correct (SWAITCA,IDTU=A000008L)
CFTU00I SEND _ Correct (part=paris,idf=bin,ida=4,wstates=tx)
CFTU00I CHAR _ Correct (name=IDT4,init=B1817082)
 
transfers: idt1=B1817075 - idt2=B1817080 - idt3=B1817081 - idt4=B1817082
CFTU00I PRINT _ Correct (MSG='transfers: idt1=B1817075 - idt2=B1817080 - id
CFTU00I t3=B1817081 - idt4=B1817082')
 
Log
00 16/02/18 17:07:27 CFTT57I Requester transfer started <IDTU=A000008F PART=paris IDF=BIN IDT=B181
7075 >...
00 16/02/18 17:07:27 CFTT58I Requester transfer ended <IDTU=A000008F PART=paris IDF=BIN IDT=B181
7075>.....
00 16/02/18 17:07:27 CFTT57I Requester transfer started <IDTU=A000008H PART=paris IDF=BIN IDT=B181
7080 >...
00 16/02/18 17:07:27 CFTT58I Requester transfer ended <IDTU=A000008H PART=paris IDF=BIN IDT=B181
7080>....
00 16/02/18 17:07:27 CFTT57I Requester transfer started <IDTU=A000008J PART=paris IDF=BIN IDT=B181
7081 >...
00 16/02/18 17:07:27 CFTT58I Requester transfer ended <IDTU=A000008J PART=paris IDF=BIN IDT=B181
7081>....
00 16/02/18 17:07:27 CFTT57I Requester transfer started <IDTU=A000008L PART=paris IDF=BIN IDT=B181
7082 >...
00 16/02/18 17:07:27 CFTT58I Requester transfer ended <IDTU=A000008L PART=paris IDF=BIN IDT=B181
7082>...
```
