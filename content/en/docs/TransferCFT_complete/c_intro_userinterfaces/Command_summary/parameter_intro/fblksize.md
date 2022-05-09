{
    "title": "fblksize",
    "linkTitle": "fblksize",
    "weight": "1050"
}<span id="fblksize"></span>

### fblksize

<span id="fblksize_CFTFILE"></span>

#### CFTFILE

**[FBLKSIZE = {<span class="underline">see
table below</span> &#124; n}]  **{0...32768}

According to TYPE/**OS**

Defines the block size, in bytes, of the file to be created.

The table below indicates, for each system, the default value supported
according to the type of file to be created.When the default
value of the block size of the file to be created is "no" in the following table, the
FBLKSIZE parameter does not need to be defined.

QQQ_QQQ_QQQ removed fblksize from top row


| OS  | PARM  | PART  | CAT  | COM  | LOG  | ACCNT  |
| --- | --- | --- | --- | --- | --- | --- |
|  z/OS (MVS) | no  | no  | no  | no  | 1028 | 482  |
| IBM i (OS400)  | 0  | 0  | 0  | 0  | 0  | 0  |
| UNIX  | no  | no  | no  | no  | no  | no  |
| VMS  | no  | no  | no  | no  | no  | no  |
| Windows | no  | no  | no  | no  | no  | no  |


#### CFTRECV, RECV

**[FBLKSIZE = n ]              **{0..62563}

This parameter, in bytes, controls the "blocking factor" of
the receiver file records: according to the system, it defines the disk
block size and/or the file input/output buffer size.


| System  | FBLKSIZE used  |
| --- | --- |
| MVS (z/OS) | YES  |
| OS400 (IBM i) | NO  |
| UNIX  | NO  |
| VMS  | NO  |
| Windows | NO  |



| **z/OS (MVS)**  | For protocols other than PeSIT, CFT profile, if this parameter is not defined, its value is set as follows: <br/> • For fixed format files: this value equals the largest multiple of FLRECL which is less than the constant (related to the track length) defined on installation (default value: 19069), or FLRECL if FLRECL is greater than this constant<br/> • For variable format files: this value equals the constant (related to the track length) defined on installation of the {{< TransferCFT/axwayvariablesComponentShortName  >}} in a z/OS environment (default value: 19065), or to FLRECL + 4 if FLRECL is greater than this constant<br/> • For undefined format files: this value is equal to 32760 |
| --- | --- |


<span id="fblksize_CFTSEND"></span>

#### CFTSEND, SEND

****[FBULKSIZ = n]   {
0...65536}****

Block size of file to be sent.

Typically you do not need to define this parameter as {{< TransferCFT/axwayvariablesComponentShortName  >}} is
able to locate the value for the file to be sent. This real value is then
taken into account when activating the transfer.


| System  | FBLKSIZE used  |
| --- | --- |
| MVS (z/OS) | YES  |
| OS400 (IBM i) | NO  |
| UNIX  | NO  |
| OpenVMS  | NO  |
| Windows  | NO  |


[Return to Command index](../../)
