{
    "title": "Troubleshoot and example definitions",
    "linkTitle": "Troubleshoot AM exits",
    "weight": "310"
}Common mistakes
---------------

`    > ICH409I 282-014 ABEND DURING RACHECK PROCESSING`

`           -> invalid access value (if value <> 2 & 4 & 8)`

### Administrator

This file must be managed by Transfer CFT Administrator:

`control pp_level_msg 1    // 1 = Potential Perm level message`

`                         // 0 = No message`

`control trace        1    // 0 = No trace generated`

`                         // 1 = Only specific traces (default)`

`                         // 2 = All Requests SAF traced`

`control exit_trace   0    // 0 = No trace in exit main`

`                         // 1 = trace activated in exit main`

`define VIEW       2   // racf read`

`define DEFACC     8   // racf control (default access)`

`define DELETE     8   // racf control`

`define CREATE     8   // racf control`

`define SWITCH     8   // racf control`

`define SHUTDOWN   8   // racf control`

`define PURGE      8   // racf control`

`define TURN       8   // racf control`

`define PAUSE      8   // racf control`

`define SUBMIT     8   // racf update`

`define CANCEL     8   // racf update`

`define MODIFY     4   // racf update`

`define EDIT       4   // racf update`

`define CONTROL    4   // racf update`

`define EXECUTE    4   // racf update`

`define ACTIVATE   4   // racf update`

`define DEACTIVATE 4   // racf update`

`define RELOAD     4   // racf update`

`define TURN       4   // racf update`

`define READ       2   // racf read`

`define EXEC       2   // racf read`

`define CONNECT    2   // racf read`

`PP TRANSFER,VIEW                    =%I ALL_CAT,*`

`PP TRANSFER,DELETE                  =%I ALL_CAT,*`

`PP TRANSFER,*                       =%I ALL_CAT,CONTROL  // for Start, Halt, Keep, End ...`

`  Part definition`

`PP CONFIGURATION:CFTPART,*          =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S,*`

`PP CONFIGURATION:CFTDEST,*          =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S,*`

`PP CONFIGURATION:CFTX25,*           =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S,*`

`PP CONFIGURATION:CFTTCP,*           =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S,*`

`PP CONFIGURATION:CFTSNA,*           =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S,*`

`PP CONFIGURATION:CFTLU62,*          =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S,*`

`PP CONFIGURATION:CFTUCONF,VIEW      =(allow)`

`PP CONFIGURATION:CFTUCONF,*         =%I ALL_PARM,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S,*`

`  By default: PARM`

`PP CONFIGURATION:*,*                =%I ALL_PARM,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S,*`

`PP SERVICE:COM,VIEW                 =%I ALL_COM,*`

`PP SERVICE:CFTSRV,*                 =(reject)`

`PP SERVICE:BATCH,EXECUTE            =%I BATCH,*`

`PP SERVICE:CATALOG,PURGE            =%I PURGE,*`

`PP SERVICE:LOG,SWITCH               =%I SWT_LOG,*`

`PP FILTER:LOG,*                     =%I %P.%S,*`

`PP SERVICE:ACCOUNT,SWITCH           =%I SWT_ACNT,*`

`PP SERVICE:SENTINEL,*               =%I %S,*`

`PP FILTER:CATALOG,*                 =%I %P.%S,*`

`PC TRANSFER,COMMUT        =%I COMMUT.&1.1DIRECT.&ID.&PART.&FNAME,CREATE`

`PC TRANSFER,*             =%I %R.&1.1DIRECT.&ID.&PART.&FNAME,*`

`PC SERVICE:CFTSRV,SHUTDOWN          =%I SHUT,*`

`PC SERVICE:CFTSRV,STARTUP           =(reject)`

`PC SERVICE:LOG,SWITCH               =%I SWT_LOG,*`

`PC SERVICE:CATALOG,PURGE            =%I PURGE,*`

`PC SERVICE:ACCOUNT,SWITCH           =%I SWT_ACNT,*`

`PC SERVICE:BATCH,EXECUTE            =%I BATCH,EXECUTE`

`PC SERVICE:UI,CONNECT               =%I %S,*`

`PC SERVICE:COM,*                    =%I ALL_COM,*`

`PC CONFIGURATION:CFTPART,TURN       =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I TURN.&ID,*`

`PC CONFIGURATION:CFTPART,*          =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S.&ID,*`

`PC CONFIGURATION:CFTDEST,*          =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S.&ID,*`

`PC CONFIGURATION:CFTX25,*           =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S.&ID,*`

`PC CONFIGURATION:CFTTCP,*           =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S.&ID,*`

`PC CONFIGURATION:CFTSNA,*           =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S.&ID,*`

`PC CONFIGURATION:CFTLU62,*          =%I ALL_PART,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S.&ID,*`

`PC CONFIGURATION:CFTUCONF,VIEW      =(allow)%I ALL_PARM,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S.&ID,*`

`PC CONFIGURATION:*,*                =%I ALL_PARM,* ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù %I %S.&ID,*`

`PC CONFIGURATION:CFTUCONF,*         =%I %S.&ID,*`

`PC FILE,VIEW                        =%I %R.&FNAME,*`

`PC FILTER:LOG,VIEW                  =%I %P.%S.&ID,* // Copilot`

`PC FILTER:CATALOG,VIEW              =%I %P.%S.&ID,* // Copilot`

`PC FILTER:LOG,*                     =%I %P.%S.&ID,* // Copilot`

`PC FILTER:CATALOG,*                 =%I %P.%S.&ID,* // Copilot`

``

`end of file`

` >> 'end of file' : The tag stops the interpretation of the definition`

Sample definition to filter (USER) access to Copilot
----------------------------------------------------

### Unified configuration file definition

The following examples are performed using CFTUTIL.

` UCONFSET ID=am.exit.custom.rbac_fname.value,`

`      value=permits.definition.file`

` UCONFSET ID=am.exit.custom.safclass.value,value=CORPCFT1`

` UCONFSET ID=am.type,value=EXIT`

` UCONFSET ID=am.exit.check_login,value=NO   /* or YES */`

### Example definitions

Define CONNECT 2

`PC SERVICE:UI,*        =%S,*`

`PC *,*                 =(allow)`

`PP *,*                 =(allow)`

### Create a 'RACF' profile

` ---  RDEFINE CORPCFT1 UI.** UACC(NONE) OWNER(......)`

### Give permissions to group(s) or user(s) ID(...)

` ---  PERMIT UI.**  CLASS(CORPCFT1) ACCESS(READ) -`

ID(... ...)
