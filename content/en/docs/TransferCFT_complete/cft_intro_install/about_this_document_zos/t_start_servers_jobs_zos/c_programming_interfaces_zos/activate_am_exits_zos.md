{
    "title": "AM exits",
    "linkTitle": "Activate AM exits",
    "weight": "300"
}<span id="Activat"></span>

Activating a platform-generic exit
----------------------------------

1. Check that the '`am.exit.custom.safclass.value`' parameter is not set to a value.
1. Ensure that the parameter '`am.exit.custom.rbac_fname.value`' is associated with a correct configuration file.

API and CFTUTIL
---------------

Unified configuration file (UCONF) must be allocated in the execution JCL.

JCL(s) to adapt
---------------

H84SAFDF: TO CREATE CFT GENERAL RESOURCE CLASS PROFILES

RDEFINE safcftcl UI.\*\* UACC(NONE) OWNER(grpcft)

RDEFINE safcftcl CFTUCONF.\*\* UACC(NONE) OWNER(grpcft)

RDEFINE safcftcl FILE.\*\* UACC(NONE) OWNER(grpcft)

RDEFINE safcftcl BATCH.\*\* UACC(NONE) OWNER(grpcft)

RDEFINE safcftcl FILTER.LOG.\*\* UACC(NONE) OWNER(grpcft)

RDEFINE safcftcl FILTER.CATALOG.\*\* UACC(NONE) OWNER(grpcft)

RDEFINE safcftcl SENTINEL.\*\* UACC(NONE) OWNER(grpcft)

H85SAFPR: TO ISSUE RACF PERMIT COMMANDS

**Example**

PERMIT UI.\*\* CLASS(safcftcl) ACCESS(READ) -

ID(grpcft grpdesk )
