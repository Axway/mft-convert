{
    "title": "File protection",
    "linkTitle": "File protection",
    "weight": "340"
}The section [Transfer CFT z/OS files](../file_lists_zos) provides a list of files used by Transfer CFT and, depending on the user group, indicates access rights. The default file access rights must be UACC(NONE). Privileges, which are based on user type, are detailed in the following sections:

- Program libraries
- Security files
- Secure files

Program libraries
-----------------

The security system requires Transfer CFT programs to be located in APF libraries.

If a user has developed specific applications using the Transfer CFT API to access Transfer CFT files (COM, CATALOG, etc.), the main programs (APPLIx) must:

- Be authorized (AC=1)

<!-- -->

- Be located in an APF library

<!-- -->

- Define the pgmuser program in the PROGRAM class:  
      
    REDEFINE PROGRAM pgmuser UACC(EXECUTE) OWNER(grpcft)-ADDMEN(‘pgmuser library’//NOPADCHK)  
      
    PERMIT pgmuser CLASS(PROGRAM) ACCESS(READ) -  
    ID(grpcft grpmon grpaprm)    
    SETROPTS WHEN(PROGRAM) REFRESH

<!-- -->

- Authorize file access under program control:  
      
    PERMIT‘cftv2.CATALOG’ GENERIC ACCESS(READ) -  
    ID(grpcft grpmon grpaprm …) WHEN( PROGRAM(pgmuser))  
      
    PERMIT ‘cftv2.COM’ GENERIC ACCESS(UPDATE) -  
    ID(grpcft grpmon grpaprm …) WHEN( PROGRAM(pgmuser))

<!-- -->

- Recompiled and link-edited existing applications with the authorized Transfer CFT APIs

All users executing Transfer CFT commands are assigned to a predefined group (GRPTRF).

Only the GRPMON group must have the right to execute the CFTMAIN and CFTCOPL programs.

Security files
--------------

SECINI, SECACT and SECOBJ files must be made available in read mode to all Transfer CFT program users (these files do not contain confidential data).

Although each secure file can point to a dedicated group of security files, this is not recommended, because it may complicate RACF profile management.

Secure files
------------

Secure files (PARM, PART, COM, UCONF and CATALOG) access differs according to the user type.

- Any user with the right to update the whole file has the UPDATE file access right.

<!-- -->

- Any user with the right to read the whole file has the READ file access right.

<!-- -->

- Any user with the right to update part of the file has the UPDATE WHEN (PROGRAM(CFTUTIL CFTCOPL …)) access right.

<!-- -->

- Any user with the right to read part of the file has the READ WHEN (PROGRAM(CFTUTIL CFTCOPL …)) access right.
