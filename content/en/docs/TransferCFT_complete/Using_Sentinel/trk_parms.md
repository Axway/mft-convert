{
    "title": "TRK configuration parameters",
    "linkTitle": "TRK configuration parameters",
    "weight": "210"
}To limit the number of messages related to monitoring the XFB.Transfer class, due to a large number of transfers, you can set the monitoring parameters as described in this section.

Parameters to regulate monitoring can have one of the following values:

- NO: no monitoring
- ALL: full monitoring (for each transfer status change)
- SUMMARY: summary monitoring (created at end of the transfer)
- UNDEFINED: undefined value
- ERROR: all unsuccessful transfers (where the state can be Canceled, Suspended, or Interrupted - as described in [XFBTransfer system attributes](../intro_sentinel/pesit_prot_sentinel))

For a transfer command, if Sentinel monitoring is implemented, these parameters are analyzed in the following order: transfer command, transfer definition, partner definition, general parameter (CFTPARM), and lastly the UCONF parameter definition(sentinel.xfb.transfer). If the uconf is not defined, you can set it using the command `CFTUTIL uconfset id=sentinel.xfb.transfer`.

****Parameters to regulate level of monitoring messages****

The parameter definitions are taken into account in the order listed below. For example, the transfer request definition takes precedence over the partner definition.

> **Note**
>
> Note: When using Central Governance to manage Transfer CFT, you can only use TRK at the flow level, which corresponds to transfer models, but not for partners and general parameters.


| Definition  | NO  | ALL  | SUMMARY  | ERROR  | UNDEFINED  |
| --- | --- | --- | --- | --- | --- |
| 1. Transfer requests (SEND/RECV in requester mode only)  | No tracking  | Full tracking  | First and last  | Errors only  | Uses the transfer definition  |
| 2. Transfer models (CFTSEND/CFTRECV) | No tracking | Full tracking | First and last | Errors only  | Uses the partner definition  |
| 3. Partners | No tracking | Full tracking | First and last  | Errors only  | Uses the general parameter definition  |
| 4. General parameters (CFTPARM)  | No tracking  | Full tracking  | First and last  | Errors only  | Uses the UCONF definition (sentinel.xfb.transfer)  |
| 5. UCONF definition  | No<br/> tracking | Full tracking  | First and last  | Errors only  | N/A  |


### Check Transfer CFT's Sentinel task (CFTTRK) activity

Enter the following command to get the Transfer CFT CFTTRK details:

```
cftutil mquery object=SYSTEM
```

When Sentinel is available, the following messages display in the log:

```
CFTI24I CFTTRK MQUERY OBJECT=SYSTEM
CFTI24I CFTTRK Nb max messages = 100
CFTI24I CFTTRK nb messages = 65
CFTI24I CFTTRK Sentinel state = connected
```

If Sentinel is not available, or has been disabled, the following messages display in the log:

```
CFTI24I CFTTRK MQUERY OBJECT=SYSTEM
CFTI24I CFTTRK Nb max messages = 100
CFTI24I CFTTRK nb messages = 102
CFTI24I CFTTRK Sentinel state = disconnected
```

Command parameters
------------------

****CFTPARM****

TRKPART = { \*UNDEFINED \*&#124; ALL &#124; SUMMARY &#124; NO &#124; ERROR }

TRKSEND = { \*UNDEFINED \*&#124; ALL &#124; SUMMARY &#124; NO &#124; ERROR }

TRKRECV =
{ \*UNDEFINED \*&#124; ALL &#124; SUMMARY &#124; NO &#124; ERROR }

These parameters define the TRK default settings for the CFTPART, CFTSEND, and CFTRECV commands respectively.

****SEND/RECV****

TRK =
{ \*UNDEFINED\* &#124; ALL &#124; SUMMARY &#124; NO &#124; ERROR }

An optional parameter with a default value of UNDEFINED. Enables tracking for a query.

****CFTSEND/CFTRECV****

TRK =
{ UNDEFINED &#124; ALL &#124; SUMMARY &#124; NO &#124; ERROR }

An optional parameter having TRKSEND/TRKRECV as the default values. Enables tracking for the file model (IDF).

****CFTPART****

TRK = { UNDEFINED &#124; ALL &#124; SUMMARY &#124; NO &#124; ERROR }

An optional parameter having TRKPART as the default value. Enables tracking for a partner.
