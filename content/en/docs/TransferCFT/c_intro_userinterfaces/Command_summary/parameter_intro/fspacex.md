{
    "title": "fspacex",
    "linkTitle": "fspacex",
    "weight": "1360"
}<span id="fspacex"></span>

### fspacex

#### CFTFILE

****[FSPACEX =
{see table below&#124; n}]****

****{0..65536}****

****Depending on TYPE/OS****

Secondary allocation of the file to be created expressed in Kbytes
(1024).

The following table indicates for each system the default value
according to the type of file to be created. If the indicated default
value of the secondary allocation of the file to be created is "no",
the FSPACEX does not need to be defined.<span id="FSPACEX_Table"></span>

QQQ_QQQ_QQQ removed top row fspacex


| OS  | PARM  | PART  | CAT  | COM  | LOG  | ACCNT  |
| --- | --- | --- | --- | --- | --- | --- |
| z/OS (MVS) | 50  | 100  | 0  | 0  | 50  | 50  |
| OS400  | 0  | 0  | 0  | 0  | 0  | 0  |
| UNIX  | no  | no  | no  | no  | no  | no  |
| VMS  | no  | no  | no  | no  | no  | no  |
| Windows | no  | no  | no  | no  | no  | no  |


[Return to Command index](../../)
