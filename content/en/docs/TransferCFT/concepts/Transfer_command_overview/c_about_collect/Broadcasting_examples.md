{
    "title": "Types of broadcasts ",
    "linkTitle": "Types of broadcasts",
    "weight": "320"
}**<span id="Simple_broadcasting"></span>Simple broadcasting**
-------------------------------------------------------------

Site A wants to control the broadcasting of a file in its possession,
but through an intermediate site B.

**Example of a simple broadcast**

![](/Images/TransferCFT/simple_broadcast.gif)

 

- *1st phase*

A sends to B the list of partners (C1, C2, ... Cn)
to be broadcast (LIST_B):

SEND PART=ID_B, FNAME=LIST_B, .....

This file is received by B as LIST, through the command:

CFTRECV ID=..., FNAME=LIST

- ***2**nd
    phase*

A sends the file to be broadcast to a virtual partner
C:

SEND PART=ID_C, FNAME=FILE_TO_broad, .....

Partner C cannot be reached directly, but through
B. On site A: CFTPART ID=ID_C, ...., IMINTIME=0, IMAXTIME=0, IPART=ID_B

The file FILE_TO_broad is hence received by B, which
attempts to send it on to C; C is a broadcasting list. On site B: CFTDEST
ID=C, FNAME=LIST, FOR=COMMUT

The file FILE_TO_broad is hence sent to all the partners
on the list.

<span id="Customized_broadcasting"></span>

Customized broadcasting
-----------------------

There may be several A sites (one distributing site and several production
sites). Each production site designates a different broadcasting list.

**Example of a customized broadcast**

![](/Images/TransferCFT/customized_broadcast.gif)

 

- *1st phase:*

Each site Ap sends its broadcasting list to B:  
SEND PART=ID_B, FNAME=LIST, .....

The file LIST is received by B as LIST_Ap, through
the command:  
CFTRECV ID=..., FNAME=LIST_&PART

- *2nd phase:*

Each site Ap sends a virtual partner C the file to
be broadcast (as in case I). The file is sent on by B to the list associated
with the command:  
On site B: CFTDEST ID=C, FNAME=LIST_&SPART, FOR=COMMUT

**<span id="Example_of_composite_broadcasting"></span>Composite broadcasting**
------------------------------------------------------------------------------

There may also be several B sites, for example one production site and
several distributing sites.

**Example of a composite broadcast**

![](/Images/TransferCFT/composite_broadcast.gif)

 

- 1st
    phase:  
    A broadcasts the broadcasting lists Cp1, Cp2, ... Cpn to the direct
    partners Bn:  
    SEND PART=ID_B, FNAME=LIST_&PART, .....  
      
    where B is the list of partners Bn:  
    CFTDEST ID=ID_B, PART=(ID_B1, ID_B2, .....ID_Bn), FOR=LOCAL

The file LIST is received by each Bp as "LIST",
through the command:CFTRECV ID=..., FNAME=LIST

- 2nd
    phase:  
    A sends each virtual partner Cp the file to be broadcast:  
    SEND PART=ID_C1, FNAME=FILE_TO_broad, .....  
    SEND PART=ID_C2, FNAME=FILE_TO_broad, .....  
    SEND PART=ID_Cp, FNAME=FILE_TO_broad, .....

The file can also be customized through the use of symbolic variables.
A different file can therefore be sent to each broadcasting site.

Each CFTPART ID=ID_Cp command designates Bp as an intermediate partner.

On each site Bp: CFTDEST ID=Cp, FNAME=LIST, FOR=COMMUT
