{
    "title": "Communication structure - directory type",
    "linkTitle": "Communication structure",
    "weight": "340"
}This topic describes the structure for the communication area in a directory
exit for C language as well as COBOL.

<span id="Structure_in_C_Language"></span>

Structure in C
--------------

If you want to keep an exit that was created in a version of Transfer
CFT prior to V2.4, you can continue to use the following communication
structure ****exitdU****
between the interface and the user program:

`typedef union      {          exausC exaC;          exausO exaO;          char buf[2048];} exitdU, *exitdUp;`

You can create an exit using the V2.4 format ****exitdnT****
communication structure between the interface and the user program as
follows:

`typedef union      {          exausC exaC;          exausO exaO;          char buf[2048];} exitdnU, *exitdnUp;`

Depending on the programming language used, the structure chosen is:

- exausC if the
    user program is written in C language
- exausO to provide
    an interface with a user program written in COBOL

The exausC and the exausO structures are described in exfus.h that is
delivered with the {{< TransferCFT/axwayvariablesComponentShortName  >}} product.

<span id="Structure_in_COBOL"></span>

### Structure in COBOL

If the user program is written in COBOL, the C-COBOL interfacing rules
must be complied with. Refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} delivered samples defined
in ****exaus.cop****.
