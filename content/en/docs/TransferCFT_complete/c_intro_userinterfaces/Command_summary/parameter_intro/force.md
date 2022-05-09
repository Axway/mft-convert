{
    "title": "force",
    "linkTitle": "force",
    "weight": "1230"
}<span id="force"></span>

### force

#### CFTRECV

**[FORCE = {<span class="underline">NO</span> &#124; YES}]**

Determines the priority with which
the parameters set in CFTRECV are taken into account relative to the parameters
set in an associated RECV command.

- NO: The parameters specified in the
    RECV command take priority over the parameters defined in the associated
    CFTRECV object. Thus, if a parameter is not explicitly defined in a RECV
    command but is in a CFTRECV object, the value defined in the CFTRECV object
    is taken into account.
- YES: The parameters specified in CFTRECV
    take priority over the parameters defined in an associated RECV command.
    Thus, if a parameter is not explicitly defined in a CFTRECV object but
    is in a RECV command or if the value defined in CFTRECV is the default
    value, the value defined in the RECV command is taken into account.  
    A message informs users that their command has been partially taken
    into account.

#### CFTSEND

**[FORCE = {<span class="underline">NO</span> &#124; YES}]**

Determines the priority with which
the parameters set in CFTSEND are taken into account relative to the parameters
set in an associated SEND command.

- NO: The parameters specified in the
    SEND command take priority over the parameters defined in the associated
    CFTSEND command. Thus, if a parameter is not explicitly defined in a SEND
    command but is in a CFTSEND command, the value defined in the CFTSEND
    command is taken into account.

<!-- -->

- YES: The parameters specified in CFTSEND
    take priority over the parameters defined in an associated SEND command.
    Therefore, if a parameter is not explicitly defined in the CFTSEND object
    but is in a SEND command or if the value defined in CFTSEND is the default
    value, the value defined in the SEND command is taken into account.  
    A message informs users that their command has been partially taken
    into account.

#### CFTRECV, CFTSEND

The following table summarizes {{< TransferCFT/axwayvariablesComponentShortName  >}} processing on a CFTRECV
or CFTSEND object with parameter FORCE set to YES.

****Parameters Default value Processing (FORCE = YES)****


| Parameter | Default | Processing when FORCE = YES |
| --- | --- | --- |
| EXEC, PARM, SAPPL, RAPPL, SUSER, RUSER, COMMENT, NFNAME, EXIT, WFNAME, SELFNAME, NOTIFY, USERID, GROUPID, XLATE, SPART | Empty string | If parameter is not set or set to a blank value, it can be override for a SEND/RECV command.If parameter is set to a true value, it cannot be override for a SEND/RECV command.If parameter is set to &lt;NULL&gt; (a new reserved keyword), it cannot be override for a SEND/RECV command. It means that parameter is empty |
| FNAME | Empty string | If parameter is not set, it must be override for a SEND/RECV command (otherwise transfer will failed) .If parameter is set to a true value, it cannot be override for a SEND/RECV command. |
| FCODE, FRECFM, NCODE, NRECFM | Blank char. | If parameter is not set, it can be override for a SEND/RECV command. If parameter is set to a true value, it cannot be override for a SEND/RECV command. |
| FTYPE, NTYPE, FACC | Null char. | If parameter is not set, it can be override for a SEND/RECV command. If parameter is set to a true value, it cannot be override for a SEND/RECV command. |
| FBLKSIZE, FKEYLEN, FKEYPOS, FLRECL, FSPACE, NBLKSIZE, NKEYLEN, NKEYPOS, NLRECL | Null value | If parameter is not set or set to a null value, it can be override for a SEND/RECV command. If parameter is set to a true value, it cannot be override for a SEND/RECV command. |
| FORG SEQ |   | Parameter cannot be overridden |
| PRI 128 |   | Parameter cannot be overridden |
| STATE DISP |   | Parameter cannot be overridden |
| FACTION NONE |   | Parameter cannot be overridden |
| FDISP SHR |   |   |
| NCOMP | 15 | If parameter is not set or set to value 15, it can be override for a SEND/RECV command. Otherwise, it cannot be overridden for a SEND/RECV command. |
| DELETE | NO | Parameter cannot be overridden |
| TRK | U undefined | Parameter cannot be overridden |
| TCYCLECYCLE |   | Parameter cannot be overridden |
| DAY0 |   | Parameter cannot be overridden |
| OPERMSG | &lt;os dep.&gt; | Parameter cannot be overridden |
| EXECSUB | LIST | Parameter cannot be overridden |


<span id="INACT_force"></span>

#### INACT, DELETE, END, HALT, KEEP, START, RESUME

****[FORCE = {<span class="underline">NO</span> &#124; YES}]****

 

<span id="START_force"></span>

#### START

****[FORCE = {<span class="underline">NO</span> &#124; YES}]****

Indicates whether a request, that was not executed during its time slot,
has been restarted:

- NO:
    no action is taken (default value)
- YES:
    the request is restarted immediately if you have either a:
    -   Diagnostic
        code diagi 302
    -   Cyclic request
        in the D state

The maximum execution time for a restarted request is 23595999.

Irrespective
of the FORCE parameter value, a START command does not affect a request
that has not yet reached the minimum execution time.

[Return to Command index](../../)
