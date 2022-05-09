{
    "title": "fname",
    "linkTitle": "fname",
    "weight": "1220"
}<span id="fname"></span>

### fname

<span id="fname_CFTACCNT"></span>

#### CFTACCNT

****[FNAME = filename ]   string
64****

Name of the statistical file. This name can be a:

- physical name
- logical name

<span id="fname_CFTAUTH"></span>

#### CFTAUTH

****[FNAME = filename ]    string
512****

The name of the file where authorized or prohibited users are defined.

Enter the name of the file, up to 512 characters, in which you defined
the list of model file identifiers.

The number of identifiers in this list is not limited. Build the file
using the following rules:

- a record of this
    file can only contain one ****idf****
- the size of the
    record is limited to 80 characters
- an ****idf****
    must start in the first column and only the first 32 characters of the
    record are taken into account
- characters after
    the 32nd character are ignored
- the identifier
    can be entered in upper or lower case
- the file can contain
    records of zero length

You cannot complete this field if you have selected the ****idf****
button in the old Transfer CFT UI.

<span id="fname_CFTCAT"></span>

#### CFTCAT

****[FNAME = filename ]   string
64****

Catalog file name. Service files, such as Catalog and Log. This name can be:

- a physical filename
- a logical name

<span id="fname_CFTCOM"></span>

#### CFTCOM, CFTFILE, CONFIG

****[FNAME = filename ]   {string 64}****

<span id="fname_CFTFILE"></span>

#### 

<span id="fname_COPYFILE"></span>

#### COPYFILE

****[FNAME = filename ]   {string 512}****

<span id="fname_CFTDEST"></span>

#### CFTDEST

****[FNAME = filename]
 {512
characters }****

Name of the file in which the list of the identifiers of the various
partners, each corresponding to one value of the ID parameter of a CFTPART
object, is defined.

The number of identifiers in this list is not limited.

The existence of each partner is checked at the time of the transfer.
If a partner is not defined, this does not prevent files being broadcast
to or collected from the partners defined; only the partners not described
are not served.

To build this file, the following rules must be followed:

- A record of this
    file can only contain one partner identifier
- The size of a record
    is limited to 80 characters
- A partner identifier
    must begin in the first column and only the first 32 characters of the
    record are taken into account
- Any characters
    after column 32 are ignored and considered to be a comment
- An identifier may
    be entered in either upper case or lower case letters (it is converted
    into upper case letters)
- The file may contain
    records of zero length

File example:

![Example file names for distribution list](/Images/TransferCFT/fname_dest_ex.png)

{{< TransferCFT/axwayvariablesComponentShortName  >}} does not check the transfer requester access rights for this file.

If FOR=COMMUT (broadcasting by a intermediate
site):

- The symbolic variable &SPART, network identifier of the sender partner,
    may be used in forming the name of the file (FNAME parameter value). This
    makes it possible to make a distinction between the lists defined for
    the various originating sites.

The following symbolic variables can be used:

- &FDATE, &FTIME,
    &FYEAR, &FMONTH, &FDAY
- &PART, &RPART,
    &SPART, &NPART, &GROUP
- &SUSER, &RUSER
- &SAPPL, &RAPPL
- &IDF, &PARM,
    &IDA
- &NIDF
- &NFNAME, &NFVER

<span id="fname_CFTRECV"></span>

#### CFTRECV, RECV

**[FNAME = filename]    {1...512}**

Name of the physical receiver file,
filename or complete path name, of the directory.

When the parameter value is between quotes, it becomes case-sensitive; however on certain platforms, such as UNIX, the value is case-sentive without quotes.

In the receiver server configuration, the use of this parameter
is mandatory.

In the *receiver requester* configuration, the filename may be
defined in the RECV command or in the CFTRECV object, though preferably in CFTRECV.

You can define the filename either in the:

- Receive command, or
- The CFTRECV
    object (recommended)


| To receive... | Enter... |
| --- | --- |
| a file | a complete physical file name |
| a version of a file | a file name with a root and a version number |
| a group of concatenated files | a directory name |


*****When using the complete
filename*****

The complete path name includes the names of directories, or any other
organization specific to the environment concerned, used to group files:
library, catalog, PDSE, etc.

Normally, the folder referenced in `fname ` parameter should exist or the transfer fails. However, depending on your environment, you may use a special character that can be set with the cft.char_directory_protect to implicitly create part of a path structure. An OS specific character delimits the path to be created (intermediate directories), where the names of the sub-directories appearing to the right of the character are created. Please see the uconf [char_directory](../../../../cft_intro_install/about_this_document_vms/c_cft_introduction_vms/installation/platform_specific_characters_and_functions) for more information.

**Example**

The tree structure is created after the plus special character (****+****):

`FNAME=’/home/cft/runtime/myapp/+user1/files/&idtu.rcv`

In  this example, the `user1 `and `files` folders are created if they did not already exist.

The filename may:

