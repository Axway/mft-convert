---
    "title": "Sending  a group of files ",
    "linkTitle": "Sending a group of files",
    "weight": "190"
---
You can use the FNAME parameter in a SEND command, when prefixed with the &lt;file_symb&gt;
indirection character, to trigger SEND operations that correspond to:

- A list of file names in an indirection file (SEND FNAME = \#LIST for example) in heterogeneous mode only
- A series of files indicated by a generic name (SEND
    FNAME = \#FIL\* for example) or a directory name (SEND FNAME = \#DIRECTORY\\\*)
    -   In homogeneous mode
    -   In heterogeneous mode
    -   Using a file name filter

> **Note**
>
> Note: If FNAME designates a generic name and the &lt;file_symb&gt;
> indirection character is omitted, SEND FNAME = FIL\* for example, the SEND
> command triggers a single transfer, in which pseudo-records each of which corresponds
> to a file name are sent.

Additionally, the UCONF `cft.dirdepth` =`yes ` setting indicates that subdirectories and their content should be sent. Set this parameter value to `No `to send only the parent directory.

{{% TransferCFT/snippets/filenames%}}
<span id="Sending_files_designated_by_an_indirection_file"></span>

Use an indirection file
-----------------------

You can use a list of file names to send a group of files using the indirection file structure as follows:

- A record can contain
    only one file name
- A file name must
    begin in the first column
- The file can contain
    empty records
- The file name cannot contain an asterisk (\*)

#### Example

For a file called REPORTS containing the following list:

- file1
- file2
- file3

****Windows****

```
CFTUTIL SEND part=tokyo, idf=myfiles, fname=\#REPORTS
```

****UNIX****

```
CFTUTIL SEND part=tokyo, idf=myfiles, fname=@REPORTS
```

The files file1, file2 and file3 will be sent.

> **Note**
>
> Note: You cannot use a file name with an asterisk, for example fil\*, as that denotes the beginning of a file not a directory.

If there are N files to be sent, a SEND IDF = ID_EM, FNAME = \#GROUP (or @GROUP)
... command generates N+1 transfer entries in the catalog, as follows:

- One entry for each
    file transfer (that is N entries)
- A generic (virtual)
    entry, which never triggers an actual transfer but is used locally to
    manage the group of files to be sent
    -   This virtual transfer is identified by a DIAGP code set to LIST_FI,
        when the catalog is queried. Its state is immediately set to ****K****
        in the catalog.
    -   The generic entry is set to the ****T****
        or **X** state when all transfers have been set to ******T****** state (or **X** depending on the mode).
    -   The post-processing procedure is activated when all files in
        the group have been transferred (LIST_FI entry set to the ****T****
        or **X** state, depending on the mode).
    -   If the group file does not exist or cannot be opened, the generic entry
        remains set to the ****K**** state and
        error message CFTT34E is returned.
    -   If one of the files in the group cannot be sent (for example an unknown file), the other transfers are not affected,
        but the generic entry for the group is not set to the **T** state (or **X** depending on the mode).

> **Note**
>
> Note: See also COMPAT mode, if using backward compatibility.

For the receiver:

- A catalog entry is created for each file received
- The name of each file can be specified using the
    &FPATH, &FROOT and &FSUF symbolic variables

> **Note**
>
> Note: You can specify a list of directories to be sent in the indirection
> file.
> However, do not mix files and directories in the same indirection file.

****IBM i (OS/400)****

If your file contains the list of files to be sent, you must first create a REPORTS file in \*DATA format (\*SRC files contain a header and {{< TransferCFT/axwayvariablesComponentLongName  >}} cannot use these).

For example:

1. CRTLIB MYSEND
1. CRTPF FILE(MYSEND/REPORTS) RCDLEN(92) FILETYPE(\*DATA)
1. EDTF FILE(MYSEND/REPORTS)
    -   Add the list of files to transfer:
        -   MYSEND/FILE1
        -   MYSEND/FILE2
        -   MYSEND/FILE3
1. CALL PGM(CFTUTIL) PARM(SEND 'part=paris, idf=LISTFI,fname=\#MYSEND/REPORTS')

<span id="Sending_generic_name_files"></span>

Use generic name files
----------------------

To send a group of generic files, use the command:

```
SEND FNAME=\#*mask* or SEND FNAME=\#*dirname*
```

Where the FNAME parameter is set to one of the following values:

- A directory name,
    *dirname*, in which
    case all files accessible in the directory are to be sent
- A generic name,
    *mask,* including wildcard characters,
    in which case only the selected files are to be sent
- A directory name
    and a generic file name, in which case only the files selected in the
    directory are to be sent

The processing performed is of two types, depending on the operating
system of the remote site. The type of remote site is characterized by
the SYST parameter in the CFTPART command. Sites are said to be mono-platform
when the same operating system is used by both partners.

### Types of group file transfers

There are two types of send procedures for groups of files: homogeneous and heterogeneous.
These procedure types are described in detail further on in this topic.

#### Homogeneous send for a group of files

A homogeneous send occurs between two Transfer CFT that run on the same operating
system. This transfer procedure concatenates at the site sending the group of files and de-concatenates upon reception.

Mandatory parameters for homogeneous sends include:

- WFNAME: Determines transmission and reception in homogeneous sends, because
    the file resulting from the concatenation is the file that is sent.
- SYST: Defined for a remote partner, where the default value is the local operating system. Homogeneous transfers are only possible when CFTPART command's SYST value is the same as the local operating system.


| Platform  | UNIX-like environment  | Native  |
| --- | --- | --- |
| UNIX  | Available  | Available  |
| Windows  | Not supported  | Available  |
| z/OS  | Not supported  | Available  |
| IBM i  | Not supported  | Not supported  |
| HP Nonstop  | Available  | Not supported  |
| OpenVMS  | Not supported  | Not supported  |


****Example****

An example of a homogeneous send in a Windows environment:

```
cftsend id = copie,
fname = \#c:\\e\\cft320\\tmp\\a\*,
wfname = c:\\e\\cft320\\&idtu.snd,
frecfm = v,
ftype = b,
mode = create
cftrecv id = copie,
fname = c:\\cft320\\bin\\recv,
faction = delete,
wfname = &idtu.rcv,
ftype = b
```

#### Heterogeneous send for a group of files

A heterogeneous send occurs between two Transfer CFT that run on dissimilar operating
systems. This type of group file transfer triggers the transfer
of all files belonging to the group.

****General syntax****

`Windows: fname =#directory\* `

`Unix: fname =@directory/* `

#### Force heterogeneous mode for a group of files

In {{< TransferCFT/axwayvariablesComponentShortName  >}} both homogeneous and heterogeneous mode are enabled by default. However, you may want to ensure that groups of files are transferred using only the heterogeneous mode. The UCONF configuration parameter` cft.server.force_heterogeneous_mode` allows you to do this, effectively disabling homogeneous mode even if the partner is configured for homogeneous exchanges.

For more information on sending groups of files and heterogeneous mode exchanges, see [Sending a group of files](#).

To force heterogeneous mode:

1. Access the unified configuration utility using either [command line](../../../admin_intro/uconf/uconf_w_cftutil) or the UI.
1. Set the following parameter to enable forced heterogeneous exchanges for group file transfers.

********Unix/Windows********


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.server.force_heterogeneous_mode  | No  | Force heterogeneous mode for group file transfers. This parameter replaces the deprecated environment variable: CFTSFMCPY.<br/> Possible values:<br/> • Yes: Force heterogeneous mode exchanges (override homogeneous mode)<br/> • No: Standard heterogeneous and homogeneous functioning |


#### Sending to a remote site in homogeneous mode

> **Note**
>
> Note: The following is valid when using the same system type and heterogeneous mode is not forced. When using Central Governance remember that heterogeneous mode is the default setting.

The files to be transferred are copied and concatenated in a work file
that must be specified in the WFNAME parameter of the send command.

On the sender side, two entries are
created in the catalog:

- A generic entry
    (LIST_FI) with the following attributes:
    -   FNAME =
        {dirname &#124; mask}
    -   and WFNAME
        = temp-filename

Its state is immediately set to K, once it has been created in the catalog.
This entry is set to the T (or X) state when all transfers have been set to T (or X depending on the compatibility mode).

- A transfer entry
    with the following attributes:
    -   FNAME =
        temp-filename
    -   WFNAME
        = {dirname &#124; mask}

On the receiver side, a single transfer
entry is created. The data is received in the work file that must be specified
in the WFNAME parameter and de-concatenated in the directory specified
in the FNAME parameter of the receive command.

Once the transfer has been completed, the work file is deleted by each
of the relevant partners. If the transfer is interrupted however, the
associated WFNAME work file is not deleted. It will only be removed when
the transfer has been restarted and successfully completed or the associated
catalog entry has been deleted.

The file attributes defined for the transfer (CFTRECV or RECV command)
apply to the copied/concatenated WFNAME file.

**********Sending to a remote site**********

********![](/Images/TransferCFT/send_to_remote_site.png)********

<span id="Heterogeneous send"></span>

#### Sending to a remote site with a different operating system in heterogeneous mode

The copy/concatenation operation is not performed for a remote site
with a different operating system, i.e. a heterogeneous send. If the WFNAME parameter is set in the
send command, it is ignored.

The sender creates the following entries:

- One transfer entry
    for each file selected after analyzing the generic request
- A generic entry
    identified by the DIAGP code set to LIST_FI. Once it has been created
    in the catalog, its state is immediately set to K. The state is changed
    to T (or X) when all related transfers are set to T (or X, depending on the compatibility mode).

The EXECSF end of transfer procedure is activated when all selected
files have been transferred. If one of the files concerned by the generic
transfer cannot be sent, the other transfers are in no way affected, but
the generic entry is not set to the T state (or X, depending on the compatibility mode).

On
the receiver side, one transfer entry is created for each file
received.

For operating reasons, the receiving {{< TransferCFT/axwayvariablesComponentLongName  >}} may wish to archive each
file received using a name derived from that of the sender site. The following
paragraphs explain how the sending {{< TransferCFT/axwayvariablesComponentLongName  >}} can send the name of each file
transferred to the partner via a generic request.

A receiving {{< TransferCFT/axwayvariablesComponentLongName  >}} can specify the name of each file received via the
&FROOT, &FPATH and &FSUF symbolic variables.

****Sending to a remote site with a different
operating system****

****![](/Images/TransferCFT/new_group_files.png)****

****Example****

In Windows, an example of a heterogeneous receive:

```
cftsend id = copie,
fname = \#c:\\e\\cft320\\tmp\\a\*,
wfname = c:\\e\\cft320\\&idtu.snd,
frecfm = v,
ftype = b,
mode = create
cftrecv id = copie,
fname = c:\\cft320\\bin\\recv,
faction = delete,
wfname = &idtu.rcv,
ftype = b
```
<span id="Create"></span>

### Create filters using CFTSEND

You  can use the CFTSEND [FILTER](../../../c_intro_userinterfaces/command_summary/parameter_intro/filter) and [FILTERTYPE](../../../c_intro_userinterfaces/command_summary/parameter_intro/filtertype) parameters to apply file filtering based on the FNAME.

For example, create a filter that includes all .jpg files that are:

- A word (at least one other word character)
- Followed by four-digit number

#### Heterogeneous mode

For example, a transfer from a Unix platform to Windows, the following filter would include all of the files in "myfolder" that match the pattern described above such as the file IMGP0122.jpg.

```
 CFTSEND ID = 'findfile',
 FILTERTYPE = 'EREGEX',
 FILTER = '^[0-9a-zA-Z]+[0-9]{4}.jpg'
...
 
SEND PART = PARIS,
 IDF = 'findfile',
 FNAME = '@myfolder/\*'
...
```

For example on a z/OS platform, the following filter would include the files AXDSYN.TOOLS.GRPFIL. or GRPFIM. followed by A4 or A5:

```
CFTSEND ID = 'TREGEX03',
FILTERTYPE = 'EREGEX',
FILTER = '^-[\\.]+\*\\.-[^\\.]+\*\\.GRPFI(L&#124;M).A(4&#124;5)\\.'
...
 
SEND PART = PARIS,
IDF = 'TREGEX03',
FNAME = '\#AXDSYN.TOOLS.\*'
```

The FILTERTYPE can be either STRJCMP or EREGEX, used as described further on in this topic.

#### Homogeneous mode

For example, a transfer between Unix platforms, the following filter would include all of the files in "myfolder" that match the pattern described above such as the file IMGP0122.jpg.

```
 CFTSEND ID = 'findfile',
 FILTERTYPE = 'EREGEX',
 FILTER = '^[0-9a-zA-Z]+[0-9]{4}.jpg'
...
 
SEND PART = PARIS,
 IDF = 'findfile',
 FNAME = '@myfolder/\*'
WFNAME = '&idtu.tmp'
...
```
{{% TransferCFT/snippets/strjcmp_filter%}}
{{% TransferCFT/snippets/eregex_filter%}}
<span id="SNDINDFILEERR"></span>

Define the catalog details policy for group-of-files transfers
--------------------------------------------------------------

If you execute a SEND transfer request for a group of files that uses a sequential file as input, it creates as many transfer requests as there are lines in the input file, possibly causing the catalog to overfill.

To avoid this you can use the CFTPARM parameter SNDINDFILEERR to define the policy for a group of files type of transfer to address this issue if there is an error.

Parameter values:

- CONTINUE (default): Keep the existing behavior, which creates as many transfer requests as there are lines in the input file.
- ABORT: If the input file line is not a file, this gives the current transfer the status K diagi 132 diagp SNDINDFI, the generic transfer status is K diagi 200, and no other child requests are created.

****Catalog details****

Simplified catalog view when set to CONTINUE


| Transfer type  | IDTU  | PIDTU  | Phasestep  | Diagi  |
| --- | --- | --- | --- | --- |
| Parent  | 1  |   | H  | 0  |
| Child  | 2  | 1  | K  | 110  |
| Child  | 3  | 1  | C  | 0  |
| Child  | N  | 1  | C  | 0  |


Simplified catalog view when set to ABORT


| Transfer type  | IDTU  | PIDTU  | Phasestep  | Diagi  |
| --- | --- | --- | --- | --- |
| Parent  | 1  |   | H  | 200  |
| Child  | 2  | 1  | K  | 132  |

