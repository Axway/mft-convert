{
    "title": "Synchronous communication return codes",
    "linkTitle": "Synchronous communication return codes",
    "weight": "310"
}This section describes diagnosing the return code when using synchronous communication with {{< TransferCFT/axwayvariablesComponentShortName  >}}.

How to find the return code
---------------------------

You can retrieve synchronous communication return codes using either a programming interface, such as CAPI, or CFTUTIL.

### Using a programming interface

In this method the `cftau `function locates the return code, as shown in the following example:

```
…
rc = cftau (“SWAITCAT”,” SELECT='IDTU==A000001’”)
if rc != 0 {
printf("SWAITCAT NOK RC=%d\\n", rc);
…
}
```

### Using CFTUTIL

This method uses the internal variable`_CMDRET` in the SEND, RECV, or SWAITCAT commands to retrieve the return code. You can create a script similar to the following example:

```
SWAITCAT SELECT='IDTU==”A000001”’
IF NAME = _CMDRET, VALUE = 0, TYPE = NEQ
PRINT MSG=”SWAITCAT NOK RC=”
PRINT MSG=%_CMDRET%
ENDIF
```

Note though that the _CMDRET value is not the same as the CFTUTIL return code, which could be:

- 0: no error
- 4: warning
- 8: error

In the following example the synchronous communication return code is 82, while the CFTUTIL is 8:

```
5:[CFU] SWAITCAT SELECT='IDTU=="A000001"'
CFTU25E SWAITCAT _ Error (SWAITCAT_NFOUND: select='IDTU=="A000001"' Not Found)
CFTU00I (SELECT='IDTU=="A000001"')
6:[CFU] PRINT MSG=%_CMDRET%
**82
CFTU00I PRINT _ Correct (MSG=82)
7:[CFU] /END
7:[CFU] CFTU00I RETURN _ Correct ( CODE=8
)
CFTU20I Number of Command(s) 6
CFTU20I Number of error(s) 1
CFTU20I Ending Session on 21/06/2012 Time is 16:06:32
CFTU20I Session active for 0:01:42
```
**

Send and receive related errors
-------------------------------

For the return codes 70 through 79, the error occurred in the SEND or RECV transfer command, and the transfer was not executed.


| RC  | Error  | Description  | Action  |
| --- | --- | --- | --- |
| 70  | APIS_READ_CONF_FILE  | Media configuration file error.  |   |
| 71  | APIS_PARAM_TIMEOUT  | TIMEOUT parameter error.  | See [TIMEOUT](../../../c_intro_userinterfaces/command_summary/parameter_intro/timeout).  |
| 72  | APIS_PARAM_LOWPORT  | LOWPORT parameter error.  | See [LOWPORT](../../../c_intro_userinterfaces/command_summary/parameter_intro/lowport).  |
| 73  | APIS_PARAM_HIGHPORT  | HIGHPOINT parameter error.  | See [HIGHPORT](../../../c_intro_userinterfaces/command_summary/parameter_intro/highport).  |
| 74  | APIS_PARAM_BOTHPORT  | LOWPORT and HIGHPORT are incompatible or incorrect.  | See above parameters.  |
| 75  | APIS_CREATE_SOCKET  | Create channel failed.  |   |
| 76  | APIS_OPEN_SOCKET  | Open channel failed.  |   |
| 77  | APIS_SOCKET_WRITE  | Channel write error occurred.  |   |
| 78  | APIS_SOCKET_READ  | Channel read error occurred  |   |
| 79  | APIS_CLOSE_SOCKET  | Close channel failed occurred.  |   |


SWAITCAT related errors
-----------------------

For the return codes 80 through 87, the error is related to the SWAITCAT command. Refer to the SWAITCAT [Concepts](../../../c_intro_userinterfaces/about_cftutil/managing_transfer_states/swaitcat_concepts) or [Examples](../../../app_integration_intro/synch_comm_tcpip_intro/sync_transfer_request_tasks) sections for more information.


| RC  | Error  | Description  | Action  |
| --- | --- | --- | --- |
| 80  | APIS_SWAITCAT_FAILED  | Transfer error.The transfer reached the K or H state.  | Check the diagnostic, correct problem if necessary, and restart.  |
| 81  | APIS_SWAITCAT_TIMEOUT  | The transfer was not completed within the time specified by the timeout value. After this timeout error, the transfer can have the status C (current) or D (for example if the network went down), but note that it was not completed.  | Check the diagnostic, correct problem if necessary.<br/> Repeat SWAITCAT, you may need to repeat several times. |
| 82  | APIS_SWAITCAT_NFOUND  | The idtu was not found. The selected criteria provided in the command SWAITCAT cannot find the transfer.  | Verify the selection parameters in the SWAITCAT command.  |
| 83  | APIS_SWAITCAT_DELETED  | Cannot find the transfer (transfer was deleted).  | The transfer was created but for some reason it was removed, for example by the DELETE command.  |
| 84  | APIS_SWAITCAT_PARAM_ERROR  | The command contained an invalid parameter.  | Error in the command syntax.<br/> Check the command parameters [here](../../../app_integration_intro/synch_comm_tcpip_intro/sync_transfer_request_tasks). |
| 87  | APIS_SWAITCAT_TOO_MANY  | The number of transfers corresponding to the filter selection for the SWAITCAT command was greater than 1. You cannot select more than 1 transfer.  | Refine the selection criteria for the command so that it returns a single transfer.  |

