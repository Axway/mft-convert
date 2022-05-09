{
    "title": "Using  the communication area ",
    "linkTitle": "Using the communication area",
    "weight": "360"
}<span id="Communication_area_structure__End_of_transfer_exit"></span>This topic describes basic rules for the communication area for an end-of-transfer
type exit.

The interface provides the values used by the communication structure
before the user function is called. In return, you must supply the parameters
that {{< TransferCFT/axwayvariablesComponentShortName  >}} requires to update the catalog and, optionally, a comment.

If the user function pointer initialized in the init function contains
a non-null value, the initialization and user functions are called whenever
an end of transfer occurs, whether it is normal or abnormal.

If the transfer terminates abnormally, only some of the fields are completed.
The remaining fields are reset to:

- ****0****
    in C
- ****blank****
    in COBOL

If the transfer state is T, the value in the ****diagp****
field indicates the compression ratio.

<span id="Communication_structure_in_C_language"></span>

Communication structure in C
----------------------------

If you want to keep an exit that was created in a version of Transfer
CFT ****prior**** to V2.4, you can continue
to use the following communication structure exitdU between the interface
and the user program:

```
typedef union {
     EXEusC exeC;
     EXEusO exeO;
} EXEdU, \*EXEdUp;
```

To create an exit using the V2.4 format exitdnT communication
structure between the interface and the user program is defined below:

```
typedef union {
     EXEusC exeC;
     EXEusO exeO;
} EXEdnU, \*EXEdnUp;
```

The choice of structure depends on the programming language used:

- EXEusC:
    if the user program is written in C
- EXEusO:
    to provide an interface with a user program written in COBOL

<span id="Communication_structure_in_COBOL"></span>

### Communication structure in COBOL

If the user program is written in COBOL, C/COBOL interfacing rules must
be respected.
