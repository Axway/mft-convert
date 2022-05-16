---

    title: Defining  CFTTCP
    linkTitle: CFTXXX - Default networks
    weight: 470

---
This topic describes the CFTTCP command. Use this command to define the network parameters of partners
for a given type of network.

****Related
topics****

- [CFTTCP syntax](../../../command_summary#CFTTCP)


| Parameter  | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/cnxin">CNXIN</a>  | Maximum number of sessions allocated to this partner, for incoming connections (server mode).<br/> The maximum value supported for each system is indicated in the CNXINOUT parameter comment, in tabular form. |
| <a href="../../../command_summary/parameter_intro/cnxinout">CNXINOUT</a> | Maximum number of sessions for communicating with this partner over this network resource. |
| <a href="../../../command_summary/parameter_intro/cnxout">CNXOUT</a> | Maximum number of sessions for outgoing connections with this partner (requester mode).<br/> The maximum value supported for each system is indicated in the CNXINOUT parameter comment, in tabular form.<br/> The parameters CNXIN, CNXOUT and CNXINOUT are independent with respect to each other. |
| <a href="../../../command_summary/parameter_intro/id">ID</a> | Name identifying the partner.<br/> This value makes reference to a partner description (CFTPART ID = ...).<br/> When ID is set to the CFTPARM command DEFAULT parameter value, the command describes the default network configuration of a calling partner (hence operation in SERVER mode if the partner is only a REQUESTER). In this case, the parameters specific to a partner and the parameters specific to outgoing calls (DIALNO, CNXOUT, RETRY*, O*TIME) are not applicable. |
| <a href="../../../command_summary/parameter_intro/imaxtime">IMAXTIME</a> | Maximum time after which the partner can no longer call over this type of network. |
| <a href="">IMINTIME</a>  | Minimum time before which the partner cannot call over this type of network.<br/> IMINTIME, IMAXTIME hence represent the authorized time slot for calls coming from this partner over this type of network. |
| <a href="../../../command_summary/parameter_intro/omaxtime">OMAXTIME</a> | Maximum time after which the partner can no longer be called over this type of network. |
| <a href="../../../command_summary/parameter_intro/omintime">OMINTIME</a> | Minimum time before which the partner cannot be called over this type of network.<br/> OMINTIME, OMAXTIME therefore represent the permitted time slot for calling this partner over this type of network. |
| <a href="../../../command_summary/parameter_intro/retrym">RETRYM</a>  | Maximum number of reconnection attempts.<br/> If this parameter equals 0 and if the initial connection fails, no further reconnection attempts are made. |
| <a href="../../../command_summary/parameter_intro/retryn">RETRYN</a>  | Corresponds to the number of reconnection attempts with a time interval of RETRYW between attempts.<br/> When RETRYN attempts have been made without success, Transfer CFT divides RETRYN by two and multiplies RETRYW by two and then begins the sequence again up to the total number of times specified (RETRYM). |
| <a href="../../../command_summary/parameter_intro/retryw">RETRYW</a> | Time interval between reconnection attempts (expressed in minutes).<br/> Example:<br/> RETRYW = 01,<br /> RETRYN = 08,<br /> RETRYM = 20<br/> means:<br/> • eight retries at one-minute intervals,<br/> • four retries at two-minute intervals,<br/> • two retries at four-minute intervals,<br/> • one retry after an eight-minute interval,<br/> • five retries at sixteen-minute intervals. |


<span id="CFTXXX_CFTTCP_cmd"></span>

## CFTTCP

****z/OS, IBM i , UNIX, Windows****

The CFTTCP command defines the network parameters associated with a
partner for a TCP/IP connection.

Each partner can only have one CFTTCP object.

In DELETE mode, the following parameters must be defined:

- ID parameter
- CLASS parameter
    if its value is not the default value 1

Only the parameters specific to the CFTTCP object are described. The
complete list of the permitted parameters is nevertheless given below.

<span id="TCP/IP"></span>Command syntax: [CFTTCP](../../../command_summary#CFTTCP)

Use this command to define the network parameters associated
with a partner for a TCP/IP connection.


| Parameter  | Description  |
| --- | --- |
| <a href="../../../command_summary/parameter_intro/host">HOST</a> | HOST = (string64, string64, …) |
| <a href="../../../command_summary/parameter_intro/verify">VERIFY</a> | Option to verify the IIP address (HOST) on an incoming connection request (the first 'n' digits of the caller number are checked).<br/> If VERIFY = 0, no verification is performed.<br/> <br/> The correspondent IP address or the list of correspondent IP addresses with which the user wants to start a session. The maximum number of addresses for this list is 4.<br/> This address (expressed in the form of a character string) may be defined:<br/> • either with the real IP address in the "dot notation" (for example: 192.9.200.10)<br/> • or with the logical name HOSTNAME associated with the real IP address and configured in the corresponding "database" file (HOST), supplied with any TCP/IP package |
| <a href="../../../command_summary/parameter_intro/class">CLASS</a>  | Class of the TCP local resource(s) used to establish the connection with the partner.<br/> This class value is defined in the CFTNET command corresponding to the network access method used to communicate with the partner.<br/> This parameter is used for an outgoing connection request, to select this CFTTCP using the protocol imposed by CFTPART (this mechanism allowing several CFTTCP commands to be associated with a CFTPART command).<br/> This parameter gives rise to a simple verification for an incoming connection request. |
| <a href="">IMINTIME</a>  | The minimum time of the authorized time slot for calls coming over this type of network, or with the partner if defined in CFTPART, before which the partner cannot be called. |
| <a href="../../../command_summary/parameter_intro/imaxtime">IMAXTIME</a> | The maximum time of the authorized time slot for calls coming over this type of network, or with a partner if defined in CFTPART, after which the partner can no longer be called. |
| <a href="../../../command_summary/parameter_intro/omintime">OMINTIME</a> | The authorized time slot for calls coming over this type of network. |
| <a href="../../../command_summary/parameter_intro/omaxtime">OMAXTIME</a> | The maximum time of the authorized time slot for calls over the network type or by the partner if defined, after which the partner can no longer make a call over this type of network. |
| <a href="../../../command_summary/parameter_intro/retrym">RETRYM</a>  | Use this field to specify the number of reconnection attempts to make with a time interval |
| <a href="../../../command_summary/parameter_intro/cnxinout">CNXINOUT</a> | Maximum number of communication sessions. |
| <a href="../../../command_summary/parameter_intro/cnxin">CNXIN</a>  | Maximum number of sessions for input connections. |
| <a href="../../../command_summary/parameter_intro/cnxout">CNXOUT</a>  | Maximum number of sessions for output connections. |
| <a href="../../../command_summary/parameter_intro/retryw">RETRYW</a>  | The time interval (expressed in minutes) between reconnection attempts. |
| <a href="../../../command_summary/parameter_intro/retryn">RETRYN</a>  | Use this field to specify the number of reconnection attempts to make with a time interval of retryw between attempts. |


****Example****

```
CFTTCP   MODE     = CREATE, 
/\* TCP access point  \*/
 ID   = PARIS5,   /\* to the
PARIS5 partner     \*/
  HOST   = SUN3,
  RETRYM  = 6,   /\* reconnection
attempt    \*/
  RETRYN   = 4,          /\*
parameters     \*/
  RETRYW   = 2
```

The PARIS5 partner has a TCP/IP address corresponding to
the mnemonic SUN3.

The intervals between connection attempts, or retries,
are calculated by the following algorithm:

- 4 retries
    at 2-minute intervals
- 2 retries
    at 4-minute intervals
