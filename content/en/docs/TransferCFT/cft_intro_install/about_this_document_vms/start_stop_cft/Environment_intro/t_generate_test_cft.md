{
    "title": "Generating a test Transfer CFT",
    "linkTitle": "Generate a test Transfer CFT",
    "weight": "230"
}Create a test Transfer CFT
--------------------------

To generate a test Transfer CFT, enter the command: `$cftinit cft_scen:cft-tcp.conf`

The following messages are displayed:

```
CFTU20I
CFTU20I CFT/VMS
CFTU20I Version 3.0.1 09/09/2012
CFTU20I (C) Copyright AXWAY 1989-2012
CFTU20I ====> Starting Session on 09/09/2012 Time is 13:58:32
CFTU20I
CFTU00I CFTFILE _ Correct (TYPE=PARAM,FNAME=CFTPARM)
CFTU00I CFTFILE _ Correct (TYPE=PART,FNAME=CFTPART)
CFTU00I CFTFILE _ Correct (TYPE=CAT,FNAME=CFTCATA,RECNB=600)
CFTU00I CFTFILE _ Correct (TYPE=COM,FNAME=CFTCOM,RECNB=50)
CFTU00I CFTFILE _ Correct (TYPE=LOG,FNAME=CFTLOG)
CFTU00I CFTFILE _ Correct (TYPE=LOG,FNAME=CFTLOGA)
CFTU00I CFTFILE _ Correct (TYPE=ACCNT,FNAME=CFTACCNT)
CFTU00I CFTFILE _ Correct (TYPE=ACCNT,FNAME=CFTACCNTA)
CFTU00I RETURN _ Correct (CODE=0)
CFTU20I Number of Command(s) 8
CFTU20I Number of error(s) 0
CFTU20I Ending Session on 23/10/2006 Time is 16:43:47
CFTU20I Session active for 0:00:10
CFTU20I CFT/VMS
CFTU20I Version 3.0.1 09/09/2012
CFTU20I (C) Copyright AXWAY 1989-2012
CFTU20I ====> Starting Session on 09/09/2012 Time is 13:59:32
CFTU20I Parameters file :CFTPARM
CFTU20I Partners file :CFTPART
CFTU20I Catalog file :CFTCATA
CFTU20I
CFTU00I CFTPARM _ Correct (id=IDPARM0,default=IDFDEF,KEY=@CFT_DAT:CFTKEY.PARM)
CFTU00I CFTCOM _ Correct (id=com0,type=file,name=CFTCOM,wscan=1,mode=REPLACE)
CFTU00I CFTCOM _ Correct (id=coms,type=tcpip,protocol=xhttp,host=127.0.0.1,p)
CFTU00I CFTCAT _ Correct (id=cat0,fname=CFTCATA,wscan=1,updat=1,mode=REPLACE)
CFTU00I CFTLOG _ Correct (ID=LOG0,AFNAME=CFTLOGA,EXEC=CFT_PROC:VIDLOG.PRO,FN)
CFTU00I CFTACCNT _ Correct (ID=ACCNT0,TYPE=FILE,AFNAME=CFTACCNTA,EXEC=CFT_PROC)
CFTU00I CFTNET _ Correct (id=NET0,maxcnx=128,type=TCP,host=INADDR_ANY,call=I)
CFTU00I CFTPROT _ Correct (id=PESITANY,net=NET0,type=PESIT,prof=ANY,sap=17610)
CFTU00I CFTPROT _ Correct (id=PESITSSL,net=NET0,type=PESIT,prof=ANY,sap=17620)
CFTU00I CFTSSL _ Correct (id=SSLPESIT,direct=SERVER,rootcid=(AXWMFTCA),userc)
CFTU00I CFTSEND _ Correct (id=IDFDEF,fcode=BINARY,fname=CFT_SEND:TEST.SND,mod)
CFTU00I CFTRECV _ Correct (id=IDFDEF,fcode=BINARY,fname=CFT_RECV:&IDF_&IDT.RE)
CFTU00I CFTSEND _ Correct (id=BIN,fcode=BINARY,fname=CFT_SEND:TEST.SND,mode=R)
CFTU00I CFTRECV _ Correct (id=BIN,fcode=BINARY,faction=delete,fname=CFT_RECV:)
CFTU00I CFTSEND _ Correct
(id=TXT,fcode=ASCII,fname=CFT_SEND:TEST.SND,mode=RE)
CFTU00I CFTRECV _ Correct (id=TXT,fcode=ASCII,faction=delete,fname=CFT_RECV:&)
CFTU00I CFTSEND _ Correct (id=FIC1,exit=ID_EXIT,fname=CFT_SEND:TEST.SND,mode=)
CFTU00I CFTEXIT _ Correct (id=ID_EXIT,language=C,prog='CFTEXITF',reserv=100,f)
CFTU00I CFTSEND _ Correct (id=ID_EXITL,exit=CFTEXLST,impl=yes,faction=delete,)
CFTU00I CFTEXIT _ Correct (id=CFTEXLST,language=C,prog='CFTEXL',reserv=8192,f)
CFTU00I CFTAPPL _ Correct (id=IDFDEF,direct=both,userid=&userid)v
CFTU00I RETURN _ Correct (CODE=0)
CFTU20I Number of Command(s) 21
CFTU20I Number of error(s) 0
CFTU20I Ending Session on 09/09/2012 Time is 13:59:32
CFTU20I Session active for 0:00:00
```

