---

    title: Types of broadcasts 
    linkTitle: Types of broadcasts
    weight: 320

---
## **<span id="Simple_broadcasting"></span><span style="font-weight: normal;">Simple broadcasting</span>**

Site A wants to control the broadcasting of a file in its possession,
but through an intermediate site B.

**Example of a simple broadcast**

![](/Images/TransferCFT/simple_broadcast.gif)

 

- *1st phase*

A sends to B the list of partners (C1, C2, ... Cn)
to be broadcast (LIST\_B):

SEND PART=ID\_B, FNAME=LIST\_B, .....

This file is received by B as LIST, through the command:

CFTRECV ID=..., FNAME=LIST

- *<span style="font-style: italic;">**2**</span><span style="font-style: normal;">nd
    phase</span>*

A sends the file to be broadcast to a virtual partner
C:

SEND PART=ID\_C, FNAME=FILE\_TO\_broad, .....

Partner C cannot be reached directly, but through
B. On site A: CFTPART ID=ID\_C, ...., IMINTIME=0, IMAXTIME=0, IPART=ID\_B

The file FILE\_TO\_broad is hence received by B, which
attempts to send it on to C; C is a broadcasting list. On site B: CFTDEST
ID=C, FNAME=LIST, FOR=COMMUT

The file FILE\_TO\_broad is hence sent to all the partners
on the list.

<span id="Customized_broadcasting"></span>

## Customized broadcasting

There may be several A sites (one distributing site and several production
sites). Each production site designates a different broadcasting list.

**Example of a customized broadcast**

![](/Images/TransferCFT/customized_broadcast.gif)

 

- *1st phase:*

Each site Ap sends its broadcasting list to B:  
SEND PART=ID\_B, FNAME=LIST, .....

The file LIST is received by B as LIST\_Ap, through
the command:  
CFTRECV ID=..., FNAME=LIST\_&PART

- *2nd phase:*

Each site Ap sends a virtual partner C the file to
be broadcast (as in case I). The file is sent on by B to the list associated
with the command:  
On site B: CFTDEST ID=C, FNAME=LIST\_&SPART, FOR=COMMUT

## **<span id="Example_of_composite_broadcasting"></span>Composite broadcasting**

There may also be several B sites, for example one production site and
several distributing sites.

**Example of a composite broadcast**

![](/Images/TransferCFT/composite_broadcast.gif)

 

- 1st
    phase:  
    A broadcasts the broadcasting lists Cp1, Cp2, ... Cpn to the direct
    partners Bn:  
    SEND PART=ID\_B, FNAME=LIST\_&PART, .....  
      
    where B is the list of partners Bn:  
    CFTDEST ID=ID\_B, PART=(ID\_B1, ID\_B2, .....ID\_Bn), FOR=LOCAL

The file LIST is received by each Bp as "LIST",
through the command:CFTRECV ID=..., FNAME=LIST

- 2nd
    phase:  
    A sends each virtual partner Cp the file to be broadcast:  
    SEND PART=ID\_C1, FNAME=FILE\_TO\_broad, .....  
    SEND PART=ID\_C2, FNAME=FILE\_TO\_broad, .....  
    SEND PART=ID\_Cp, FNAME=FILE\_TO\_broad, .....

The file can also be customized through the use of symbolic variables.
A different file can therefore be sent to each broadcasting site.

Each CFTPART ID=ID\_Cp command designates Bp as an intermediate partner.

On each site Bp: CFTDEST ID=Cp, FNAME=LIST, FOR=COMMUT
