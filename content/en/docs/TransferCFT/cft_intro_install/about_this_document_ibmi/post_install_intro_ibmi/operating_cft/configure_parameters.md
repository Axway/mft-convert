---
title: "Configuring Transfer CFT parameters"
linkTitle: "Configuring Transfer CFT parameters"
weight: 310
--- From the Main Menu, enter the command **`cft`** and press ****Enter**** to open the Manager Menu.

1. Select option 1 Customization and press ENTER.
1. In the Customization screen, select option 1 CFT parameters and press ENTER.
1. You can select option **2 Interpret the selected member**.

<span id="Interpreting a parameter source"></span>

## Interpreting a parameter source

The parameter file that you selected in the previous step, option 1 Editing the parameter source member, is interpreted by CFTUTIL. The options are confirmed before being interpreted. To interpret or update a parameter source, select option 2 Interpret selected member in the Customization screen. The following screen is displayed.

The parameter and partner files can be created, re- created, or updated prior to interpretation. If this is the first time that you are interpreting a parameter source, you must first create it.

- Select 1= Create for each time you interpret a new parameter source.

<!- - - - >

- Select 2 = Update to change existing command parameters. You can modify many of the Transfer CFT parameters while CFT is running, but some configuration command changes cannot be applied dynamically. For more information, refer to the Transfer CFT online documentation.

If you select 1 Configuration interpretation, Transfer CFT displays following messages.

View messages