Create sample partners
----------------------

To create sample partners, enter the command:` $ cftinit cft_scen:cft-tcp-part.conf`

The following procedure creates the sample partners:

- loop transfer: partners Paris - New York
- loop transfer using SSL: loopssl

```
CFTU20I
CFTU20I CFT/VMS
CFTU20I Version 3.0.1 09/09/2012
CFTU20I (C) Copyright AXWAY 1989-2012
CFTU20I ====> Starting Session on 09/09/2012 Time is 13:57:39
CFTU20I Parameters file :CFTPARM
CFTU20I Partners file :CFTPART
CFTU20I Catalog file :CFTCATA
CFTU20I
CFTU00I CFTPART _ Correct (id=NEWYORK,prot=PeSITANY,sap=17610,nspart=PARIS,nr)
CFTU00I CFTTCP _ Correct (id=NEWYORK,host=127.0.0.1,cnxin=8,cnxout=8,cnxinou)
CFTU00I CFTPART _ Correct (id=PARIS,prot=PeSITANY,sap=17610,nspart=NEWYORK,nr)
CFTU00I CFTTCP _ Correct (id=PARIS,host=127.0.0.1,cnxin=8,cnxout=8,cnxinout=)
CFTU00I CFTPART _ Correct (id=LOOP,prot=PeSITANY,sap=17610,nspart=LOOP,nrpart)
CFTU00I CFTTCP _ Correct (id=LOOP,host=127.0.0.1,cnxout=8,cnxin=8,cnxinout=8)
CFTU00I CFTPART _ Correct (id=LOOPSSL0,prot=PESITSSL,sap=17620,nspart=LOOPSSL)
CFTU00I CFTTCP _ Correct (id=LOOPSSL0,host=127.0.0.1,cnxout=8,cnxin=8,cnxino)
CFTU00I CFTSSL _ Correct (id=SSL_LOOP0,direct=CLIENT,rootcid=(AXWMFTCA),pass)
CFTU00I RETURN _ Correct (CODE=0)
CFTU20I Number of Command(s) 9
CFTU20I Number of error(s) 0
CFTU20I Ending Session on 09/09/2012 Time is 13:57:39
CFTU20I Session active for 0:00:00
```

The Transfer CFT environment is now built and ready for use.

About submitting commands
-------------------------

To begin submitting commands to Transfer CFT you can use:

- CFTUTIL
- Copilot
- API programming interfaces

For more information, see *[communications]()*.
