---
title: "Filename  conventions"
linkTitle: "Filename conventions"
weight: 170
--- This topic
provides examples of filenames, and file naming conventions.

> **Note**
>
> Tip  
> You can find a comprehensive list of file naming examples in the Send command page.

## Unauthorized characters

The following characters are system- restricted characters:

- UNIX: /\*? $()
- Windows: /\\:\*?"&lt;&gt;

> **Note**
>
> You can use the $ character on UNIX systems as an environment variable. To have a file created when the name includes one or more $ characters (without resolving the environment variable), see the UCONF cft.unix.throw_error_on_envvar_not_found variable.

## Naming the local file to be sent FNAME=filename

The fname is a complete physical filename. It can either:

- Be created dynamically
    from symbolic variables, or
- Correspond to the
    name of a version file

## Using symbolic variables

The following variables can be used to form the FNAME character string:

- &FDATE,
    &FTIME, &FYEAR, &FMONTH, &FDAY
- &SPART,
    &RPART, &PART, &NPART, &GROUP
- &SUSER,
    &RUSER
- &SAPPL,
    &RAPPL
- &IDF,
    &PARM, &IDA
- &NIDF,
    &IDTU
- &BDATE,
    &BTIME, &BYEAR, &BMONTH, &BDAY
- &NFNAME,
    &NFVER (see details
    below)

The values of the variables in the last two lines are set just before
the transfer. As the substitutions in the FNAME string are performed at
the time the request is saved in the catalog, these variables cannot be
used in FNAME except in the case of an implicit SEND command.

The ‘&’ character used here replaces the char_symb character specific
to each operating system .

Refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Installation and Operations Guide* that
corresponds to your OS.

### Reconstitute a filename

A filename can be composed of different elements, such as the unit, path, root, and suffix. When transferring a file between disparate systems, you can use symbolic variables to reconstruct the original filename at the receiving end. See [Symbolic variables](../symbolic_variables) for details.

### Specific case of the &NFNAME symbolic variable (PeSIT CFT/CFT)

The use of &NFNAME is only valid if the monitor is a sender server
and the send transfer is implicit, that is when CFTSEND IMPL=YES. In this
case, the physical filename proposed by the receiver requester partner
can be taken into account at each transfer.

> **Note**
>
> When the file sent corresponds exactly to the one the partner
> requested (FNAME= &NFNAME), this corresponds to the open operating
> mode.

### Sending of a file with versions (z/OS, OpenVMS)

This name includes a root and a version number. According to the case,
the relative name is converted into an absolute name in different stages
as shown in the following table.

| Command  | Version  | Parameter  | Conversion to an absolute name  |
| --- | --- | --- | --- |
| CFTSEND  | 0 or - n  | IMPL=YES  | at the start of the transfer  |
|   |   | IMPL=NO  | when the request is placed in the catalog  |
| SEND  |  • be 0 or - n<br/> • specific case of z/OS (MVS) (1) | FNAMEABS=YES  | when the request is placed in the catalog  |
|   |   | FNAMEABS=NO  | at the start of the transfer  |

\(1\) the version number may be 0 or
- n and the FNAMEABS parameter must be set to YES. A GDG file is rotated
at the end of the job.

****z/OS****

Example of a file with versions. The notation of the version of the file in the SEND stage is
the same as the last notation used in the JCL.

```
//ST1 EXEC PGM=USER
//DD1 DD DSN=FIL(- 1)
//ST4 EXEC PGM=CFTUTIL
   SEND     FNAME=FIL(- 1)
```

****OpenVMS****

The previous example of files with versions is also applicable if the
value of DSN and FNAME is FIL;1.

> **Note**
>
> For the &NFVER symbolic
> variable: the use of &NFVER is only valid if the Transfer CFT is the sender server
> and the send transfer is implicit (CFTSEND IMPL=YES). In this case, it
> is possible to send the file version requested by the partner. Use the following parameter format: FNAME=GDGNAME(- &NFVER)

<span id="Filename__listing_a_directory"></span>

### Listing a directory: filename

This section
provides an example of how to list a directory.

Example of listing a directory

```
FNAME={dirname &#124; mask}
```

The name specified can be a generic file name or a directory name. It
can include:

- Specific symbolic
    variables, such as &PART and &IDF
- The \* and ? wildcard
    characters

This mode is used, for example, to send the list of a local directory
contents to the partner. In this case, each record transferred contains
the name of a selected file.

<span id="Filename__sending_a_group_of_files"></span>

### Sending a group of files: filename

This section
provides an example of sending a group of files based on a selection.

Where, `FNAME={#mask &#124; #dirname}`

The name specified can be a generic file name or a directory name.

It can include:

- Specific symbolic
    variables, such as &PART and &IDF
- The \* and ? wildcard
    characters

The directory name represents any structure specific to the environment
and used to group files together: library, catalog, PDSE and so on.

The group of files to be transferred is selected dynamically based on
the generic name or directory name. Depending on the operating system,
the selection mechanism is the result of a directory list or equivalent.

If the files are sent to the same type of site (CFTPART SYST parameter
set to the same value as on the local system), the selected files are
copied and concatenated into a temporary file (see the WFNAME parameter)
defined in the send command. Two entries are created in the catalog: a
generic entry and a transfer entry.

If the files are sent to a different type of site, a generic entry is
created in the catalog, along with one transfer entry for each file selected.

For more information, see [Broadcasting: Sending
a set of files with the same IDF in send mode](../../../concepts/transfer_command_overview/broadcast_collect).

<span id="Sending_a_group_of_specified_files__filename"></span>

### Sending a group of specified files: filename

This
section provides an example of sending a group of files, the names of which
are listed in the specified file:

```
FNAME=#filename
```

The specified name is the full name of a physical file, containing the
list of files to be sent, list of physical file names, with one name per
record.

The FNAME parameter can contain specific symbolic variables, such as
&PART and &IDF.

The name of the indirection file is preceded by the &lt;file- symb>
character specific to each system. In most environments, the ‘#’ symbol
is used.

> **Note**
>
> Refer to the table of platform- specific characters that corresponds to your operating system.

A catalog entry is created for each file. Each file is transferred in
the same way as any other file.

For more information, see [Broadcasting: Sending
a set of files with the same IDF in send mode](../../../concepts/transfer_command_overview/broadcast_collect).

Additionally, when sending a group of files:

- You can specify
    a list of directories to be sent in the indirection file. The copy/concatenation
    mechanism works in the same way for each directory as for the other generic
    send modes.
- Do not mix files
    and directories in the same indirection file.
