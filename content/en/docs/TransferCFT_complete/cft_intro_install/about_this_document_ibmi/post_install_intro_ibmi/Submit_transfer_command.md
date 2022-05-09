{
    "title": "Viewing transfer messages",
    "linkTitle": "Viewing transfer messages",
    "weight": "230"
}After depositing a transfer command, such as the loop-back test, the following messages are displayed.

Example
-------

`SEND PART=LOOP,IDF=TEST,FNAME=CFTPROD/TEST`

```
From . . . : CFT 04/02/15 17:12:45
CFTR12I SEND PART=LOOP IDF=TEST Treated for USER CFT
From . . . : CFT 04/02/15 17:12:45
+
From . . . : CFT 04/02/15 17:12:45
CFTS20I Communication file row number deleted: 00000030
From . . . : CFT 04/02/15 17:12:45
+
From . . . : CFT 04/02/15 17:12:46
CFTW09I PART=LOOP IDF=TEST IDT=D0217124 CFTSEND IDFDEFT NIDF=TEST
From . . . : CFT 04/02/15 17:12:46
+
From . . . : CFT 04/02/15 17:12:46
CFTT13I PART=LOOP IDF=TEST IDT=D0217124 _ Session parameters
From . . . : CFT 04/02/15 17:12:46
+ PROT=PESITANY SAP=65535 HOST=127.0.0.1
From . . . : CFT 04/02/15 17:12:46
CFTI34I PID=9 CFTTFIL Task started successfully
From . . . : CFT 04/02/15 17:12:47
CFTT53I PART=LOOP IDF=TEST IDT=D0217124 Requester file selected
From . . . : CFT 04/02/15 17:12:47
CFTT55I PART=LOOP IDF=TEST IDT=D0217124 Requester file opened
From . . . : CFT 04/02/15 17:12:47
CFTH56I PART=LOOP IDS=00003 PESIT Server session opened
pi7=02:00512
From . . . : CFT 04/02/15 17:12:47
CFTH56I PART=LOOP IDS=00002 PESIT Requester session opened
pi7=02:00512
From . . . : CFT 04/02/15 17:12:47
CFTW09I PART=LOOP IDF=TEST IDT=D0217124 CFTRECV IDFDEFT NIDF=TEST
From . . . : CFT 04/02/15 17:12:47
+
From . . . : CFT 04/02/15 17:12:47
CFTT53I PART=LOOP IDF=TEST IDT=D0217124 Server file created
From . . . : CFT 04/02/15 17:12:47
CFTT55I PART=LOOP IDF=TEST IDT=D0217124 Server file opened
From . . . : CFT 04/02/15 17:12:47
CFTT57I PART=LOOP IDF=TEST IDT=D0217124 Server transfer started
From . . . : CFT 04/02/15 17:12:47
CFTT57I PART=LOOP IDF=TEST IDT=D0217124 Requester transfer started
From . . . : CFT 04/02/15 17:12:47
CFTT58I PART=LOOP IDF=TEST IDT=D0217124 Server transfer ended
From . . . : CFT 04/02/15 17:12:47
CFTT58I PART=LOOP IDF=TEST IDT=D0217124 Requester transfer ended
From . . . : CFT 04/02/15 17:12:47
CFTT56I PART=LOOP IDF=TEST IDT=D0217124 Server file closed
From . . . : CFT 04/02/15 17:12:47
CFTH58I PART=LOOP IDS=00003 IDF=TEST NIDT=9317124 transfer
deselected
From . . . : CFT 04/02/15 17:12:47
+ T=400
From . . . : CFT 04/02/15 17:12:47
CFTT54I PART=LOOP IDF=TEST IDT=D0217124 Server file deselected
From . . . : CFT 04/02/15 17:12:47
CFTT88I+IDT=D0217124 WORKINGDIR= FNAME=CFTPROD/R_A000000L NBC=160
From . . . : CFT 04/02/15 17:12:47
CFTH58I PART=LOOP IDS=00002 IDF=TEST NIDT=9317124 transfer
deselected
From . . . : CFT 04/02/15 17:12:47
+ T=200
From . . . : CFT 04/02/15 17:12:47
CFTT56I PART=LOOP IDF=TEST IDT=D0217124 Requester file closed
From . . . : CFT 04/02/15 17:12:47
CFTT54I PART=LOOP IDF=TEST IDT=D0217124 Requester file deselected
From . . . : CFT 04/02/15 17:12:48
CFTT88I+IDT=D0217124 WORKINGDIR= FNAME=CFTPROD/TEST NBC=160
From . . . : CFT 04/02/15 17:12:48
CFTS03I PART=LOOP IDF=TEST IDT=D0217124 _ \*LIBL/CFTSRC(B_EXECRF)
From . . . : CFT 04/02/15 17:12:48
+ executed
From . . . : CFT 04/02/15 17:12:50
CFTS03I PART=LOOP IDF=TEST IDT=D0217124 _ \*LIBL/CFTSRC(B_EXECSF)
From . . . : CFT 04/02/15 17:12:50
+ executed
From . . . : CFT 04/02/15 17:12:50
CFTR17I END PART=LOOP IDF=\* IDTU=A000000L In progress for USER
From . . . : CFT 04/02/15 17:12:50
+ CFT
From . . . : CFT 04/02/15 17:12:50
CFTR12I END PART=LOOP IDF=\* IDTU=A000000L Treated for USER
CFT
From . . . : CFT 04/02/15 17:12:50
+
From . . . : CFT 04/02/15 17:12:50
CFTS20I Communication file row number deleted: 00000031
From . . . : CFT 04/02/15 17:12:50
+
From . . . : CFT 04/02/15 17:12:50
CFTR12I SEND PART=LOOP IDM=REP Treated for USER CFT
From . . . : CFT 04/02/15 17:12:50
From . . . : CFT 04/02/15 17:12:50
CFTS20I Communication file row number deleted: 00000032
From . . . : CFT 04/02/15 17:12:50
+
From . . . : CFT 04/02/15 17:12:50
CFTT13I PART=LOOP IDM=REP IDT=D0217125 _ Session parameters
From . . . : CFT 04/02/15 17:12:50
+ PROT=PESITANY SAP=65535 HOST=127.0.0.1
From . . . : CFT 04/02/15 17:12:50
CFTT59I PART=LOOP IDM=TEST IDT=D0217124 Server reply
transfered
From . . . : CFT 04/02/15 17:12:50
CFTH60I PART=LOOP IDS=00002 IDM=TEST NIDT=9317124 reply
transfered
From . . . : CFT 04/02/15 17:12:50
CFTT59I PART=LOOP IDM=REP IDT=D0217125 Requester reply
transfered
From . . . : CFT 04/02/15 17:12:50
CFTH62I+ REF=9317124.LOOP.LOOP.0.TEST..
From . . . : CFT 04/02/15 17:12:51
CFTR17I END PART=LOOP IDF=\* IDTU=A000000K In progress for USER
From . . . : CFT 04/02/15 17:12:51
+ CFT
From . . . : CFT 04/02/15 17:12:51
CFTR12I END PART=LOOP IDF=\* IDTU=A000000K Treated for USER
CFT
From . . . : CFT 04/02/15 17:12:51
+
From . . . : CFT 04/02/15 17:12:51
CFTS20I Communication file row number deleted: 00000033
From . . . : CFT 04/02/15 17:12:51
```
