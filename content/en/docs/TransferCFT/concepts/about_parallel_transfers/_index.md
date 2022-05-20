---
title: "Configure simultaneous transfers"
linkTitle: "Configure simultaneous transfers"
weight: 210
---The maximum number of simultaneous transfers in Transfer CFT is managed by a combination of global parameters, partner settings, and the license key.

The objective of this section is to help you understand the global and partner parameters, and provide configuration recommendations in order to assist you in optimizing your file transfer volume.

Contents include:

* [Global](#Global) and [partner](#Partner) settings as they relate to the Transfer CFT configuration (this page)
* [Client and server recommendations](maxtrans_use_cases) that demonstrate settings for a high volume of transfers
* [Connection / maximum transfer](connection_maxtrans_troubleshoot) scenarios with output
* [Session related scenarios](session_troubleshooting) with output
* [Multi-node recommendations](multi_node_simultaneous_transfers)
* [FAQ and troubleshooting](faq)

> **Note**
>
> Some parameters benefit from further tuning if using a multi node architecture. See also Multi-node partner configuration.

<span id="Global"></span>

## Global settings

The global settings define the connection and session parameters on the local Transfer CFT side.

### Connection

A connection is a network link established between two machines (peers). The maximum number of simultaneous connections in Transfer CFT is defined by the MAXCNX parameter.

#### MAXCNX

**CFTNET object**

The MAXCNX parameter determines the maximum number of connections for a network resource. The maximum number of connections handled by Transfer CFT is the sum of all of the MAXCNX parameter values for the CFTNET object.

**Example**

If there are two CFTNET objects defined as follows, the maximum number of simultaneous connections for this Transfer CFT instance is 30, 10 connections for NET0 and 20 for NET1.

```
CFTNET id=NET0, maxcnx=10CFTNET id=NET1, maxcnx=20
```

### Session

A session is a conversation between two partners. The session occurs at the protocol level and executes over a network connection. A session is established at a certain point in time and then is later disconnected.

An established session is either active or inactive. An active session means that a transfer is using it. An inactive session remains established during a wait timeout; this is referred to as *session persistence*. After the timed-out session is closed, its associated connection is automatically closed.

You can also set the maximum number of simultaneous sessions using the UCONF parameter `cft.server.max_session`. The default value is 0  and the maximum number of supported simultaneous transfer is 2000 (2 x 1000).

> **Note**
>
> We recommend using the default value for cft.server.max_session.

#### DISCTD/DISCTS

**CFTPROT object**

In Transfer CFT, the timeout (session persistence) is defined by DISCTS in server mode, and DISCTD in client mode. Normally the session is first closed by the requester, which implies a best practice of the DISCTD being less than the server DISCTS.

**Client configuration**

```
CFTPROT id=PESIT, DISCTS=8, DISCTD= 8
```

**Server configuration**

```
CFTPROT id=PESIT, DISCTS= 10
, DISCTD=7
```

> **Note**
>
> We recommend setting these values to 60 seconds or less.

### Transfer execution

A transfer executes over a session. Transfer CFT attributes a new transfer to an inactive but established session, flagging the session as active. If there is no inactive session, a new session is created. Once the transfer is finished the session is set to inactive.

During a session's life, multiple transfers can be executed sequentially on a same session. We call this session reuse or session persistence.

The MAXTRANS parameter defines the maximum number of simultaneous transfers. The value can be different than the license key limit as described below. However, if the set value is greater than the license key limit, the MAXTRANS value is automatically set to the license key limit when starting Transfer CFT.

#### MAXTRANS

**CFTPARM object**

```
CFTPARM ID=IDPARM0,MAXTRANS=n, ...
```

In general, you should base your MAXTRANS on the transfer peak in your daily activity.

Related UCONF values:

* cft.run.maxtrans: The current MAXTRANS value used by Transfer CFT (information only, you cannot set this value)
* cft.server.maxtrans: This parameter overrides the CFTPARM MAXTRANS parameter when it is not set to 0. If the `cft.server.maxtrans `parameter is set to 0, the MAXTRANS value is used.

<span id="License"></span>

### License key

Your license key is another factor that affects the maximum value that you can use for MAXTRANS.

Use the CFTUTIL ABOUT command to check for the maximum number of allowed transfers as determined by your key. The value "64" is the authorized limit in the following example.

```
CFTUTIL about
Key information :
\* idparm = IDPARM0
\* key = XXXX
\*
\* type = DATE
\* expire = 2015/11/14
\* sysname = linux-x86-64
\* Nb Transfers = 64
\* Nb CPU = 4
\* Nb Partners = Max
```

To view the number of active transfers in Transfer CFT, check this message which displays when starting Transfer CFT:

`CFTI18I+On 999 authorized simultaneous transfer(s), 100 is(are) active`

To increase the license key limit contact your Axway sales representative, or visit [away.com](https://www.axway.com/) to find your regional representative.

#### MAXTASK

**CFTPARM object**

This parameter controls the number of CFTTFIL processes that can run. Since more processes use more machine memory, remember when increasing MAXTASK to take available resources into account. See also [MAXTASK values.](../../c_intro_userinterfaces/command_summary/parameter_intro/maxtask)

```
CFTPARM ID=IDPARM0,MAXTASK=n, ...
```

> **Note**
>
> There is a one to one relationship between a transfer and its task. That is, the task cannot be shared, so it is recommended to set the MAXTASK to less than or equal to MAXTRANS.

#### TRANTASK

**CFTPARM object**

This parameter defines the number of transfers managed per task, and operates similar to load balancing between tasks. See also [TRANTASK values.](../../c_intro_userinterfaces/command_summary/parameter_intro/trantask)

The MAXTASK value multiplied by the TRANTASK value should be less than or equal to MAXTRANS.

```
CFTPARM ID=IDPARM0,TRANTASK=n, ...
```

##### Example

The following parameter values can create a maximum of 2 file access tasks. Transfers 1 through 4 are assigned to the first task, and the next transfer triggers a new file access task that handles transfers 5-8 (as there are four transfers for each task). Any additional transfers are balanced between the two existing tasks. (This means that all new concurrent transfers are put on hold until resources become available.)

```
MAXTRANS = 14,MAXTASK = 2,
TRANTASK = 4,
```

#### WSCAN scheduling parameter

**CFTCAT object**

This parameter defines the frequency, in minutes, with which Transfer CFT scans the catalog file when restarting a transfer. That is, WSCAN reschedules transfers that have a remote MAXTRANS or local MAXCNX diagnostic. The default value is 5, but we recommend setting this value to 1.

> **Note**
>
> The WSCAN scheduling retries continues indefinitely until the transfer can be executed.

<span id="Partner"></span>

## Partner settings

This section provides information and examples concerning the session parameters at partner level.

### Session parameters

When defining flow exchanges with a partner, it is recommended to check the partner's number of incoming, outgoing, or incoming/outgoing connections, and keep these values in mind during configuration. That means that for each Transfer CFT partner, you should specify in the CFTTCP command the parameters CNXIN, CNXOUT, and CNXINOUT parameters relative to the CFTPART (having the same ID).

#### CNXIN

**CFTTCP object**

This parameter defines the maximum number of sessions for incoming connections.

The value should be less than or equal to the MAXCNX and MAXTRANS values.

```
CFTTCP ID =<partner1_id>, HOST=<partner1_URL>, CNXIN=n, ...
```

#### CNXOUT

**CFTTCP object**

This parameter defines the maximum number of sessions for outgoing connections.

```
CFTTCP ID =<partner1_id>, HOST=<partner1_URL>, CNXOUT=n, ...
```

#### CNXINOUT

**CFTTCP object**

This refers to the maximum number of communication sessions (less than or equal to the MAXCNX and MAXTRANS values). This is the total number of both incoming and outgoing sessions. In the following example the number of sessions that you can open at the same time is 4; it is not the sum of CNXIN and CNXOUT but rather an additional limitation.

```
CFTTCP ID =<partner1_id>, HOST=<partner1_URL>, CNXIN=2, CNXOUT=3, CNXINOUT=4, ...
```

### Recommendations

When using a stand alone Transfer CFT to another stand alone Transfer CFT (or other PeSIT application), the recommendations for a heavily loaded configuration should remain symmetrical.


| Stand alone  | Stand alone  |
| --- | --- |
| CNXOUT=(Partner's CNXIN) | CNXIN = (Partner’s CNXOUT)  |
| CNXIN=(Partner’s CNXOUT) | CNXOUT=(Partner’s CNXIN)  |
| CNXINOUT=CNXIN+CNXOUT | CNXINOUT=CNXIN+CNXOUT  |


#### RETRYW scheduling parameter

**CFTTCP object**

This parameter defines the retry period following a network interruption. This is one of 3 parameters that control the retry policy for rescheduling following a network connection interruption. The RETRYW value is an approximate time in order to avoid all blocked transfers being rescheduled at the same time overloading the system.

> **Note**
>
> The RETRYW scheduling retries is limited by the RETRYM number.

#### RETRYM

Use this parameter to specify the maximum number of reconnection attempts. If you enter 0 and if the initial connection fails, no further reconnection
attempts are made.

#### RETRYN

Use this parameter to specify the number of reconnection attempts to make
with a time interval of retryw
between attempts. When retryn attempts have been
made without success, {{< TransferCFT/axwayvariablesComponentShortName  >}} divides retryn
by two and multiplies retryw by
two and then begins the sequence again up to the total number of times
specified retrym.

****Related topics****

* [Client and server recommendations](maxtrans_use_cases)
* [Scenarios and outputs](session_troubleshooting)
