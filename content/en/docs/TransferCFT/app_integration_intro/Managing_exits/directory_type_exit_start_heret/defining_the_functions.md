{
    "title": "Defining  functions",
    "linkTitle": "Defining functions",
    "weight": "360"
}<span id="About_the_Functions"></span>This topic describes the directory exit task components and functions.
A directory EXIT task comprises two modules:

- The interface supplied
    with {{< TransferCFT/axwayvariablesComponentShortName  >}}
- The user program

The interface is written in C language. The main entry point of the
EXIT task, the principle function in C language, is located in the interface.
You do not need to know how this interface works to write a user program.

The user program can be written in C or in COBOL. When you use COBOL,
you must comply with C-COBOL interfacing rules.

Once you have written the user program, the two modules are linked together
with the appropriate system libraries. The result of link editing forms
an EXIT task.

You must define two types of functions in your programs:

- An initialization
    function **exaini**
- One or more functions
    that will be referred to as **usrfct**

A directory EXIT task:

- Calls the main
    entry point, interface code, on activating the task
- Calls the initialization
    function **exaini** function in server mode, when a connection is indicated,
    or in requester mode, when a connection request is made. If this function
    returns a 0 code, the ****usrfct**** function
    is called. If not, {{< TransferCFT/axwayvariablesComponentShortName  >}} considers that the user does not want
    to take control

<span id="Initialization_Function"></span>

Initialization function
-----------------------

### About the initialization function

The initialization function **exaini** must specify the stages at
which you want to take control. It must supply the address of the **usrfct**
function to be called if the user wants to take control at one stage at
least.

****Restriction****

The initialization function cannot be written in COBOL since its main
purpose is to indicate the address of a user function. You must use a
language such as C language or Assembler.

### Parameters

The following table lists all the parameters of the
initialization function.


| Parameter  | Description  |
| --- | --- |
| exafref | Address of an (512+1) byte area.<br/> The initialization function can modify this area. |
| mode | Address of a one-byte area<br/> Processing mode:<br/> • S: call in server mode<br/> • R: call in requester mode (Requester) |
| part | Address of an (32+1) byte area.<br/> Partner local identifier if this identifier is known to {{< TransferCFT/axwayvariablesComponentShortName  >}}. |
| parm | Address of an (64+1) byte area.<br/> This area contains the value of the PARM parameter of the CFTEXIT command and can be modified by the initialization function. |
| language | Address of a one-byte area.<br/> This area contains the value of the LANGUAGE parameter of the CFTEXIT command and can be modified by the initialization function. |
| function | Address of an area that contains the address of the user function.<br/> If you want to take control, the initialization function must update this area. The mode, part, parm and language parameters are initialized by the interface. The mode, part and parm parameters can be used to select a function when the user has provided several functions.<br/> The user can use the exaref parameter as required. The contents of this parameter are thereafter provided to the user function.<br/> The (n+1) byte areas contain characters up to n bytes long followed by a binary zero. |
| Return codes | The possible values are:<br/> • 0: the user wants to take control at one stage at least<br/> • 1: the user does not want to take control |


### Example in C

`long exaini ( char     *exaref,          char       *mode,          char       *part,          char       *parm,          char       *language,          EXA       *function );`

Where EXA is defined as:

`typedef long (*EXA)(char*,char*);`

<span id="User_Function"></span>

User Function
-------------


| Parameter  | Description  |
| --- | --- |
| zecom<br/> <br/>  | Address of the interface communication area. Also known as context table or transfer context, this area is:<br/> • allocated by the interface for each transfer<br/> • updated by the interface before each call of the user function<br/> • freed by the interface at the end of the transfer<br/> Some fields of this area can be updated by the user function. |
| zgcom | Address of the global communication area (1024 bytes).<br/> This area is allocated and reset to 0 by the interface once and for all at the time the EXIT task is activated, and then freed by the interface at the time it is de-activated.<br/> You can use this area to save the information common to all the calls of user functions.<br/>  |
| Return codes | Not used |


Example in C

`long usrfct (     char   *zecom,`

`  char   *zgcom            );`

<span id="Interface_Files"></span>

Interface files
---------------

Interface files are listed in the following table.


| File  | Description  |
| --- | --- |
| EXAUS.H | File that contains the definition of the interface/user program communication structure<br /> For the interface and the user program  |
| EXA.H  | File for the interface  |
| EXA2MN.C  | Main entry point for the EXIT task  |
| EXA2UT.C | Miscellaneous functions for the interface  |

