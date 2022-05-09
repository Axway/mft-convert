{
    "title": "CFTUTIL commands for z/OS",
    "linkTitle": "CFTUTIL commands for z/OS",
    "weight": "350"
}This section lists the Transfer CFT z/OS CFTUTIL commands, and additional object and class information for:

- Users with all rights

<!-- -->

- Users with all rights except for the $CFTOPER class

User with all rights
--------------------


| Command  | Object  | Class  | VARS  | UserID  | Actions  | File/ACC  | Notes  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ABOUT |   |   |   |   |   |   | No control |
| PURGE |   |   |   |   |   | COM(UPDATE) | No control |
| COPYFILE |   |   |   |   |   |   | No control |
| CFTFILE |   |   |   |  <br/>  | Cr/De | PART(ALTER)<br/> PARM(ALTER)<br/> CATLG(ALTER)<br/> LOG(ALTER)<br/> COM(ALTER) | No control<br/>  |
| CFTPARM | CFTPARM | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTCOM | CFTCOM | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTCAT | CFTCAT | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTLOG | CFTLOG | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTACCNT | CFTACCNT | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTAPPL | CFTAPPL | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTSEND | CFTSEND | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTSEND IMPL=YES | CFTSENDI | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTRECV | CFTRECV | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTEXIT | CFTEXIT | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTAUTH | CFTAUTH | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTIDF | CFTIDF | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTXLATE | CFTXLATE | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTNET | CFTNET | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTPROT | CFTPROT | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTETB | CFTETB | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTPART | CFTPART | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PART(UPDATE) |   |
| CFTDEST | CFTDEST | parmcls | &amp;ID | Cmduse | Cr/De/Mo | PART(UPDATE) |   |
| CFTTCP | CFTTCP | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PART(UPDATE) |   |
|   |   |   |   |   |   |   |   |
| UCONFGET | UCONF |   | &amp;ID | Cmduser | Read | UCONF(READ) |   |
| UCONFSUBST | UCONF |   | &amp;ID | Cmduser | Read | UCONF(READ) |   |
| LISTCUCONF | UCONF |   | &amp;ID | Cmduser | Read | UCONF(UPDATE) |   |
| UCONFSET | UCONF |   | &amp;ID | Cmduser | Modify | UCONF(UPDATE) |   |
| UCONFUNSET | UCONF |   | &amp;ID | Cmduser | Delete | UCONF(UPDATE) |   |
|   |   |   |   |   |   |   |   |
| CFTEXT | ALL_PARM | opercls | &amp;FNAME | Cmduser | Read | PARM(READ) |   |
| - &quot; -  | ALL_PART | opercls | &amp;FNAME | Cmduser | Read | PART(READ) |   |
| LISTCAT | ALL_CAT | opercls | &amp;FNAME | Cmduser | Read | CATLG(READ) |   |
| LISTPARM | ALL_PARM | opercls | &amp;FNAME | Cmduser | Read | PARM(READ) |   |
| LISTPART | ALL_PART | opercls | &amp;FNAME | Cmduser | Read | PART(READ) |   |
| LISTCOM | ALL_COM | opercls | &amp;FNAME | Cmduser | Read | COM(READ) |   |
|   |   |   |   |   |   |   |   |
| ACT | ACT | cmdecls |   | Cmduser | Create | PART(UPDATE) |   |
| - &quot; -  | ALL_PART | opercls | &amp;FNAME | Cmduser | Read |   |   |
| - &quot; -  | CFTPART | parmcls | &amp;ID | Cmduser | Control |   |   |
| INACT | INACT | cmdercls |   | Cmduser | Create | PART(UPDATE) |   |
| - &quot; -  | ALL_PART | opercls | &amp;FNAME | Cmduser | Read |   |   |
| - &quot; -  | CFTPART | parmcls | &amp;ID | Cmduser | Control |   |   |


Cmduser = System user (TSO) of the user submitting the command.

User with all rights except the $CFTOPER class
----------------------------------------------


| Command  | Object  | Class  | VARS  | UserID  | Actions  | File/ACC  | Notes  |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ABOUT |   |   |   |   |   |   | No control |
| PURGE |   |   |   |   |   | COM(UPDATE) | No control |
| COPYFILE |   |   |   |   |   |   | No control<br/>  |
| CFTFILE |   |   |   |   | Cr/De<br/>  | PART(ALTER)<br/> PARM(ALTER)<br/> CATLG(ALTER)<br/> LOG(ALTER)<br/> COM(ALTER) | No control<br/>  |
| CFTPARM | CFTPARM | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTCOM | CFTCOM | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTCAT | CFTCAT | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE |   |
| CFTLOG | CFTLOG | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTACCNT | CFTACCNT | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTAPPL | CFTAPPL | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTSEND | CFTSEND | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTSEND IMPL=YES | CFTSENDI | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTRECV | CFTRECV | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTEXIT | CFTEXIT | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTAUTH | CFTAUTH | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTIDF | CFTIDF | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTXLATE | CFTXLATE | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTNET | CFTNET | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTPROT | CFTPROT | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTETB | CFTETB | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PARM(UPDATE) |   |
| CFTPART | CFTPART | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PART(UPDATE) |   |
| CFTDEST | CFTDEST | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PART(UPDATE) |   |
| CFTTCP | CFTTCP | parmcls | &amp;ID | Cmduser | Cr/De/Mo | PART(UPDATE) |   |
| CFTEXT | CFTxxx* | parmcls | &amp;ID | Cmduser | Read | PARM(READ) |   |
| - &quot; -  | CFTyyy* | parmcls | &amp;ID | Cmduser | Read | PART(READ) |   |
| LISTPARM | CFTxxx* | parmcls | &amp;ID | Cmduser | Read | PARM(READ) |   |
| LISTPART | CFTyyy* | parmcls | &amp;ID | Cmduser | Read | PART(READ) |   |
| LISTCAT | APPL | applcls | &amp;ID | Cmduser | Read | CATLG(READ) |   |
| LISTCOM |   |   |   |   |   |   | Access denied |
| ACT | ACT | cmdecls |   | Cmduser | Create | PART(UPDATE) |   |
| - &quot; -  | CFTPART | parmcls | &amp;ID | Cmduser | Read | - &quot; -  |   |
| - &quot; -  | CFTPART | parmcls | &amp;ID | Cmduser | Control | - &quot; -  |   |
| INACT | INACT | cmdecls |   | Cmduser | Create | PART(UPDATE) |   |
| - &quot; -  | CFTPART | parmcls | &amp;ID | Cmduser | Read | - &quot; -  |   |
| - &quot; -  | CFTPART | parmcls | &amp;ID | Cmduser | Control | - &quot; -  |   |


CFTxxx\* = PARM file configuration commands.

CFTyyy\* = PART file configuration commands.
