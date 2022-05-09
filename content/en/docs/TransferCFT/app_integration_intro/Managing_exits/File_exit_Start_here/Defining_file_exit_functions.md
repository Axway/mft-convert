{
    "title": "Define  functions",
    "linkTitle": "Defining functions",
    "weight": "340"
}This topic describes the functions in an exit task and the parameters
involved. It is comprised of the following sections:

- [Initialization
    function](#Initialization_Function)
- [User
    function](#User_function_s_)
- [Interface
    files](#Interface_Files)

A file exit task comprises the following two modules:

- {{< TransferCFT/axwayvariablesComponentShortName  >}} interface
- User program

The interface is written in C language. The main entry point of the
EXIT task, the main function in C language, is located in the interface.
You do not need to know how this interface works to write a user program.

The user program can be written in C or in COBOL. If you use COBOL,
you must comply with the standard C-COBOL interfacing rules.

Once the user program has been written and compiled, the two modules
are linked together with the appropriate system libraries. The result
of link editing forms an EXIT task.

You must define two types of functions in your programs:

- An initialization
    function: **exfini**
- One or more functions
    that will be referred to as: **EXFxmp1**

A file type EXIT task:

- Calls the main
    entry point (interface code) on activating the task
- Calls the initialization
    function **exfini** at the beginning of a transfer whose identifier
    (idf) is linked to the EXIT task
- Calls the user
    function **EXFxmp1** whenever the user wants to take control

<span id="Initialization_Function"></span>

Initialization function
-----------------------

### About the initialization function

The initialization function **exfini** must specify the stages at
which you want to take control. It must supply the address of the **EXFxmp1**
function to be called if you want to take control at one stage at least.

****Restrictions****

- You cannot change a user function during the transfer.
- The initialization function cannot be written in COBOL since its main
    purpose is to indicate the address of a user function. You must use a
    language such as C language or Assembler.

### Parameters

The following table lists all the parameters of the initialization function.


| Parameter | Explanation |
| --- | --- |
| ex_name | Address of an (512+1) byte area.<br/> The initialization function can modify this area. |
| idf | Address of an (32+1) byte area.<br/> This area contains the file identifier of the transfer in process. |
| parm | Address of an (64+1) byte area.<br/> This area contains the value of the PARM parameter of the CFTEXIT command and can be modified by the initialization function. |
| language | Address of a one-byte area.<br/> This area contains the value of the LANGUAGE parameter of the CFTEXIT command and can be modified by the initialization function. |
| pfonc | Address of an area that contains the address of the user function.<br/> If you want to take control, the initialization function must update this area. The idf, parm and language parameters are initialized by the interface. The idf and parm parameters can be used to select a function when the user has provided several functions.<br/> The user may use the exfref parameter as required. The contents of this parameter are thereafter provided to the user function.<br/> The (n+1) byte areas contain characters up to n bytes long followed by a binary zero. |
| Return codes | The possible values are:<br/> • 0: the user wants to take control at one stage at least<br/> • 1: the user does not want to take control |


### Example in C

```
long exfini
( char     \*ex_name,
          char    
\*idf,
          char    
\*parm,
          char    
\*language,
          EXF    
\*pfonc);
```

Where EXF is defined as:

```
typedef long (\*EXF)
(char\*,char\*,char\*,char\*);
```
<span id="User_function_s_"></span>

User functions
--------------

The following table describes the parameters involved in the user functions.


|   |   |
| --- | --- |
| Parameter | Explanation |
| psCtx | Address of the interface communication area.<br/> Also known as context table or transfer context, this area is:<br/> • Allocated by the interface for each transfer<br/> • Updated by the interface before each call of the user function<br/> • Freed by the interface at the end of the transfer<br/> For simultaneous transfers using a given EXIT, the interface manages the context tables. Each transfer has its own context.<br/> Some fields of the context table can be updated by the user function. For more information, refer to the [About the communication structure](../file_exit_communication_area_) topic.  |
| psCtxWork | Address of the user working area.<br/> This area, allocated and de-allocated by the interface for each transfer, enables users to save the information they consider necessary for the purposes of the processing performed.<br/> Its size equals the value of the RESERV parameter of the CFTEXIT object. |
| psData | Address of the data area.<br/> This area, allocated and de-allocated by the interface for each transfer, contains the record to be sent or the record received.<br/> Its size is limited to 4096 bytes. The effective size of the data is specified in the context table.<br/> You can modify the data at certain stages. You must then change the effective size of the data in the context table. For more information, refer to [About the communication structure](../file_exit_communication_area_). |
| psWork | Address of the global communication area (1024 bytes).<br/> This area is allocated and reset to 0 by the interface once and for all at the time the EXIT task is activated, and then freed by the interface at the time it is de-activated.<br/> You can use this area to save the information common to all the calls of user functions.<br/> This area must be used with care if the value of the WAITTASK parameter of the CFTEXIT command is not 1441 (EXIT task permanently present in memory). |
| Return codes | Not used |


### Example in C language

```
long EXFxmp1   (char
\*psCtx,
          char
\*psCtxWork,
         char
\*psData,
          char
\*psWork)
```
<span id="Interface_Files"></span>

Interface files
---------------

The following table describes the interface files.


| File | Explanation |
| --- | --- |
| EXFUS.H | File that contains the definition of the interface/user program communication structure<br /> For the interface and the user program  |
| EXF.H  | File for the interface  |
| EXF2MN.C  | Main entry point for the EXIT task  |
| EXF2UT.C | Miscellaneous functions for the interface  |
| EXF2RS.C | Miscellaneous functions for the interface |