- Be assigned dynamically
    using symbolic variables
- Correspond to the
    name of a file with versions (GDG for instance) *z/OS only*

*****Using symbolic variables*****

The following variables may be used to form the FNAME character string:

- &BDATE, &BTIME, &BYEAR, &BMONTH, &BDAY
- &FDATE, &FTIME, &FYEAR, &FMONTH, &FDAY
- &SYSD,&SYST,&QQ,&SYSQQ
- &HOME,&USERID
- &SPART, &RPART, &PART, &IPART, &NPART, &GROUP,&NRPART,&NSPART
- &SUSER, &RUSER
- &SAPPL, &RAPPL
- &IDF, &PARM, &IDA,&PI99
- &NIDF, &IDTU, &IDT,&PIDTU
- &NFNAME,&FROOT,&FSUF,&FPATH,&FUNITC,&FUNIT,&SFNAME
- &NCHARSET,&FCHARSET

The ‘&’ character here replaces the char_symb character specific
to each operating system. Refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide*
corresponding to your OS.

```
**PeSIT E CFT/CFT
The &FUNIT, &FUNITC, &FPATH, &FROOT and
&FSUF variables are used so that the FNAME parameter gives the
full "path" pointing to the file to be written.
```
**

Specific case using &NFNAME symbolic
variable    

**PeSIT E CFT/CFT**

The use of &NFNAME is only valid if the Transfer CFT is a receiver server.

This variable is only used in the open mode.

*****Receiving a file with versions          *****

**z/OS,
OpenVMS**

This name includes a root and a version number.

The file is received as the temporary file defined in the WFNAME parameter
and must concern the receiver file version + 1 (mandatory operation with
RENAME).

The relative filename is converted into an absolute filename on completion
of the transfer. The temporary file is then renamed with the name defined
by FNAME.

> **Note**
>
> Note: When
> a temporary file is used, the WFNAME parameter, there may be restrictions
> related to the operating system. On IBM systems, for example,
> the type of unit must not appear in the name of the FNAME file.

```
**PeSIT E CFT/CFT profile
Receiving a group of copied/concatenated files. This
name must correspond to a directory name if a copy/concatenation
operation is performed when the files are sent (transfer of a group
of files between systems of the same type).
The data received is stored in the temporary file specified
in the WFNAME  parameter. The files are then deconcatenated in the
directory specified by FNAME.
```
<span id="fname CFTSEND__CFTRECV__CFTISEND"></span>**

#### CFTSEND, SEND

****[FNAME = {filename &#124; mask &#124; dirname &#124;
&lt;file-symb&gt;filename &#124; &lt;file-symb&gt;mask &#124; &lt;file-symb&gt;dirname}]    {string
512} ****

> **Note**
>
> Note: here the &lt;file-symb&gt; character is specific to each system (for example \# on Windows and @ on UNIX environments).

Name of the local file, directory, indirection file, selection mask
or selection directory to be sent. The maximum length of a filename value-type
is 512 characters.

When the parameter value is between quotes, it becomes case-sensitive; however on certain platforms, such as UNIX, the value is case-sentive without quotes.

****Examples****

The following examples use a UNIX syntax, modify accordingly for your environment.

FNAME=filename where the FNAME is expressed as an absolute name:

```
FNAME= '/home/cft/runtime/pub/FTEST'
```

`FNAME = filename` where the FNAME is expressed in relative name from runtime folder:

```
FNAME = 'pub/FTEST'
```

FNAME=dirname which transfers a file that contains the list of all files in the dirname folder (pub in this example), but not the actual files:

```
FNAME= '/home/cft/runtime/pub' or FNAME= 'pub'
```

FNAME=&lt;file-symb&gt;filename which transfers all the files referenced in the list file:

```
FNAME= '@/home/cft/runtime/pub/list'
```

FNAME=&lt;file-symb&gt;mask  which transfers all the files that correspond to the mask criteria:

```
FNAME= '@/home/cft/runtime/pub/F\*'
```

FNAME=&lt;file-symb&gt;dirname which transfers all the files in dirname folder:

```
FNAME= '@/home/cft/runtime/pub'
```
<span id="fname_CFTLOG"></span>

#### CFTLOG

**[FNAME = filename]
   {string
64}**

Name
of the log file.

Check that the file &lt;*filename*&gt; exists and is accessible;
if not, information will be truncated on the CFTOUT standard output and
will not be able to be used. This also applies when the current log file
is the file defined by AFNAME.

Do not use logical names.

<span id="fname_CFTXLATE"></span>

#### CFTXLATE

**[FNAME = filename]
  {string
512}**

Name of the file containing the description of the translation table
(1 record of 256 characters).

This file must have a sequential organization. Examples of such files
are given with the various products. Refer to the *Operations Guide*
specific to the system.

#### EXTAMCACHE

**[FNAME = filename]
  {string
}**

Name of the Access Management cache file.

[Return to Command index](../../)
