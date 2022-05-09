{
    "title": "File  management functions",
    "linkTitle": "File management functions",
    "weight": "260"
}-   [Managing
    file flushes](#Managing_file_flushes)
- [Managing
    access conflicts to CFT working files](#Managing_access_conflicts_to_CFT_working_files)
- [Recognizing
    file types](#Recognizing_file_types)
- [Sending
    a group of files](#Sending_a_group_of_files)
- [Disabling
    the homogeneous mode](#Disabling_the_homogeneous_mode)

<span id="About_file_management_functions"></span>

About file management functions
-------------------------------

<span id="Managing_file_flushes"></span>

### Managing file flushes

In certain circumstances, Transfer CFT performs an operation that forces a given file to be physically written to the disk - a
physical flush operations.

During the transfer, Transfer CFT implements two types of flushes:

- Flushes
    on received files. These types of flush take place each time synchronization
    points are received.
- Flushes
    on the Transfer CFT catalog when synchronization points are received.
    See also the CFTCAT [UPDAT](../../../../../c_intro_userinterfaces/command_summary/parameter_intro/updat) parameter.
     

These flush operations tend to secure transfers against risks
caused by serious hardware malfunctions and unexpected mains power failures.
Since this security behavior is time-consuming,you may feel that such behavior is not warranted.

### Deactivating the flush function

****Environment variable****

#### CFTNOFLUSH

You can modify one type of "flush",
those implemented on received files, by setting the CFTNOFLUSH
environment variable. Set the environment variable to 1 or to
0, depending on whether you want to suppress or implement this type of
flush.

For example, to deactivating the "flush" function:

`SET CFTNOFLUSH=1`

By default, transfer
flushes continue to occur.

<span id="Managing_access_conflicts_to_CFT_working_files"></span>

Managing access conflicts with Transfer CFT working files
---------------------------------------------------------

In certain operating configurations, particularly in a Transfer
CFT/Server – Transfer CFT/Client architecture, the Transfer CFT working
files are subject to frequent requests for simultaneous write access;
particularly for the communication file. To manage access conflicts
the file (or part of it) is locked during the write operation. In other
words, during the operation, only the process currently writing has access
to the file being written.  

During this time the other "waiting to write" processes repeat
their access requests to the operating system.

****Environment variable****

#### CFTLCKMAX

You can modify the maximum number of attempts to access
a file before a request fails (and a failed write attempt message is sent)
to the operating environment.

The CFTLCKMAX environment variable gives users the option of
adapting the maximum number of access attempts to their environment.

To access a file, Transfer CFT effects 50\*CFTLCKMAX attempts.  
The time between two attempts is calculated randomly between 0 and 500
msec.

By default, CFTLCKMAX=1

For example, if the defined environment is CFTLMAX=10, there are 10\*50 attempts to access
files before a fail message is posted.

<span id="Recognizing_file_types"></span>

Recognizing file types
----------------------

The Windows operating systems only handle files known as binary "stream"
files. Therefore, with these operating systems you do not know
the type of data (binary, text, other) that a file contains.

An attempt to determine the
type of data contained in a file by making a semantic study of its content is not adequate. To remedy this, Transfer CFT proposes
a special file type recognition function using the suffix.

Change of name or path of suffix file

****Environment variable****

#### CFTSUFX

By default the suffix file is called CFTSUFX and it is located in the
working directory.

To change the name and/or path of the suffix file
use the CFTSUFX environment variable.

The data which enables Transfer CFT to ascertain a file type from its
suffix is collected into a text file of which the logical name is CFTSUFX
(see also the paragraph Logical File Names).

The CFTSUFX file is made up of lines that may consist of:

- A comment
- The definition
    of a suffix, possible followed by a comment

A comment is any item of text beginning with the character ‘\#’.

A suffix is defined in accordance with the following syntax:

&lt;suffix of 1 to 3 letters&gt;=&lt;letter defining the file type&gt;

Only characters supported by the operating system can be within the
first 1-3 letters defining the suffix. Wild cards, ‘?’ and ‘\*’ cannot
be used.

There are three main file types:

- Binary files

Transfer CFT treats this type of file as any collection of
bytes. In this type of file, no binary configuration takes any particular
role. In the Transfer CFT parameterization, this type of file is characterized
by the letter "B".

- Text
    files

Transfer CFT treats this type of file as a series of text
lines each separated by the pre-defined control code sequence CR-LF (0x0D
– 0x0A).  
This type of file may or may not end in the binary code 0x5A (ctrl Z) For
all Transfer CFTs, a text file is characterized by the letter "B".

- Variable
    files

In this type of file, records with binary contents are preceded
by 2 bytes, stating the length of the record. This type of file can be
generated, provided the parameterization is adequate, by Transfer CFT
or by its COPYFILE utility. In the Transfer CFT parameter setting, this
type of file is characterized by the letter "V".

Adding types for the Transfer CFT/Server – Transfer CFT/Client architecture
---------------------------------------------------------------------------

In this architecture, you can operate Transfer CFT/Client
Windows with a UNIX Transfer CFT/Server.

When you select a Transfer CFT/Server-client architecture, the Transfer
CFT/Server and Transfer CFT/Client elements should be specified in conjunction
with Axway Sales Department.

There are certain differences in the way Transfer CFT manages text files
between Windows machine and UNIX machines. These should be taken into
account to obtain satisfactory functioning.

In a UNIX text file, the lines are separated from one another by the
single character "0x0A", while in a Windows text file, the line
separator is 2 characters "0x0D0A". When reading, this difference
causes no problem at all. When writing, users must specify to Transfer
CFT the type of text file it must create (UNIX text file, or Windows text
file).

Transfer CFT provides you with two other letters, in
addition to the letter "T", so that the text file properties
can be given in full within a Transfer CFT server/client architecture,
operating UNIX and Windows machines:

- O: forces Windows
    text type
- X: forces UNIX
    text type

The "T" type signifies native text (a Transfer CFT/Windows
generates a readable text file in the Windows environments, a Transfer
CFT/UNIX generates a UNIX text file).

Files of type "text" are managed in the same way on Windows systems, which are considered by Transfer CFT
to be standardized environments.

The table below summarizes the letters indicating the type of file in
the CFTSUFX file.

****Letters defining a file type****


| Letter  | Type of file  |
| --- | --- |
| B  | Binary file  |
| O  | Text file (Windows text files)  |
| V  | Variable file (in the Transfer CFT sense)  |
| T  | Text file (native)  |
| X  | Text file (UNIX)  |


Example of the content of the CFTSUFX file:

`DOC=T     #files with a ‘DOC’ suffix are text files.T*=T     #files with a suffix beginning with the letter   ‘T’#are text files.EXE=B     #files with an ‘EXE’ suffix are binary files.V?R=V     #files with a ‘V?R’ suffix are variable files.`

Differentiation between upper and lower
case

****Environment variable****

#### CFT_CSFN

Transfer CFT/Windows does not differentiate between upper and lower
case in the suffix names described in the suffix file. Such differentiation
does take place if the environment variable CFT_CSFN is set.

For example:

`SET CFT_CSFN = 1`

<span id="Sending_a_group_of_files"></span>

Sending a group of files
------------------------

This section describes how to create a command to send a group
of files. To better understand this section, refer to the following general group file information:

- [Sending
    a group of files](../../../../../concepts/send_command/send_group_of_files_cl)
- The [FNAME](../../../../../c_intro_userinterfaces/command_summary/parameter_intro/fname)
    and [WFNAME](../../../../../c_intro_userinterfaces/command_summary/parameter_intro/wfname) parameters
    in the CFTSEND and CFTRECV commands

A group file send request takes place implicitly when the value of the FNAME parameter
for the CFTSEND command has the following two characteristics:

- The first character
    is a surrogate character ‘\#’
- The FNAME parameter
    states a folder, or contains meta-characters

****Example****

`FNAME = #FIC*.*`

A group of files can be sent in two different ways depending on whether
the two partners are standardized sites or not. The term of standardized
sites means that the operating system of the two Transfer CFT partners
have identical file systems (for example, Windows is considered a
standardized system).

To indicate to a local Transfer CFT that it is not in standardized mode
with a remote partner, the SYST parameter in the CFTPART command should
be used. The SYST parameter makes it possible to indicate to the local
Transfer CFT that it and the remote Transfer CFT are on different operating
systems (not the same file system).

It is not necessary for the SYST parameter between standardized systems
to contain data.

### Sending a group of files in mixed mode

Transfer CFT detects that it is in a mixed environment because the SYST
parameter of the CFTPART command contains data and is different from the
local system.

In this mode, each file designated by the generic name of FNAME containing
meta-characters is the object of a special transfer.

This request to send generates as many posts in the catalogue as there
are files to transfer.

### Sending a group of files in standardized mode

The fact that Transfer CFT is sending a group of files in standardized
mode allows the supplementary functions described below to be implemented.

Transfer CFT detects that it is in standardized mode either because
the SYST parameter in the CFTPART command contains no data, or because,
containing data, the SYST parameter indicates the same system as that
on which the local Transfer CFT is based.

In both cases, the two Transfer CFT partners implement a device which
allows them to exchange only a single (large) file in place of all the
files designated by the generic FNAME of the sending Transfer CFT.

This implementation is performed by calling a concatenation process
external to Transfer CFT (before the transfer for the transmitter) then
a "de-concatenation" process (after the transfer for the receiver).

These operations are performed on large and medium-sized systems, by standard tools within the operative system (for example, the IEBCOPY utilities on the IBM host or tar on UNIX). This type of standard tool does not exist on Windows. Therefore, the tools zip/unzip are provided in the Transfer CFT Windows package.

These tools operate, either with several incoming files and one outgoing (this is the concatenation called prior to the transmission), or with a single incoming and several outgoing files (this is the de-concatenation called after reception).

Transfer CFT/Windows ensures that this function is "opened"
by calling different batches before transmission by the transmitter and
after reception on the receiver, as follows:

- The batch file
    CFTSVG01 is called before transmission and on the transmitter

This batch should constitute the file called WFNAME in the CFTSEND command.
This is the file which will actually be transmitted.

- The batch file
    CFTRST01 is called after reception and on the receiver

This batch should de-concatenate the file that has been received. This
file takes the name stated in the WFNAME parameter of the CFTRECV command.

The CFTSVG01 and CFTRST01 batch files must be located in the folder
as default, when Transfer CFT is executed.

These batch files are automatically called by the following parameters:

- For CFTSVG01:
    -   The 5th
        parameter (%5) designates all the files to concatenate and corresponds
        to the NFNAME of the CFTSEND command
    -   The 6th
        parameter (%6) gives the name of the out file for the concatenation utility
        and corresponds to WFNAME of the CFTSEND command
- For CFTRST01:
    -   The 5th
        parameter (%5) gives the name of the in file for the de-concatenation
        utility and corresponds to WFNAME of the CFTRECV command
    -   The 6th
        parameter (%6) designates all the out files to concatenate and corresponds
        to the NFNAME of the CFTRECV command

The other batch call parameters are unused, and therefore
insignificant.

### Parameter setting

To implement the transfer of a group of files in standardized
mode, the conditions on the Transfer CFT parameters are as follows:

- The parameter FNAME
    in the CFTSEND command is in the form of \#&lt;string&gt;, in which &lt;string&gt;
    indicates a folder or contains meta-characters
- The parameter WFNAME
    of the CFTSEND command is a string indicating the name of the out file
    for the concatenator on the transmitter
- The parameter WFNAME
    of the CFTRECV command is a string indicating the name of the IN file
    for the DE-concatenator on the receiver  
    This name does not need to be the same as that of WFNAME of the CFTSEND
    on the transmitter
- The parameter FNAME
    of the CFTRECV command indicates the full path of a folder name ending
    with the ‘/’ character

****Example****

`CFTSEND.FNAME = #FIC*.*,WFNAME = &idtu.snd,.`

`CFTRECV.FNAME = E:\CFTN.301\RECEPT\,WFNAME = &idtu.rcv,.`

> **Note**
>
> WFNAME must contain an extension (& idtu.rcv in this example) otherwise the ZIP or UNZIP utilities add .zip to the file name. Certain operations in CFTSVG01.bat CFTRST01.BAT cannot be performed then, and return the message "unknown or empty file".

To use the above function between two different but standardized
operating systems (such as Windows 7 for transmission and Windows Server for
reception), check that the tool (or tool pair) can be used
in your operating system. In the example above, the de-concatenation on Windows Server knows how to correctly restore on the Windows Server all of the files concatenated in Windows 7.

However there is a problem when the two Transfer CFT partners are standardized
and you simply want to have recourse to the functions associated with
transferring groups of files in mixed mode.

<span id="Disabling_the_homogeneous_mode"></span>

Disabling the homogeneous mode
------------------------------

Use the unified configuration parameter [uconf:cft.server.force_heterogeneous_mode](../../../../../admin_intro/uconf/uconf_heterogeneous_mode) to enable forced heterogeneous mode exchanges, and disable homogeneous mode.
