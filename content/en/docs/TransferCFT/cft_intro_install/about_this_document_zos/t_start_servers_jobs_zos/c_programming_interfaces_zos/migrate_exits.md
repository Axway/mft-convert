{
    "title": "How to migrate DLL exits",
    "linkTitle": "How to migrate DLL exits",
    "weight": "280"
}This section describes how to migrate DLL exits for Transfer CFT in a z/OS environment.

**Considerations prior to migrating**

- Is the exit still valid? Could you instead replace it with a new Transfer CFT processing procedure?
- Is the exit written in assembler? If so, would it be better to use a more accessible language, such as C?

Preparation
-----------

Use the examples supplied in distribution libraries as a basis for your migration. Each example provides recommendations for compilation and link-edits, as well as adequate settings for CFTEXIT. The body of the exit should not be affected; the updates concern the encapsulation of the entire exit's Prologue and Epilogue (especially in assembler).

> **Note**
>
> Note: For API/Batch C and COBOL recompilation, the correct link-edit options (DLL,…) may be sufficient to migrate APIs. For assembler, see the recommendations in the following section.

Migrate the Assembler API exit
------------------------------

Note the following:

- Reserve and do not use register 12 (mandatory for the Language Environment).
- The use of the V24 format for the API catalog requires additional registers.
- Calls to Transfer CFT APIs are performed via the CEEPCALL macro.
- The CEEENTRY macro creates the prologue.
- The CEETERM macro creates the epilogue.
- Since the exit is LE compatible, this means it can use certain C functions (for example PRINTF, etc.).

### Known issues

- There is an inconsistency between the format of the exit in the CFTEXIT and the macro version, the C structure, and COBOL copy chosen in the exit.
- For assembler, an issue occurs when using register 12.

How to link-edit
----------------

### Reentrancy / amode / rmode

- The examples and link-edit plans are delivered with reentrant rmode=any, and amode (31), the recommended setting.
- If the source code requires that you change other values, when you modify the link-edit options remember that the exit may have the AC link (1) option.

### Post migration DLL maintenance

It is recommended that for each version change you recompile and link the exits and the APIs, especially when using the v24 communication structure version.
