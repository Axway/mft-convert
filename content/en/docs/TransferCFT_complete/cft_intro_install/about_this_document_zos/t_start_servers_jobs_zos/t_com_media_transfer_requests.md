{
    "title": "Communication media JCL for transfer requests",
    "linkTitle": "Communication media JCL for transfer requests",
    "weight": "190"
}This section presents JCL examples that you can use to create the JOBs to perform transfer requests. All of these JCLs are located in the target.SAMPLE library. The following examples for send procedures are described below:

- [CFTSEND for a JCL requesting a transfer](#CFTSEND%20for%20a%20JCL%20requesting%20a%20transfer)
- [CFTSENDS synchronous transfer request API](#CFTSENDS%20synchronous%20transfer%20request%20API)

<span id="CFTSEND for a JCL requesting a transfer"></span>

### CFTSEND for a JCL requesting a transfer

The following JOB is an example of a JCL for requesting a file transfer. Using the CFTSEND example, you can create JOBs that satisfy your operating requirements.

Example

```
CFTSEND
 
//LIB    JCLLIB ORDER=(cftv2.INSTALL)
//      INCLUDE MEMBER=cftenv
//CFTSEND  EXEC PCFTUTIL,PARM='/1=&CFTENV',
//         QUAL=&CFTENV,OUT=&OUT
//CFTIN    DD \*
   SEND PART=LOOP,IDF=SAMPLE,
           FNAME=%_ARGV1%.FTEST
/\*
 
The parameters shown in bold are substituted during the customization phase.
CFTUTIL parameters:
/1= Transfer CFT prefix environment [%_ARGV1%]
This JCL contains others templates, such as:
• Transfer a pds member.
• Transfer files from a files list.
• Transfer a file using broadcast list.
• Generate and transfer a files list.
• Transfer a group files
• Transfer a group files using REGEX filter.
```
<span id="CFTSENDS synchronous transfer request API"></span>

### CFTSENDS synchronous transfer request API 

Example

```
CFTSENDS
 
//LIB    JCLLIB ORDER=( cftv2
.INSTALL)
//      INCLUDE MEMBER= cftenv
//CFTSENDS EXEC PCFTUTIL,PARM='/1=&CFTENV',
//         QUAL=&CFTENV,OUT=&OUT
/\* ----  WITH INDIRECT CONFIGURATION FILE ---- \*/
 
   CONFIG TYPE=COM,FNAME=$CFTTCP
 
   SEND PART=LOOP,IDF=SAMPLE,
      FNAME=%_ARGV1%.FTEST
The parameters shown in bold are substituted during the customization phase
CFTUTIL parameters:
/1= Transfer CFT prefix environment [%_ARGV1%]
```
<span id="CFTSENDM request deposit in XMEM mailbox"></span>

### 

****Related topics****

- [Starting and stopping the Transfer CFT servers JOBs](../)
