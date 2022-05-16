---

    title: Migrate a multi-node architecture
    linkTitle: Multi-node migration
    weight: 250

---
This section describes the upgrade of an instance of Transfer CFT z /OS configured in multi-node. Perform the sames steps as in the [non multi-node environment](../), with the only difference being the MIGRCAT, MIGRCOM and PMIGR2 procedures as described in the following sections.

## Migrate the CATALOG files (MIGRCAT)

The MIGRCAT procedure migrates one catalog file at a time. The procedure must be modified and executed for each of the nodes.

Customize the PMIGR2 step of the JCL MIGRCAT as follows, where you define RECNB, NODE, OLDFIL, TMPFIL and NEWFIL, replacing <span style="color: #ff4500;">X</span> with the node number.

```
//PMIGR2  EXEC PMIGR2,XEXPORT=&XEXPORT,
//     OUT=&OUT,
//     UNIT=&CFTUNIT,
//     CFTLOAD=&CFTLOAD,
//     OLDEXEC=&OLDEXEC,
//     SPACE=&SEP&TMPSCAT&SEP,
//     SER='DK231F',
//     SPACE=&SEP&TMPSCAT&SEP,
<span class="span_2">//   <span style="color: #ff4500;">  NODE='X',                  => node id x   </span></span>
//     <span class="span_1">OLDFIL=Source.CATALOG.N0</span><span class="span_3" style="color: #ff4500;">X</span>, => Source CATALOG node 0X      
//     <span class="span_1">TMPFIL=</span><span class="span_1">Prefix.WORK.CATALOG.N0</span><span class="span_3" style="color: #ff4500;">X</span>,   => Temporary file (size defined by &TMPSCAT)
//     <span class="span_1">NEWFIL=Target.CATALOG.N0</span><span class="span_3" style="color: #ff4500;">X</span>, => Target CATALOG node 0X   
//     <span class="span_1">RECNB=</span><span class="span_1">50000</span>,               => Target CATALOG records number (to be customized)
//     DISPFIL=&DISPCAT,
//     HABFNAME='NO'
 
```

Submit the procedure <span class="code">`..INSTALL(MIGRCAT)`</span>

## Migrate the communication media files

The MIGRCOM procedure migrates one media file at a time. The procedure must therefore be modified and executed for each of the nodes and for the main communication medium.

### Migrate the main communication media file

Customize the PMIGR2 step of the MIGRCOM procedure and set the variables RECNB, OLDFIL, TMPFIL and NEWFIL as follows:

```
//PMIGR2  EXEC PMIGR2,XEXPORT=&XEXPORT,
**//     OUT=&OUT,**
**//     UNIT=&CFTUNIT,**
**//     CFTLOAD=&CFTLOAD,**
**//     OLDEXEC=&OLDEXEC,**
**//     SPACE=&SEP&TMPSCOM&SEP,**
**//     SER='DK231F',**
//     OLDFIL=Source.COM,   => Source MAIN COM
//     TMPFIL=Prefix.WORK.COM,     => Temporary file (size defined by &TMPSCOM)
//     NEWFIL=Target.COM,   => Target MAIN COM 
//     RECNB=5000,          => Target MAIN COM records number (to be customized)
**//     DISPFIL=&DISPCOM,**
**//     HABFNAME='NO'**
```

****Submit the procedure ..INSTALL(MIGRCOM).****

## Migrate the nodes (node x) communication media file

The following steps must be performed for each of the nodes.

Customize the PMIGR2 parameters in the JCL MIGRCOM:

Define the variables RECNB, OLDFIL, TMPFIL and NEWFIL. Replace <span style="color: #ff4500;">X</span> with the node number.

```
//PMIGR2  EXEC PMIGR2,XEXPORT=&XEXPORT,
**//     OUT=&OUT,**
**//     UNIT=&CFTUNIT,**
**//     CFTLOAD=&CFTLOAD,**
**//     OLDEXEC=&OLDEXEC,**
**//     SPACE=&SEP&TMPSCOM&SEP,**
**//     SER='DK231F',**
//     OLDFIL=Source.COM.N0<span style="color: #ff4500;">X</span>,  => Source COM node <span style="color: #ff4500;">0X </span>       
//     TMPFIL=Prefix.WORK.MCOM.N0X, => Temporary file (size defined by &TMPSCOM) 
//     NEWFIL=Target.COM.N0X,  => Target COM node <span style="color: #ff4500;">0X</span>
//     RECNB=5000,             => Target COM node <span style="color: #ff4500;">0X</span> records number (to be customized)
**//     DISPFIL=&DISPCOM,**
**//     HABFNAME='NO'**
```

****Submit the procedure ..INSTALL(MIGRCOM).****

## Customize the JCL ..INSTALL (MNRMON)

Set the Transfer CFT submission option, by STC or JCL, as for the source instance.

## Customize the procedure ..INSTALL(PCFTUTIL)and INCLUDE(CFTINC)

Comment the following lines:

```
//\*CFTCAT DD DISP=SHR,//\* DSN=&QUAL..CATALOG
```
