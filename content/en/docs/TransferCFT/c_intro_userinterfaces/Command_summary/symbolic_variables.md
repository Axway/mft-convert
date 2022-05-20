---
title: "Symbolic variables"
linkTitle: "Symbolic variables"
weight: 150
---<span id="About_symbolic_variables"></span>A symbolic variable represents
a data item whose value is not known at the time {{< TransferCFT/axwayvariablesComponentShortName  >}}
parameters are set, but only at the time of execution. Additionally, the symbolic variable is prefaced by a special character, which is the ‘&’ character in this document. However, you should refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Installation Guide* that corresponds to
your OS, in order to determine the special character &lt;char_symb>
used on your system.

For example, prior to a transfer the transfer identifier IDT
is not known, so the symbolic variable &IDT
can be used to reference the value. For more information see [Symbolic variable syntax](#Symbolic_variable_syntax).

Symbolic variables may be used in defining the values of certain parameters
of the commands and procedure files associated with transfers. This avoids
having to repeat the same basic descriptions a considerable number of
times: a single CFTSEND command can hence be applied to several files
by using the symbolic variables in the FNAME parameter. This also makes
it possible to only have to describe one transfer-related procedure applicable
to several transfers. The real value of the parameter is substituted for
the symbolic variable, at the time the {{< TransferCFT/axwayvariablesComponentShortName  >}} command or the procedure
is executed.

> **Note**
>
> Tip  
> You can find a comprehensive list of file naming examples in the Send command page.

<span id="Symbolic_variable_syntax"></span>

### Symbolic variable syntax

The symbolic variable syntax is as follows:

* A special character
    in the first position (leftmost)
* Optionally followed
    by the character(s):


| Character  | Indicates...  |
| --- | --- |
| +  | That the variable toggles to upper case  |
| -  | That the variable toggles to lower case  |
| :  | That the right padding of the variable is suppressed  |
| &lt;  | The left justification of the variable (default value)  |
| &gt;  | The right justification of the variable  |
| %  | Indicates use of the [separator syntax](#Separate)  |
|   | These characters can be used in combination, such as ****+:**** or ****&gt;+:****. <br/> See the [Example using optional characters](#Examples) |


* Optionally followed
    by a character string to be used as a prefix (-string_prefix)
    and/or a character string to be used as a suffix (+string_suffix)
    and/or a character string to be used as a substitute/alternate value (=string_alternate)
* Optionally followed
    by a numeric formula with a syntax ‘n’ or ‘p.’ or ‘p.n’
* And then a character
    string representing the identifier of the variable to be substituted

The identifiers, recognized by {{< TransferCFT/axwayvariablesComponentShortName  >}}, which can be used in the
syntax of a symbolic variable are indicated in the *[List of symbolic variables](#List_of_symbolic_variables)*.
In this section, the ‘VAR’ notation is used to generically designate such
an identifier.

The substitution mechanism, which is used to assign a value to the symbolic
variable using the effective value of the ‘VAR’ identifier,
depends on the numeric formula indicated after the & character.

According to the syntax used:

* &VAR syntax
    (numeric formula not used)

    The value substituted for the symbolic variable equals
    the effective value of the ‘VAR’ identifier, truncated of the blank characters
    on the right.

* &+VAR syntax:

    The value substituted for the symbolic variable is
    shifted to upper case.

* &nVAR syntax:
    *   The value substituted for the symbolic variable corresponds
        to a fixed length string of n characters, equal to the string corresponding
        to the effective value of ‘VAR’, truncated as required, or with blank
        characters added to the right.
    *   The first character in this string corresponds to
        the first character, from the left, of the string corresponding to the
        value of the ‘VAR’ identifier.

* &p.VAR syntax:

    The value substituted for the symbolic variable corresponds
    to the character substring formed from the effective value of ‘VAR’ such
    that:

    *   The first character
        of this substring corresponds to the p th character (from the left) of
        the string corresponding to the value of ‘VAR’
    *   The length
        corresponds exactly to the length of the effective value of ‘VAR’, the
        character in the i position being equal to the character in the p+i position
        of the value of ‘VAR’. If the length of the value of ‘VAR’ is m characters,
        the length of the substring will consequently be m-p+1 characters

<!-- -->

* &p.nVAR syntax:
    *   The value substituted for the symbolic variable corresponds
        to the character sub-string formed from the effective value of ‘VAR’ such
        that:

        The first character
        of this substring corresponds to the p th character (from the left) of
        the string corresponding to the value of ‘VAR’

        This substring
        is n characters long (fixed length), the character in the i position being
        equal to the character in the p+i position of the value of ‘VAR’. If the
        length of the effective value of VAR is less than p+n characters, the
        characters completing the substring are blank characters

&(-string_prefix)
(+string_suffix) (=string_alternate)p.nVAR syntax:

After the &p.nVAR symbolic variable has been substituted:

* The &lt;str_prefix>character
    string is added before the variable, if the substituted variable is not
    empty
* The &lt;str_suffix>
    character string is added after the variable, if the substituted variable
    is not empty
* The &lt;str_alternate>
    character string is used if the substituted variable is empty
* The character
    strings &lt;str_prefix>, &lt;str_suffix> and &lt;str_alternate>
    can contain a symbolic variable

<span id="Separate"></span>

#### Separate fields in a symbolic variable

You can use the field extraction syntax as follows:

`&%<separator>[<start_field>[.[<end_field>]]VARIABLE`

1. If` start_field` is omitted, the default value is 1.
1. If `end_field` is omitted, the default value is the last field in the variable.
1. If there are 2 consecutive separators, the extracted field between the 2 separators is empty.
1. If there is only one number after the separator indicating the value placement, this returns just that token value.

For the following example, see the corresponding syntax:

`&<VARIABLE>=S052368_Z123_HZUI34_92___TYU`

1. `&%_.2<VARIABLE>`: separator=_, start_field=1 , end_field=2 (this returns the value `S052368_Z123`)
1. `&%_3.<VARIABLE>`: separator=_ , start_field=3, end_field=last_field (this returns the value `HZUI34_92___TYU`)
1. `&%_5<VARIABLE>`: separator=_, start_field=5, end_field=5 (this returns ' ')
1. `&%_4<VARIABLE>`: separator=_, start_field=4, end_field=4 (this returns `92`)

You can combine field extraction with the other filtering methods. The full syntax is:

`&[<&#124;>][+&#124;-][:][(-string_prefix)(+string_suffix)(=string_alternate)][<position>.][<length>][%<separator>][<start_field>][.[<end_field>]]<VARIABLE>`

#### Example symbolic variable usage

If PART=PART1 and IDF=TEST, then FNAME=&IDF&PART.tst is substituted
by FNAME=TESTPART1.tst

In formats in which "n" or "p" are used, the following
remarks are valid:

* A value of "n"
    equal to 0 is non significant
* A value of "p"
    equal to 0 is non significant
* A position "p"
    greater than the length of the "effective" value of ‘VAR’ gives,
    after substitution, a string of zero length

This allows identifiers of length less than p to be selected, for example.

> **Note**
>
> &.VAR is substituted as &VAR. The &lt;char_symb> concatenated
> with a point is substituted as &lt;char_symb>. The rule applies even
> if VAR is not an identifier known to Transfer CFT. For example, for the
> formats &.VAR (&0.VAR, &0.0VAR or &.0VAR), the value substituted
> is not the effective value of the identifier ‘VAR’ but the literal string
> &VAR.

#### Example of possible formats

Given the generic identifier ‘VAR’ of effective value, the character
string: ‘F2345’.

Depending on the format of the associated symbolic variable, the substituted
values are:

```
&VAR = ’F2345’
&3VAR = ’F23’
&7VAR = ’F2345 ’
 
&.VAR = ’&VAR’ (not substituted)
&.4VAR = 'F2345'
&1.VAR = ’F2345’
&2.VAR = ’2345’
&5.VAR = ’5’
&6.VAR = ’’
 
&1.1VAR = ’F’
&2.1VAR = ’2’
&5.1VAR = ’5’
&6.1VAR = ’ ’
 
&1.2VAR = ’F2’
&2.3VAR = ’234’
&5.6VAR = ’5 ’ (5 blank characters)
&6.7VAR = ’ ’ (7 blank characters)
```
<span id="Examples"></span>

#### Example using optional characters

Given the generic identifier ‘VAR’ of an effective value, and the character string: ‘SEND'.

Depending on the associated symbolic variable format, the substituted values are:

![](/Images/TransferCFT/chars_symbolic_vars.png)

<span id="Example"></span>

#### Example of rebuilding filenames using symbolic variables

Given the syntax FNAME=&FROOT&(-.)FSUF:

* If &FSUF is empty, FNAME gets the value &FROOT
* If &FSUF is not empty, FNAME gets the value &FROOT.&FSUF

For example, if you have a file readme.txt on two different platforms:

![](/Images/TransferCFT/text_froot.png)

Given the syntax FNAME=&(=DUMMY)PARM,

* If &PARM is empty, FNAME gets the value DUMMY
* If &PARM is not empty, FNAME gets the value &PARM

Given the syntax FNAME=&(-PREF)(+SUF)(=DUMMY)PARM,

* If &PARM is empty, FNAME gets the value DUMMY
* If &PARM is not empty, FNAME gets the value PREF&PARMSUFF

> **Note**
>
> To add a closing parenthesis within the str_prefix, str_suffix and str_alternate, it must be preceded by the character ‘&’:

* Given the syntax FNAME=&(+(1234&))PARM
    *   If &PARM is not empty, FNAME gets the value &PARM(1234)

#### Example of breaking down files names

Break down the name of the file to send using the symbolic variables &FUNIT , &FUNITC , &FPATH , &FROOT , &FSUF (applied to

the SFNAME).

Break down the name of the received file using the symbolic variables &UNIT, &UNITC, &PATH , &ROOT, &SUF (applied to the FNAME).

<span id="List_of_symbolic_variables"></span>

### List of symbolic variables

The table below indicates all the symbolic variables available, using
the syntax &VAR. The substituted value, corresponding to the effective
value of the identifier ‘VAR’ (truncated of the blank characters on the
right), is also indicated.

> **Note**
>
> The other syntax shown above may also be used, for each of the identifiers
> listed.

QQQ_QQQ Table CHECK CHECK big split!!!

#### PARTNERS domain

<table>
   <thead>
      <tr>
<th >Domain         </th>
<th >Symbolic variable         </th>
<th  >Maximum length         </th>
<th >Corresponding substituted
value         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td rowspan="8"  data-valign="top" width="21%">PARTNERS          </td>
         <td  data-valign="top" width="26%"><p>&amp;PART </p>         </td>
         <td  >32         </td>
         <td  data-valign="top"><p>Partner name (ID of CFTPART) </p>         </td>
      </tr>
      <tr>
         <td  data-valign="top" width="26%"><p>&amp;GROUP </p>         </td>
         <td  >32         </td>
         <td  data-valign="top"><p>Group to which the partner belongs </p>         </td>
      </tr>
      <tr>
         <td  data-valign="top" width="26%"><p>&amp;SPART </p>         </td>
         <td  >32         </td>
         <td  data-valign="top"><p>Sending partner name </p>         </td>
      </tr>
      <tr>
         <td  data-valign="top" width="26%"><p>&amp;RPART </p>         </td>
         <td  >32         </td>
         <td  data-valign="top"><p>Receiving partner name </p>         </td>
      </tr>
      <tr>
         <td  data-valign="top" width="26%"><p>&amp;IPART </p>         </td>
         <td  >32         </td>
         <td  data-valign="top"><p>Intermediate partner name </p>         </td>
      </tr>
      <tr>
         <td  data-valign="top" width="26%"><p>&amp;NPART </p>         </td>
         <td  >32         </td>
         <td  data-valign="top"><p>Network name of partner sending data (NSPART or NRPART
according to the transfer direction) </p>         </td>
      </tr>
      <tr>
         <td  data-valign="top" width="26%">&amp;NSPART         </td>
         <td  >24         </td>
         <td  data-valign="top">Network identifier by which the
local {{< TransferCFT/axwayvariablesComponentShortName  >}} identifies itself to its partner         </td>
      </tr>
      <tr>
         <td  data-valign="top" width="26%">&amp;NRPART         </td>
         <td  >24         </td>
         <td  data-valign="top"><p>Network identifier by which the
remote partner identifies itself to the local {{< TransferCFT/axwayvariablesComponentLongName  >}}</p>         </td>
      </tr>
   </tbody>
</table>

#### USER domain


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;SUSER  | 32  | Sending user name  |
| &amp;RUSER  | 32  | Receiving user name  |
| &amp;USERID  | 32  | Local user identifier  |
| &amp;GROUPID | 32  | Group identifier linked to the userid |
| &amp;COMMENT  | 160  | Comment indicated in CFTSEND/SEND or CFTRECV/RECV <br/> In listcat content=debug this is attribute is MSG  |
| &amp;NOTIFY  | 8  | User notified on transfer  |
| &amp;SJOBNAME  | 15  | {{< TransferCFT/axwayvariablesComponentShortName  >}} job name, can be used in exec and cronjob procedures  |


#### APPLICATIONS domain


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;SAPPL |  <br/> 8<br/> 48 | Sending application name <br/> PeSIT E<br/> PeSIT E CFT/CFT |
| &amp;RAPPL  |  <br/> 8<br/> 48 | Receiving application name<br/> PeSIT E PeSIT E CFT/CFT |
| &amp;IDA  | 64  | Application identifier  |
| &amp;PARM  | 512  | Parameter  |
| &amp;PI99  | 512  | PI99 contents (PeSIT E)  |


#### TRANSFER domain


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;IDT | 8  | Transfer identifier  |
| &amp;NIDT  | 8  | Protocol transfer identifier  |
| &amp;IDTU  | 8  | Local transfer counter (unique) |
| &amp;PIDTU  | 8  | Parent idtu of the child transfers  |
| &amp;PHASE  | 1  | Processing phases to help manage transfer flows  |
| &amp;PHASESTEP  | 1  | Step in processing phase  |
| &amp;APPSTATE  | 32  | State step for the processing script to restart if relaunched  |
| &amp;NSUB  | 4  | Counter for the submitting of end-of-transfer procedures, error procedures and procedures submitted by SUBMIT.<br /> If 4 characters long, the counter is reset to 1 after 9999  |
| &amp;DIAGI  | 8  | Internal diagnostic code value  |
| &amp;DIAGP  | 64  | Protocol diagnostic code value  |
| &amp;DIAGC  | 254  | Complimentary diagnostic code value  |
| &amp;COMP  | 2  | Compression negotiated for the transfer<br/> Compression negotiated for the transfer<br/> When listcat content=debug this is attribute is FCOMP / COMPNEG |
| &amp;NBT  | 20  | Number of bytes transferred  |
| &amp;PRI  | 3  | {{< TransferCFT/axwayvariablesComponentShortName  >}} priority for the transfer (0 to 255)  |
| &amp;QQ  | 3  | Number of the day in the year associated with the transfer identifier  |
| &amp;SELFNAME  | 512  | Name of the generic transfer selection file  |
| &amp;FCODE  | 1  | Code for the data in a file  |
| &amp;TRTYPE | 8  | Available at the end of transfer to designate FILE, MESSAGE, REPLY, or NACK<br/> When using listcat content=debug the attribute is TYPE |
| &amp;NCODE  | 1  | Code for the data sent over the network  |
| &amp;EXITFREE  | 64  | Free communication area between multiple exits  |
| &amp;XLATE  | 32  | Transcoding table used during transfer  |
| &amp;MODE  | 1  | Server mode = ‘S’ transfer<br/> Requester mode = ‘R’ transfer |


#### FILE domain


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;IDF  | 32  | Model file identifier (logical name)  |
| &amp;FNAME | 512  | Physical file local name  |
| &amp;FKEYLEN  | 5  | Length (received) of the indexed file key at the sender’s site  |
| &amp;FKEYPOS  | 5  | Position (received) of the indexed file key at the sender’s site  |
| &amp;NBR  | 20  | Number of records in the file<br/> For listcat=content debug, this attribute is FRECS |
| &amp;BLKNUM  | 6  | Catalog block number  |
| &amp;XLATE  | 32  | Identifier of the translation table used  |
| &amp;NBC  | 20  | Number of bytes in the transferred file |
| &amp;NIDF  | 512  | Model file network identifier  |
| &amp;FDB  | 64  | Database name  |
| &amp;FCHARSET  | 32  | Local file encoding  |
| &amp;NCHARSET  | 32  | Destination file encoding for network data  |
| &amp;WORKINGDIR  | 512  | Specify a directory other than the default directory  |
| &amp;HOME  | 512  | Keyword that allows different users to work with files placed in their home directory  |
| Receiving  |   |   |
| &amp;NFNAME  | 512  | Physical file network name  |
| &amp;FROOT  | 512  | Root (file name) <br/> Based on the SFNAME (remote sending file) |
| &amp;FSUF  | 512  | File name suffix -<br/> Based on the SFNAME (remote sending file) |
| &amp;FPATH  | 512  | Prefix (file path) -<br/> Based on the SFNAME (remote sending file) |
| &amp;FUNITC  | 512  | Physical file unit (z/OS) -<br/> Based on the SFNAME (remote sending file) |
| &amp;FUNIT<br/>  | 512  | Physical file volume -<br/> Based on the SFNAME (remote sending file) |
| &amp;UNIT  | 512  | Physical file volume name for received file |
| &amp;UNITC  | 512  | Physical file unit class for received file (z/OS)  |
| &amp;PATH | 512  | Local file path of the received file |
| &amp;ROOT | 512  | Local file root for the received file |
| &amp;SUF | 512  | Local file suffix for the received file |
| Sending  |   |   |
| &amp;SFNAME  | 512  | Name of file to send  |
| &amp;FUNIT<br/>  | 512  | Physical file volume name for sending file |
| &amp;FUNITC  | 512  | Physical file unit for sending file (z/OS)  |
| &amp;FPATH  | 512  | Prefix (file path) of the sending file |
| &amp;FROOT  | 512  | Root (actual file name) of the sending file |
| &amp;FSUF  | 512  | Suffix associated with file name of the sending file |


#### MESSAGES domain


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;IDM | 32  | Message identifier  |
| &amp;MSG  |  <br/> 80<br/> 512 | Message text <br/> PeSIT D CFT<br/> PeSIT E |


#### DATE and TIME associated with a FILE 

> **Note**
>
> Note: The symbolic variable formats concerning dates and times are:

* Time: HHMMSSCC
     
* Complete date: YYYYMMDD
* Year: YY
* Month: MM
* Day: DD


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;FDATE  | 8  | Date associated with the file  |
| &amp;FTIME  | 8  | Time associated with the file  |
| &amp;FYEAR  | 2  | Year associated with the file  |
| &amp;FMONTH  | 2  | Month associated with the file  |
| &amp;FDAY  | 2  | Day associated with the file  |


#### DATE and TIME associated with a CATALOG 


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;CDATE  | 8  | Catalog entry date  |
| &amp;CTIME  | 8  | Catalog entry time  |
| &amp;CYEAR  | 2  | Catalog entry year  |
| &amp;CMONTH  | 2  | Catalog entry month  |
| &amp;CDAY  | 2  | Catalog entry day  |


#### DATE and TIME associated with a TRANSFER 


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;BDATE  | 8  | Transfer start date <br/> When listcat content=debug the start date is<br/> DATEB instead of BDATE |
| &amp;BTIME  | 8  | Transfer start time  |
| &amp;BYEAR  | 2  | Start year for the transfer |
| &amp;BMONTH  | 2  | Start month for the transfer |
| &amp;BDAY  | 2  | Transfer start day  |
| &amp;EDATE  | 8  | Transfer end date<br/> When listcat content=debug the end date is<br/> DATEE instead of EDATE |
| &amp;ETIME  | 8  | Transfer end time f |
| &amp;EYEAR  | 2  | Transfer end year |
| &amp;EMONTH  | 2  | Transfer end month |
| &amp;EDAY  | 2  | Transfer end day |
| &amp;TT  | 10  | Transmission duration in seconds (TIMES attribute in the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog) |


#### CONTROL OUTPUT  domain


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;FLOG  | 512  | Name of last log file used by {{< TransferCFT/axwayvariablesComponentShortName  >}}  |
| &amp;FACCNT  | 512  | Name of last statistics file used by {{< TransferCFT/axwayvariablesComponentShortName  >}}  |
| &amp;FCAT | 512  | Name of catalog used by {{< TransferCFT/axwayvariablesComponentShortName  >}} |


#### TRACKING domain


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;XFRCYCID  | 250  | Processing cycle identifier (set of tracked instances that concern a single transfer)  |
| &amp;XFROBJID  | 32  | Tracked object name  |


#### SSL domain

These variables are linked to SSL use.


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;SSL | 1  | Indicates if the session the transfer was carried out on was secured (‘1’) or not (‘0’)  |
| &amp;SSLMODE  | 1  | SSL session mode on which the transfer was carried out. (C: Client / S: Server)  |
| &amp;SSLAUTH  | 1  | Authentication rule<br /> (A: Anonymous /S: Server / B: Both)  |
| &amp;SSLCIPH  | 2  | SSL cipher suite  |
| &amp;SSLPROF  | 32  | SSL profile identifier  |
| &amp;SSLPARM  | 64  | SSL user parameter Parm parameter of the CFTSSL command  |
| &amp;SSLRMCA  | 256  | Certificate identifier of the authority that signed the certificate presented by the remote partner  |
| &amp;SSLUSER  | 256  | Identifier of the user certificate used locally for authentication by the remote partner  |
| &amp;SSLCFNAM  | 64  | Physical name of the file in which the certificate chain presented by the remote partner was recorded <br/> This is the same as the CFTSSL CERFNAME parameter value |


#### SYSTEM  domain


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;SYSDATE  | 8  | System date  |
| &amp;SYSTIME  | 8  | System time  |
| &amp;SYSQQ  | 3  | Number of the day in the year associated with the system date  |
| &amp;SYSDAY | 1  | Day of the week (Sunday = 0, 6 = Saturday) |


#### CAT/ ACCOUNT ENVIRONMENT domain


| Symbolic variable  | Maximum length  | Corresponding substituted value  |
| --- | --- | --- |
| &amp;CFTNAME | 32  | Name of the {{< TransferCFT/axwayvariablesComponentShortName  >}} (CFTPARM PART parameter) |
| &amp;CFTEVENT | 16  | The type of job submitted by {{< TransferCFT/axwayvariablesComponentShortName  >}}, see (2) below |
| &amp;SJOBNAME  | 15  | The {{< TransferCFT/axwayvariablesComponentShortName  >}} jobname, which is the name of the job submitting the cronjob or exec procedure (z/OS)  |
| &amp;CFTVERSION  | 16  | The Transfer CFT version  |
| &amp;CFTSP  | 16  | The latest SP applied to the Transfer CFT  |
| &amp;CFTPATCH  | 16  | The latest patch applied to the Transfer CFT  |
| &amp;CFTTARGET  | 16  | The Transfer CFT platform with additional details required for a support ticket, for example  |
| &amp;CFTHOSTOS  | 64  | The Transfer CFT hostname  |
| &amp;CFTHOSTMACHINE  | 64  | The machine processor name where Transfer CFT is running  |


(2): EXEC in SEND, EXECSF, EXECSM, EXEC in RECV, EXECRF, EXECRM, EXECE, EXECSE, EXECRE, EXECA, EXECSFA, EXECSMA, PREEXEC, EXITEOT, EXECSUB, EXECSUBA, EXECSUBPRE

****Sender****

The symbolic variables are
substituted by the values of the local parameters of the commands.

****Receiver****

The symbolic
variables are substituted:

* By the sender parameter
    values when these values are conveyed by the protocol
* By default, by
    the corresponding local parameter values

<span id="Using_symbolic_variables"></span>

## Using symbolic variables

Symbolic variables can be used:

* To assign a value
    to certain parameters of the parameter setting or transfer commands
* In the processing
    operations defined by the user in the procedures associated with the transfers


| Parameter  | Symbolic variables  |
| --- | --- |
| WFNAME, NFNAME, FNAME for the CFTSEND/SEND and CFTRECV/RECV commands  |  • &amp;FDATE, &amp;FTIME, &amp;FYEAR, &amp;FMONTH, &amp;FDAY<br /> <br/> • &amp;BDATE, &amp;BTIME, &amp;BYEAR, &amp;BMONTH, &amp;BDAY<br /> <br/> • &amp;SPART, &amp;RPART, &amp;PART, &amp;NPART, &amp;GROUP<br /> <br/> • &amp;SUSER, &amp;RUSER<br /> <br/> • &amp;SAPPL, &amp;RAPPL<br /> <br/> • &amp;IDF, &amp;PARM, &amp;IDA<br /> <br/> • &amp;NIDF<br /> <br/> • &amp;NFNAME (only for FNAME and WFNAME)<br /> <br/> • &amp;IDT (only the FNAME and WFNAME parameters when you receive a file)<br /> <br/> • &amp;SYSQQ<br/> • &amp;WORKINGDIR |
| EXEC, EXECE, PREEXEC for the CFTSEND/SEND and CFTRECV/RECV commands<br/> EXECRE, EXECSE, EXECRF, EXECSF, EXECSFA, EXECSM, EXECRM, EXECSMA for CFTPARM command |  • &amp;SPART, &amp;RPART, &amp;PART, &amp;GROUP, &amp;NRPART, &amp;NSPART, &amp;USERID,&amp;GROUPID<br/> • &amp;BDATE, &amp;BTIME, &amp;BYEAR, &amp;BMONTH, &amp;BDAY<br/> • &amp;CDATE, &amp;CTIME, &amp;CYEAR, &amp;CMONTH, &amp;CDAY<br/> • &amp;FDATE, &amp;FTIME, &amp;FYEAR, &amp;FMONTH, &amp;FDAY<br/> • &amp;EDATE, &amp;ETIME, &amp;EYEAR, &amp;EMONTH, &amp;EDAY<br/> • &amp;COMMENT<br/> • &amp;SUSER, &amp;RUSER<br /> <br/> • &amp;SAPPL, &amp;RAPPL<br /> <br/> • &amp;PARM, &amp;MSG, &amp;PI99<br /> <br/> • &amp;DIAGI, &amp;DIAGP, &amp;DIAGC<br /> <br/> • &amp;FNAME*, &amp;UNIT*, &amp;UNITC*, &amp;NFNAME*, &amp;NFVER*, &amp;FDB*, &amp;SELFNAME*, &amp;FUNITC*, &amp;FUNIT*, &amp;FPATH*, &amp;FROOT*, &amp;SFNAME*, &amp;WORKINGDIR*, &amp;HOME*<br /> <br/> • &amp;IDF*, &amp;PIDTU, &amp;IDTU, &amp;IDT, &amp;NIDT, &amp;NIDF*, &amp;IDA, &amp;IDM, &amp;NSUB, &amp;PATH*, &amp;ROOT*, &amp;SUF*<br /> <br/> • &amp;FCODE, &amp;NCODE, &amp;fcharset, &amp;ncharset<br /> <br/> • &amp;BLKNUM<br /> <br/> • &amp;CFTEVENT, &amp;CFTNAME<br/> • &amp;FMCL, &amp;MODE, &amp;TRTYPE<br/> • &amp;FBLKSIZE*, &amp;FKEYLEN*, &amp;FKEYPOS*, &amp;NKEYLEN*, &amp;NKEYPOS*, &amp;FLRECL*, &amp;FORG*, &amp;FRECFM*, &amp;FSPACE*, &amp;FTYPE*<br/> • &amp;NBR*, &amp;NBC*, &amp;NBT*, &amp;TT, &amp;QQ, &amp;COMP, &amp;NOTIFY, &amp;SYSQQ<br/> • &amp;SSLAUTH, &amp;SSLCIPH, &amp;SSLMODE, &amp;SSLPROF, &amp;SSLPARM, &amp;SSLRMCN, &amp;SSLRMCA, &amp;SSLUSER, &amp;SSLCFNA, &amp;SSL<br/> • &amp;XLATE<br/> • &amp;SYSDATE, &amp;SYSTIME, &amp;SYSDAY<br/> • &amp;PRI<br/> • &amp;XFRCYCID, &amp;XFROBJID<br/> • &amp;EXITFREE<br/> • &amp;JOBNAME, &amp;NCHARSET<br/> • &amp;APPSTATE, &amp;PHASESTEP, &amp;PHASE<br/> • &amp;[TARGETAPPL](../parameter_intro/sourceappl)<br/> <blockquote> **Note**<br/> You cannot use the variables designated by asterisk (*) in procedures associated with the EXEC* parameters relative to message transfers.<br/> </blockquote>  |
| EXEC for CFTACCNT or CFTLOG  | &amp;FACCNT, &amp;FLOG  |
| TLVCEXEC, TLVWEXEC for CFTCAT  | &amp;FCAT, &amp;SYSDATE, &amp;SYSTIME, &amp;CFTEVENT, &amp;SYSDAY, &amp;CFTNAME, &amp;RUNTIMEDIR  |
| TLVCEXEC, TLVWEXEC for CFTCOM  | &amp;SYSDATE, &amp;SYSTIME, &amp;CFTEVENT, &amp;SYSDAY, &amp;CFTNAME, &amp;RUNTIMEDIR  |
| USERID parameter of the CFTSEND and CFTRECV commands  | &amp;RUSER, &amp;SUSER, &amp;RAPPL, &amp;SAPPL, &amp;RPART, &amp;SPART, &amp;PART  |
| EXIT of the {{< TransferCFT/axwayvariablesComponentShortName  >}} CFTSEND/SEND and CFTRECV/RECV commands  | &amp;IDF  |
| FNAME parameter of CFTDEST  | &amp;FDATE, &amp;FTIME, &amp;FYEAR, &amp;FMONTH, &amp;FDAY<br /> <br/> &amp;PART, &amp;RPART, &amp;SPART, &amp;NPART, &amp;GROUP<br /> <br/> &amp;SUSER, &amp;RUSER<br /> <br/> &amp;SAPPL, &amp;RAPPL<br /> <br/> &amp;IDF, &amp;PARM, &amp;IDA<br /> <br/> &amp;NIDF<br /> <br/> &amp;NFNAME, &amp;NFVER |
| The name of the identifier of the IDF parameter of the CFTPROT command  | &amp;NIDF |
| The IDA parameter of SEND and CFTSEND  | &amp;FNAME, &amp;FUNITC, &amp;FUNIT, &amp;FPATH, &amp;FROOT, &amp;FSUF, &amp;NFNAME, &amp;PART,<br/> &amp;IDF, &amp;IDTU, &amp;IDT, &amp;IDM, &amp;COMMENT, &amp;SYSDATE, &amp;SYSTIME |
| SUSER and RUSER parameters of SEND and CFTSEND  | &amp;USERID, &amp;FNAME, &amp;FUNITC, &amp;FUNIT, &amp;FPATH, &amp;FROOT, &amp;FSUF, &amp;NFNAME, &amp;PART, &amp;IDA, &amp;IDF, &amp;IDTU, &amp;IDT, &amp;IDM, &amp;COMMENT, &amp;SYSDATE, &amp;SYSTIME, &amp;FCHARSET, &amp;NCHARSET |
| PARM, SAPPL, RAPPL parameters of SEND and CFTSEND  | &amp;FNAME, &amp;FUNITC, &amp;FUNIT, &amp;FPATH, &amp;FROOT, &amp;FSUF, &amp;NFNAME, &amp;PART, &amp;IDA, &amp;IDF, &amp;IDTU, &amp;IDT, &amp;IDM, &amp;COMMENT, &amp;SYSDATE, &amp;SYSTIME, &amp;FCHARSET, &amp;NCHARSET |
| UCONF sentinel.xfb.cyclelink.metadata  | &amp;IDA, &amp;IDF, &amp;IDTU, &amp;IDT, &amp;NPART, &amp;NIDF, &amp;PART, &amp;PARM, &amp;COMMENT, &amp;RAPPL, &amp;RPART, &amp;RUSER, &amp;SAPPL, &amp;SPART, &amp;SUSER, &amp;SOURCEAPPL, &amp;TARGETAPPL |


******Example******

A file name can consist of the day’s date and the partner’s name:  the
description command for the PAY file in reception.

```
RECV IDF = PAY, FNAME = PAY&4PART.&FDAY
```

When a file of this type is received from the ALPHSITE partner on July
14 13:

```
RECV PART = ALPHSITE, IDF = PAY
```

{{< TransferCFT/axwayvariablesComponentShortName  >}} creates and writes to a file: PAYALPH.14

See the end-of-transfer examples in [Transfer-related
procedure examples](../../../concepts/about_transfer_processing/procedure_examples).

## Defining symbolic variable blacklists for processing scripts

**UNIX and Windows only**

You can use blacklist characters as a POSIX Regular Extended expression to define forbidden characters in a processing script. To prevent unauthorized actions, do not use these characters in symbolic variables.

****Defining the blacklist****

Use the uconf `cft.server.processing_scripts_variables_blacklist` parameter to define the character sequence to forbid. We recommend setting this parameter to **`&#124;\\$\\(&#124;;&#124;&&#124;\\&#124;** for UNIX, and **"&"** for Windows.

****UNIX****

```
uconfset id=cft.server.processing_scripts_variables_blacklist , value= "`&#124;\\$\\(&#124;;&#124;&&#124;\\&#124;"
```

When setting the blacklist values shown above, the forbidden characters are: **`&lt;&gt;`ls`&lt;&gt;`** characters, respectively.

****Windows****

```
uconfset id=cft.server.processing_scripts_variables_blacklist , value="&"
```

****DIAGI 158****

This DIAGI indicates that there was an error while replacing the {{< TransferCFT/axwayvariablesComponentLongName  >}} variables.

****Log messages****

* CFTS67E: Error replacing variable &lt;var> &lt;error message>
* CFTS68E: PART=&part [IDF=&idf &#124; IDM=&idm]IDT=&idt _ &fname not executed
