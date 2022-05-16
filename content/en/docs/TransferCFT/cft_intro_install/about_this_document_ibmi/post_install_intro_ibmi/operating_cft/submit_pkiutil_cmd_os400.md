---

    title: About transport security and PKIUTIL commands
    linkTitle: Transport security and PKIUTIL commands
    weight: 260

---
This section describes SSL security parameters. For more information on transport security concepts, refer to the <span class="span_2" style="font-style: italic;">**Security** </span>sub-book in the *Transfer* <span class="span_2" style="font-style: italic;">**CFT**</span> documentation.

## Certificates

Refer to the [Transfer CFT {{< TransferCFT/suitevariablesDocTypeUser  >}}](https://docs.axway.com/bundle/TransferCFT_38_UsersGuide_allOS_en_HTML5/page/Content/AxwayStartPage.htm) for more information on certificates.

## Configuration changes

You must define certain elements in the product configuration if you want to use transfer security. See the delivered samples in CFTPGM/CFTSRC(TCPPARAM).

To use the PKIUTIL utility:

1. Access the Transfer CFT &lt;span class="italic\_in\_para">Main Menu&lt;/span> screen. In the Main Menu enter the command &lt;span class="code">&lt;code>cft&lt;/code>&lt;/span> and press &lt;span class="bold\_in\_para">&lt;b>Enter&lt;/b>&lt;/span> to open the &lt;span class="italic\_in\_para">Manager Menu&lt;/span>.
    &lt;/li>
1. Select <span class="bold_in_para">****option**** </span>**2. Security commands**. Then select option **2. Interpret Security configuration** and enter the member you want to interpret. By default this is the PKIBASE member in the UTIN file in CFTPROD library.
1. To edit the security configuration file selection option 2. Security commands then option 1. Edit Security configuration file and enter the member you want to edit. By default it is the PKIBASE member in the UTIN file in CFTPROD library.

## Submitting PKIUTIL commands

<span class="bold_in_para">****Select option**** </span>**<span class="bold_in_para">****2. Security commands****</span>**, and then**<span class="bold_in_para"> ****3. PKIUTIL operation****</span>** in the Operations screen to start the <span class="bold_in_para">****PKUTIL session****</span>.

This option allows you to use the keyboard to enter and execute PKIUTIL commands.

```
PKIU20I
PKIU20I PKI
PKIU20I Version 3.2.4 2017/02/02
PKIU20I (C) Copyright AXWAY 1989-2017
PKIU20I ====> Starting Session on 03/03/2017 Time is 16:20:37
PKIU20I
 
===> LISTPKI
```

## Create a database

Use the following commands, in order, to create a database:

```
PKIFILE MODE=CREATE, FNAME= 'CFTPROD/PKIBASE'
```

 

```
PKICER ID=NEWCA, MODE=CREATE, PKIFNAME=CFTPROD/PKIBASE, ITYPE=ROOT,
INAME=CFTPROD/AXWRCA, IFORM=DER, STATE=ACT
```

 

```
PKICER ID=NEWUSER,MODE=CREATE, PKIFNAME=CFTPROD/PKIBASE, INAME=CFTPROD/MFTUSRCA, IKNAME=CFTPROD/MFTUSRCAK, ITYPE=USER,
IKPASSW=user, STATE=ACT, ROOTCID=NEWCA, IKFORM=DER
```

### List PKI Internal datafiles contents

1. To list the PKI internal datafiles  contents, enter the command: <span class="code">` LISTPKI`</span>
1. Press ENTER to execute the command.

A correct execution displays the following messages:

```
> LISTPKI
1:¬PKU|
Date = 03/03/2017 Time = 16:24:43
PKI Fname =
 
Id.     Root   T S C K E   Exp.Date   Delivered to   Delivered by
------- -----  - - - - -  ---------- ------------- ---------------
CAXMP CAXMP   R A x      19/12/2017 CA SAMPLE FOR CA SAMPLE FOR CLIENT
CAXMP         U A x x    18/12/2017 CLIENT SAMPLE CA SAMPLE FOR SERVER
CAXMP         U A x x    18/12/2017 SERVER SAMPLE CA SAMPLE
 
PKIU00I LISTPKI _ Correct ()
```
