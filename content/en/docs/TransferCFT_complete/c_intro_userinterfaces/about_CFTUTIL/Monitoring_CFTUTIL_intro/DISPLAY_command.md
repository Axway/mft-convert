{
    "title": "DISPLAY - Catalog output display model",
    "linkTitle": "DISPLAY - Catalog output display model",
    "weight": "280"
}The <span id="DISPLAY"></span>DISPLAY
command is an enhanced version of the LISTCAT command and displays
the catalog transfer field values. The output can be organized
by columns or by lines depending on the selected mode.

Display uses an external
XML file that lists and describes the format for customized models. This means the Display command can call an XML document as a fmodel parameter.

- In column mode, an adjustable title bar is displayed at the top of
    the catalog content to improve readability.
- In line mode, every line is presented horizontally
    in the form FIELD_NAME = FIELD_VALUE.

Several customizing options are
available in both modes to control the display format for field values such
as prefix, suffix, length, and alignment.

DISPLAY uses the same parameters as the [LISTCAT](../listcat_command) command and, with the exception of the CONTENT parameter, all common parameters use the
same semantics. However,
there are certain parameters that are applicable only for the DISPLAY
command, as described later in this topic.

The use of the DISPLAY command overrides all other global model
options. Parameters that are affected by this command are MODE, NA, and
EMPTY.

See also, LISTCAT/DISPLAY - Statistical variables.

About XML
---------

The XML formatting must comply with certain conventions:

- The file must begin with the header: &lt;?xml content='ascii'?&gt;
- Every tag &lt;tag&gt; must be closed &lt;/tag&gt;
- Using a tag in the format &lt;tag/&gt; is accepted but not recommended and should be empty

For more information, refer to an XML standards reference such as <http://www.w3.org/TR/REC-xml/>. Additionally, you can reference the sample template delivered with your Transfer CFT, which is located in the OS-specific distribution package:

- UNIX, Windows: DSPCNF.XML in runtime/conf
- z/OS (MVS): distlib.XMLLIB(DSPCNF) and instance.XMLLIB(DSPCNF)
- IBM i (OS/400): CFTPGM1/DSPCNF

****Details****

Fmodel structure

```
<?xml content='ascii'?>
<CFTDisplayFilter>
<Fields>
<Field>
<Field>
[...]
</Fields>
</CFTDisplayFilter>
```

Attributes for the &lt;CFTDisplayFilter&gt;


| Parameter  | Description  |
| --- | --- |
| id='string'  | Model ID, call within the DISPLAY command with the content parameter  |
| mode = 'column &#124; line'  | Output orientation (line or column)  |
| title_size = '-1 &#124; NUM'  | Title size, only in column mode (undefined or number)  |
| title_align = 'left &#124; center &#124; right'  | Title alignment (column mode only)  |
| line_prefix = '&lt;LF&gt;&#124;STR'  | Prefix in line mode (empty or string)  |
| line_suffix = '&#124; STR'  | Suffix in line mode (empty or string)  |
| default_prefix = '&#124; STR'  | Default prefix (empty or string)  |
| default_suffix = ' &#124; STR'  | Default suffix ('' in column mode and 'Line Feed' in line mode) (empty or string)  |
| default_empty = '&#124; STR'  | Default String if empty (empty or string)  |
| default_na = '&#124; STR'  | Default String if not applicable (empty or string)  |


Attributes for the &lt;Fields&gt; and &lt;Field&gt;

The &lt;Fields&gt; tag has no attributes, but may contain one or several &lt;Field&gt; tags.

Each &lt;Field&gt; tag has the following attributes:


| Parameter  | Description  |
| --- | --- |
| id  | This parameter is mandatory and should be the same as the listcat id parameter  |
| title  | Title of the column / line.  |
| maxlength : -1 &#124; NUM  | Max length: -1 means no maxlength  |
| minlength : -1 &#124; NUM  | Min length: -1 means no minlength  |
| prefix =' &#124; STR'  | Prefix (empty or string)  |
| suffix =' &#124; STR'  | Suffix (empty or string)  |
| align = left &#124; center &#124; right  | Field alignment  |
| na = '&#124; STR' : default  | String if empty (empty or string)  |
| empty = '&#124; STR' : default  | String if not applicable (empty or string)  |


Parameter descriptions
----------------------

Use this command to displays the catalog
transfer field values organized either by columns (mode=column) or
by lines (mode=line).

Command syntax: [DISPLAY](../../../command_summary)