```
CFTU20I CFT OS/400
CFTU20I Version 3.2.1 2015/03/15
CFTU20I (C) Copyright AXWAY 1989- 2015
CFTU20I ====> Starting Session on 02/04/2013 Time is 09:28:19
CFTU20I
CFTU00I CFTFILE _ Correct (type=param,mode=create,fname=CFTPROD/PARM1) CFTU20I Number of Command(s) 1
CFTU20I Number of error(s) 0
CFTU20I Ending Session on 02/04/2013 Time is 09:28:20
CFTU20I Session active for 0:00:01
Press ENTER to end terminal session.
CFTU20I
CFTU20I CFT OS/400
CFTU20I Version 3.0.1 2013/03/15
CFTU20I (C) Copyright AXWAY 1989- 2012
CFTU20I ====> Starting Session on 02/04/2013 Time is 09:29:20
CFTU20I
CFTU00I CFTFILE _ Correct (type=part,mode=create,fname=CFTPROD/PART1 )
CFTU20I Number of Command(s) 1
CFTU20I Number of error(s) 0
CFTU20I Ending Session on 02/04/2015 Time is 09:29:22
CFTU20I Session active for 0:00:02
Press ENTER to end terminal session.
CFTU20I
CFTU20I CFT OS/400
CFTU20I Version 3.2.1 2015/03/15
CFTU20I (C) Copyright AXWAY 1989- 2015
CFTU20I ====> Starting Session on 02/04/2013 Time is 09:29:24
CFTU20I Parameters file :+CFTPARM
CFTU20I
CFTU00I CFTPARM _ Correct (MODE=REPLACE,ID=IDPARM0,CAT=IDCAT0,COM=IDCOM0,LOG
=
CFTU00I IDLOG0,NET=TCP0,PROT=PESITANY,MAXTASK=4,TRANTASK=8
CFTU00I ,MAXTRANS=32,BUFSIZE=32000,KEY='£CFTPROD/KEY',DEF
CFTU00I AULT=IDFDEFT,PART=CFT400,WAITRESP=900,EXECSF='\*LIB
CFTU00I L/CFTSRC(B_EXECSF)',EXECRF='\*LIBL/CFTSRC(B_EXECRF)
CFTU00I ',PARTFNAM='+CFTPART')
CFTU00I CFTCAT _ Correct (MODE=REPLACE,ID=IDCAT0,FNAME='+CFTCAT',SHARE=NO,UP
CFTU00I DAT=0,WSCAN=1,ST=2,RT=2,SX=1,RX=1)
CFTU00I CFTCOM _ Correct (MODE=REPLACE,ID=IDCOM0,TYPE=FILE,NAME='+CFTCOM',WS
CFTU00I CAN=10)
CFTU00I CFTLOG _ Correct (MODE=REPLACE,ID=IDLOG0,FNAME='CFTPROD/LOG1',AFNAM
CFTU00I E='CFTPROD/ALOG1',MAXREC=15000,SWITCH=1300,EXEC=
'
CFTU00I \*LIBL/CFTSRC(B_EXECLOG)',OPERMSG=255,NOTIFY='
CFTU00I ')
CFTU00I CFTNET _ Correct (MODE=REPLACE,ID=TCP0,MAXCNX=32,CALL=INOUT,TYPE=TCP
CFTU00I ,HOST=INADDR_ANY)
CFTU00I CFTPROT _ Correct (MODE=REPLACE,ID=PESITANY,NET=TCP0,TYPE=PESIT,PROF=
CFTU00I ANY,CONCAT=YES,SEGMENT=YES,MULTART=YES,SCOMP=0,RCO
CFTU00I MP=0,SCHKW=2,RCHKW=2,SPACING=512,RPACING=512,SAP=6
CFTU00I 5535,RRUSIZE=32000,SRUSIZE=32000,RTO=250,DISCTC=40
CFTU00I 0,DISCTD=200,DISCTR=10,DISCTS=400)
CFTU00I CFTPART _ Correct (MODE=REPLACE,ID=BOUCLE,NSPART=LOOP,NRPART=LOOP,PRO
CFTU00I T=PESITANY,SAP=65535,SYST='OS400')
CFTU00I CFTTCP _ Correct (MODE=REPLACE,ID=BOUCLE,CNXOUT=16,CNXIN=16,CNXINOUT
CFTU00I =16,HOST=127.0.0.1)
CFTU00I CFTPART _ Correct (MODE=REPLACE,ID=CIBLE,NSPART=AS400,NRPART=CIBLE,PR
CFTU00I OT=PESITANY,SAP=65531,SYST='OS400')
CFTU00I CFTTCP _ Correct (MODE=REPLACE,ID=CIBLE,CNXOUT=16,CNXIN=16,CNXINOUT=
CFTU00I 16,HOST=CIBLE)
CFTU00I CFTSEND _ Correct (MODE=REPLACE,ID=TSTIMPL,IMPL=YES,FNAME='QGPL/QAUOO
CFTU00I PT',EXEC='\*LIBL/CFTSRC(B_EXECSIMP)',FCODE=EBCDIC)
CFTU00I CFTSEND _ Correct (MODE=REPLACE,ID=TSTNCOMP,IMPL=NO,FCODE=EBCDIC,NCOM
CFTU00I P=0)
CFTU00I CFTSEND _ Correct (MODE=REPLACE,ID=IDFDEFT,IMPL=NO,FCODE=EBCDIC)
CFTU00I CFTRECV _ Correct (MODE=REPLACE,ID=SRC400,FTYPE='S',FDISP=BOTH,FACTIO
CFTU00I N=ERASE,FCODE=EBCDIC,FRECFM='F',FNAME='CFTPROD/UT
CFTU00I IN(R_?PARM)')
CFTU00I CFTRECV _ Correct (MODE=REPLACE,ID=SRCFILE,FDISP=BOTH,FACTION=ERASE,F
CFTU00I CODE=EBCDIC,FRECFM='F',FNAME='CFTPROD/UTIN(R_?PAR
CFTU00I M)')
CFTU00I CFTRECV _ Correct (MODE=REPLACE,ID=COMMUT,FDISP=BOTH,FACTION=DELETE,F
CFTU00I CODE=BINARY,FRECFM='V',FSPACE=65535,FNAME='CFTPROD
CFTU00I P/R_?IDTU')
CFTU00I CFTRECV _ Correct (MODE=REPLACE,ID=IDFDEFT,FDISP=BOTH,FACTION=DELETE
,
CFTU00I FCODE=EBCDIC,FRECFM='F',FSPACE=65535,FNAME='CFTPROD
CFTU00I /R_?IDTU')
CFTU00I CFTRECV _ Correct (MODE=REPLACE,ID=SAVEFILE,FDISP=BOTH,FACTION=DELETE
CFTU00I ,FCODE=BINARY,FTYPE='Z',FNAME='CFTPROD/R_?IDTU')
CFTU00I RETURN _ Correct (CODE=0)
CFTU20I Number of Command(s) 18
CFTU20I Number of error(s) 0
CFTU20I Ending Session on 02/04/2013 Time is 09:29:25
CFTU20I Session active for 0:00:01
```

An interpretation is considered to be valid when all messages are displayed as Correct.

If an interpretation error is detected, modify the invalid parameter or parameter, and select **2 Repeat the interpretation**. When you select 2 Repeat the interpretation, only messages concerning configuration commands are displayed.
