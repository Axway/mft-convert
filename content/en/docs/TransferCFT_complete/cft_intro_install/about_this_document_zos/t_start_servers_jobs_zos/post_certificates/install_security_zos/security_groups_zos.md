{
    "title": "Administrator group (GRPCFT)",
    "linkTitle": "User groups",
    "weight": "320"
}### Administrator group (GRPCFT)

#### ALTER access rights for all Transfer CFT files

The administrator group is in charge of product installation and maintenance. As such, it may need to create or delete Transfer CFT files.

#### safcftcl RACF class profile owner

All safcftcl class profile definitions must specify the grpcft group as OWNER.

CONTROL access rights for all safcftcl class general resources

The administrator group must be able to perform all Transfer CFT management commands and operations.

### Monitor group (GRPMON)

#### CFTMAIN JOB owner

The only JOBs to be executed under the GRPMON group user must be the monitor start procedures, and the SWITCH – LOG and SWITCH-ACCNT commands. All CFTRECV and CFTSEND IMPL=YES commands must be defined with the USERID= and GROUPID= parameters, so that the Transfer CFT does not execute any end-of-transfer commands under user authority instead of under a correct user. 

#### CFTMAIN execution rights

For security reasons, no users other than those belonging to the GRPMON group must be able to execute the CFTMAIN program:

`PERMIT ‘CFTV2.LOADMAIN’  ID(GRPMON GRPCFT)  ACCESS(EXECUTE)`

`SECINI, SECACT, SECOBJ, UCONF, PARM and PART file read access rights:`

`PERMIT ‘CFTV2.*’          ID(GRPMON)  ACCESS(READ)PERMIT ‘CFTV2.SECINI’   ID(GRPMON)  ACCESS(READ)PERMIT ‘CFTV2.SECACT’ ID(GRPMON)  ACCESS(READ)PERMIT ‘CFTV2.SECOBJ’   ID(GRPMON)  ACCESS(READ)PERMIT ‘CFTV2.PARM’   ID(GRPMON)  ACCESS(READ)PERMIT ‘CFTV2.PART’        ID(GRPMON)  ACCESS(READ)`

`COM, CATALOG, LOG and ACCNT file update access rights:`

`PERMIT ‘CFTV2.COM’            ID(GRPMON)  ACCESS(UPDATE)PERMIT ‘CFTV2.CATALOG’  ID(GRPMON)  ACCESS(UPDATE)PERMIT ‘CFTV2.LOG1’         ID(GRPMON)  ACCESS(UPDATE)PERMIT ‘CFTV2.ACCNT1’    ID(GRPMON)  ACCESS(UPDATE)PERMIT ‘CFTV2.UCONF’    ID(GRPMON)  ACCESS(UPDATE)`

`Read access rights for all procedure files (EXEC parameter):`

`PERMIT ‘XXX.EXEC’  ID(GRPMON  … )  ACCESS(READ)`

`Execution rights for all EXIT programs (CFTEXIT command):`

EXIT programs are located in the library containing the CFTMAIN program.

> **Note**
>
> Note: The GRPMON group must be the only group in charge of Transfer CFT (CFTMAIN) execution, for all Transfer CFT environments that depend on the same RACF mechanism and regardless of their role (production or test). This group has no reason to have specific access rights with regard to the files that are sent or received, as transfers (FNAME and WFNAME file allocation and EXEC procedure submission) are performed with the user of the transfer owner.

### Configuration group (GRPAPRM)

This group allows any user belonging to the group to edit all configuration objects protected by the safcftcl class profiles: 

- SECINI, SECACT and SECOBJ file read access rights.
- PARM, UCONF and PART file write access rights:

PERMIT ‘CFTV2.PARM’ ID(GRPAPRM)ACCES(UPDATE)  
PERMIT ‘CFTV2.PART’ ID(GRPAPRM)ACCES(UPDATE)

PERMIT ‘CFTV2.UCONF’ ID(GRPAPRM)ACCES(UPDATE)

Note: The GRPAPRM group must be the only group in charge of Transfer CFT configuration files (PARM and PART) for all Transfer CFT environments that depend on the same RACF mechanism, regardless of their role (production or test).

### Parameter file access group (GRPFPRM)

This group defines PARM and PART parameter file access rights so that any user belonging to this group can, under Transfer CFT program control, edit the PARM and PART files.

CFTUTIL, CFTCOPL execution rights:

PERMIT ‘CFTV2.LOAD’  ID(GRPFPRM)  ACCESS(EXECUTE)

SECINI, SECACT and SECOBJ file read access rights.

PARM and PART file write access rights:

PERMIT ‘CFTV2.PARM’  ID(GRPFPRM)  ACCESS(UPDATE) WHEN(PROGRAM(CFTUTIL  …))  
PERMIT ‘CFTV2.PART’  ID(GRPFPRM)  ACCESS(UPDATE) WHEN(PROGRAM(CFTUTIL  …))

### Help desk group (GRPDESK)

Users belonging to this group can submit Transfer CFT management commands (SHUT, LISTCAT, ACT, INACT, and so on) or transfer management commands (DELETE, START, and so on).

CFTUTIL, CFTCOPL execution rights:

`PERMIT ‘CFTV2.LOAD’  ID(GRPDESK)  ACCESS(EXECUTE)`

`SECINI, SECACT and SECOBJ file read access rights.`

`CATALOG, UCONF and PARM file read access rights under CFT utility control:`

`PERMIT ‘CFTV2.CATALOG’  ID(GRPDESK)  ACCESS(READ) WHEN(PROGRAM(CFTUTIL  …))PERMIT ‘CFTV2.PARM’  ID(GRPDESK)  ACCESS(READ) WHEN(PROGRAM(CFTUTIL  …))`

`PERMIT ‘CFTV2.UCONF’  ID(GRPDESK)  ACCESS(READ) WHEN(PROGRAM(CFTUTIL  …))`

` `

`PART and COM file write access rights under CFT utility control:`

`PERMIT ‘CFTV2.PART’  ID(GRPDESK)  ACCESS(UPDATE) WHEN(PROGRAM(CFTUTIL  …))PERMIT ‘CFTV2.COM’  ID(GRPDESK)  ACCESS(UPDATE) WHEN(PROGRAM(CFTUTIL  …))`

`Access to all ALL_xxx objects and Transfer CFT management commands:`

`                        ALL_CAT, ALL_COM, ALL_PART and ALL_PARM                               SHUT, SWITCH LOG, SWITCH ACCOUNT and MQUERY`

### File transfer group (GRPTRF)

This group can define Transfer CFT file access rights, though it is not itself intended to submit transfer commands. It does not hold rights to APPL, TRANSFER, COMMUT, MESSAGE, and FILE objects.

Users submitting transfer commands (SEND, RECV, DELETE, START, and so on) must belong to this group to access Transfer CFT files, and hold specific rights to objects, such as APPL, TRANSFER and MESSAGE.

### Execution rights for utilities (CFTUTIL, CFTCOPL,…)

These are located in the CFTV2.LOAD library, and defined as follows:

`PERMIT ‘CFTV2.LOAD’  ID(GRPTRF)  ACCESS(EXECUTE)`

`SECINI, SECACT and SECOBJ file read access rights.`

`COM and CATALOG write access rights controlled by CFT utilities: `

`PERMIT ‘CFTV2.COM’  ID(GRPTRF)  ACCESS(UPDATE)WHEN(PROGRAM(CFTUTIL  …))`

`PERMIT ‘CFTV2.CATALOG’  ID(GRPTRF)  ACCESS(UPDATE)       WHEN(PROGRAM(CFTUTIL  …))`
