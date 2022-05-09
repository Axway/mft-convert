{
    "title": "Developing  exits",
    "linkTitle": "Developing exits",
    "weight": "270"
}This topic describes using exits
in Transfer CFT Windows. Every Transfer CFT is supplied
with a toolkit that enables you to develop your own EXITs
in C. This topic provides additional information
for developing exits.

The following source examples are supplied in the toolkit:

- File exit
- Directory EXIT

Development environment compilers
---------------------------------

You should develop the exits under the Windows operating system, using
Microsoft Visual Studio.

****Before you begin developing exits****

You are strongly advised to do the following before starting the development
work itself:

1. Read the general section relating to
    the [developing exits](../../../../../app_integration_intro/managing_exits).
1. Read this section.
1. Familiarize yourself with the sample sources in `runtime\src\exit`.
1. Reconstruct the executable
    in the sample from the C source and the other files supplied: libraries,
    definition files, make files.
1. Test the exit of the sample
    you want to construct.

<span id="Functions_associated_with_exits"></span>

Functions associated with exits
-------------------------------

This section provides information for implementing exits that have already been developed and supplied with
the product, or that are developed specifically by and for a particular
user. See Using exits.

This section describes:

- The different types of exits:
- File exits
- Exit list
- Directory exits

<!-- -->

- How to proceed
    when developing an exit
- How to define Transfer
    CFT parameters so that it will take account of the different types of
    exits

The Transfer CFT *[Exit list guide](#Developing_exits)* provides a
description of a special EXIT file that is already developed
and supplied with Transfer CFT.

<span id="Dynamic_identification_exit_upon_connection"></span>

### Dynamic identification exit upon connection

The aim of the directory type EXIT is to enable the Transfer CFT user
to enter dynamically a user name and/or a password at the connection stage,
before transmission in request mode.

The prefix for the executable corresponding to this EXIT is CFTEXPWD
and it must contain data for the PROG parameter of the CFTEXIT command
in the Transfer CFT parameterization.

The way the EXIT functions depends on the NSPART and NSPASSW parameters
in the CFTPART command:

- If these parameters
    contain any text strings, these strings will be used when the connection
    is made.
- If these parameters
    contain the string of "\*" (excluding the quotation marks), NSPART
    and NSPASSW are entered from a dialog box when the first connection is
    made to the partner.
- If these parameters
    contain the string of "\*\*" (excluding the quotation marks),
    NSPART and NSPASSW are entered from a dialog box every time a connection
    is made to the partner.
- NSPART and NSPASSW
    may contain different strings; so if NSPART is entered and NSPASSW = \*\*,
    then only the password will be requested each time a connection is made
    to the partner.  

****Example  
****

`CFTPART ID=PART1`  
`NSPART=*`  
`NSPASSW = **`  

In this example, Transfer CFT will request the NSPART
to be entered when the connection is made for the first time, and for
the password NSPASSW to be entered every time a connection is made.

<span id="Exit_list"></span>

### Exit list

The Transfer CFT EXIT list is an exit that enables remote partners to consult the Transfer
CFT catalog on the central site or on a server. The *Exit list guide* describes the functions provided by
the Exit list and gives the indications required to implement them.

Transfer CFT Windows supplies the exit list in the form
of an executable *cftexl.exe* (loaded into memory when the consultation
takes place), accompanied by a sample file containing the selection criteria,
*exitlist.txt*, which allows the data to be output by the Exit list
to be selected from the central site (or from the server). To use the exit list you also need a definition file CFTNMLOG (see
the section Logical File Names, the paragraph *Using
a definition file*). This is supplied as a sample and can be used only
on condition that the file name for the selection criteria is *exitlist.txt.*

****Exit example****

Exit-list
: on server

`cftexit id      = exitl,`

`reserv = 8192,`

`prog = cftexl,`

`language = c,`

`mode = replace`

`cftsend id = texit,`

`impl = yes,`

`exit = exitl,`

`mode = replace`

****Exit-list remote partner side****

`cftrecv id = texit,`

`fname = '&idt.rcv',`

`fcode = binary,`

`frecfm = f,`

`faction = delete,ftype     = b,`

`     mode = replace`