| ****Parameter**** | ****Description**** |
| --- | --- |
| CONTENT  | Filter to use on the messages written in the active LOG file.  |
| DATETIMEMAX  | Use to display catalog transfers that happened on or before this end date and time according to the transfer record creation (DATEK, TIMEK).  |
| DATETIMEMIN  | Use to display catalog transfers that happened on or after this start date and time according to the transfer record creation (DATED, TIMED).  |
| DIAGI  | Define the diagi catalog transfer field display:<br/> • DIAGI=0: select transfers that have a DIAGI=0<br/> • DIAGI=ERROR: select transfers that have a DIAGI other than 0<br/> • DIAGI=* : select all transfers (default value) |
| DIRECT  | Transfer direction of the requests.  |
| EMPTY | Use this parameter to replace the default output of ****Empty**** values, usually empty string values.<br/> The default string ****ANY**** means that EMPTY is specified in the model. The default EMPTY used is '-' if it is not defined in the model. |
| FILE  | Enter file name  |
| FMODEL | Complete name or logical name of the XML model file.<br/> This parameter default value is fixed.  |
| FOUT  | ****PeSIT**** You can extract Transfer CFT messages from the Catalog file, and forward these messages to a specified file using the fout parameter.<br/> The message length for PeSIT ANY profile, when forwarding a message from one CFT to another, has increased from 512 to 4096 bytes. The S/RRUSIZE must be greater than the maximum message length and message information combined (for example, 4127).<br/> The fout parameter enables you to redirect output to a specified file. |
| HELP | Displays help information:<br/> • FIELDS: Output all the fields name available for display model creation<br/> • MODELS: Output all the models available in the current model file |
| IDA  | Local transfer identifier assigned by the user or user application. This identifier may be a search criterion for the catalog entry asso  |
| IDF  | File type identifier.  |
| IDT  | Transfer identifier. Identifies a transfer for a given partner and transfer direction.  |
| IDTU  | Catalog identifier. It is a unique, local reference to a transfer.  |
| MODE | This parameter is used to force a model's output mode.<br/> Two modes are available:<br/> • COLUMN: This mode outputs the catalog fields in columns with a title bar (see [LISTCAT CONTENT=BRIEF](../brief_catalog_listing)).<br/> • LINE: This mode outputs the catalog fields with one field per line prefixed by its title.<br/> The default value ANY means that the mode is specified in the model. The default mode COLUMN is used if not defined in the model either. |
| NA | Use this parameter to replace the default output of &quot;Non Applicable&quot; values. <br/> A Non Applicable value is a value that does not mean anything for the concerned transfer. For instance, the message content field doesn't mean anything for a file transfer, so the NA string will be displayed instead.<br/> The default string 'ANY' means that the NA is specified in the model. The default NA used will be '#' if not defined in the model either. |
| NPART  | Network name of the transfer partner.  |
| PART  | The local identifier for the site where the monitor runs.  |
| PHASE  | Refers to the highest level in the transfer flow cycle, for example X (done).  |
| PHASESTEP  | The processing phase step.  |
| PIDTU  | The parent idtu is the idtu of the generic transfer. This means that for a group of files, file collection, or for broadcasting, the child transfers are now linked to the parent via the PIDTU.  |
| RUSER | Displays value as defined in the CONTENT parameter. |
| SORTBY  | Use this parameter to display information in an alphabetical/alphanumberic order.<br/> For example, to sort by partner name and identifier, enter:<br/> <code>CFTUTIL DISPLAY SORTBY=(PART,IDF)</code><br/> Additionally, you can add a prefix to define the criteria direction. Use <code>+</code> to increase (default) or <code>-</code> to decrease. For example:<br/> <code>CFTUTIL DISPLAY SORTBY=(-IDTU)</code> |
| STATE  | Defines the transfer request state.  |
| SUSER | Displays value as defined in the CONTENT parameter. |
| TYPE  | Defines the concerned type (object, medium, etc.).  |


Examples
--------

****Example 1****

Displays all the fields described in ****listcat****
model concerning all transfers.

```
DISPLAY     CONTENT
= listcat
```

****Example 2****

Displays all the fields described in 'listcat' model concerning all
transfers. The COLUMN mode is overridden by the LINE. Moreover, every
invalid field value is replaced by the string '\#\#\#\#', and every empty
field value is replaced by the string '&lt;empty&gt;'.

```
DISPLAY    MODE
= LINE,
NA
= '\#\#\#\#',
EMPTY
= '<empty>'
```

****Example 3****

This shows an example of a display model file.

```
<?xml content='ascii'?>
<!-- LISTCAT Model : classical CFTUTIL LISTCAT with long
IDs -->
<CFTDisplayFilter id           =
'listcat' title_align  =
'right'>
<Fields>
<Field id='PART' title='Partner' />
<Field id='DIRECT' title='Direction' maxlength='1' suffix=''
/>
<Field id='TYPE' title='Type' maxlength='1' suffix=''
/>
<Field id='STATE' title='State' maxlength='1' suffix=''
/>     
<Field id='ACK' title='Ack' maxlength='1'/>
<Field id='IDF' title='IDF' minlength='4' />
<Field id='IDT' title='IDT' />
<Field id='IDTU' title='IDTU' />
<Field id='BLKNUM' title='BLK' />
<Field id='NREC' title='Transmited' align='right' />
<Field id='FREC' title='Total' align='right' />        <Field
id='MSG'         title='Msg'
    maxlength='32'
align='center'/>
<Field id='DIAGI'     title='Diagi'
/>
<Field id='DIAGP'     title='Diagp'
/>
<Field id='REQUSER' title='User' />
<Field id='REQGROUP' title='Group' />
<Field id='IDA'       title='IDA'
/> </Fields></CFTDisplayFilter>
```

The output for this command would resemble the following:

```
Partner    DTSA
IDF IDT IDTU  BLK
Transmited Total  Msg
 Diagi Diagp
 User Group
IDA
NEWYORK SFT- TEST1  D1217250
00000001 29  \#
  0
 CP 29%
user1 group1 -
PARIS    RFT-
TEST1  D1217250
00000002 30  2
  2
   \#
  0
    CP
29% -     -
     -
PARIS    SFT-
TEST2  D1217251
00000003 31  2
  2
   \#
  0
   CP
29% user1 group1 -
NEWYORK RFT- TEST2  D1217251
00000004 32  2
  2
   \#
  0
    CP
29% -   -
 -
PARIS   SMT-
MESSAGETEST1 D1217261 00000005 33 \# \# test message 1 0 - user1 group1
-
NEWYORK RMT- MESSAGETEST1 D1217261 00000006 34 \# \# test
message 1 0 -   -
 -      -
NEWYORK SMT- MESSAGETEST2 D1217263 00000007 35 \# \# test
message 2 0 - user1 group1 -
PARIS    RMT-
MESSAGETEST2 D1217263 00000008 36 \# \# test message 2 0 -  -
 -     -
```
