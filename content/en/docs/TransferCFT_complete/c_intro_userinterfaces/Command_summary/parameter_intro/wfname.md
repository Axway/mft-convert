{
    "title": "wfname",
    "linkTitle": "wfname",
    "weight": "3780"
}<span id="wfname"></span>

### wfname

<span id="wfname_CFTRECV"></span>

#### CFTRECV, RECV

****[WFNAME =
filename]  {STRING512}
OS****

****MVS {String
248}****

Name of the temporary file used during the transfer.

The file is renamed on completion of the transfer using the name defined
by the FNAME parameter.

When the parameter value is between quotes, it becomes case-sensitive.

A temporary file is used to ensure the integrity of the file received:
the file which can be used (by a user application for example) is only
available at the end of the transfer.

- The name of this
    file may include the following symbolic variables:
- &FDATE,
    &FTIME, &FYEAR, &FMONTH, &FDAY
- &SPART,
    &RPART, &PART, &NPART, &GROUP
- &SUSER,
    &RUSER
- &SAPPL,
    &RAPPL
- &IDF, &PARM,
    &IDA
- &NIDF,
    &NFNAME, &IDT
- &BDATE,
    &BTIME, &BYEAR, &BMONTH, &BDAY

The ‘&’ character here replaces the char_symb character specific
to each operating system. Refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} *"Operations
Guide"* corresponding to your OS.

You should make sure that the assigned name is unique by adding the &IDTU variable to the filename so
there are no access conflict problems.

This parameter can be used for "store and forward" purposes
(CFTRECV ID = COMMUT command).


| ****MVS, VMS**** | This parameter is mandatory if the receiver file is a version file (in particular for operation in the open mode and when the sender sends an NFNAME corresponding to a GDG name). |
| --- | --- |


This parameter is mandatory when a copy/concatenation operation is performed
during the send, transfer of a group of files in PeSIT CFT/CFT mode.

<span id="wfname_CFTSEND"></span>

#### CFTSEND, SEND

******[WFNAME =
*filename*]  {STRING
512}   OS******

******MVS {String
248}******

Name of the temporary file used to send a group of files selected in
line with the generic name specified in FNAME.

This file is only used when additional processing is required on the
target machine. Once sent, it is deleted.

Example:

****MVS - IEBCOPY
procedure for partitioned files (PDSE)****

For additional information on the file and its structure, refer to the
*Installation and Operations Guide* specific to your operating system.

The files to be sent are selected in line with the name specified in
the FNAME parameter. The temporary file then contains the data to be sent.
The file is transferred in the same way as a sequential file.

This process can only be used for:

- transfers
    between systems of the same type (same SYST parameter value in the CFTPART
    command)
- the
    following protocol: **PeSIT CFT to CFT**

The following variables may be used to form the WFNAME character string:

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
    &NFNAME, &IDT
- &BDATE,
    &BTIME, &BYEAR, &BMONTH, &BDAY

The ‘&’ character here replaces the char_symb character specific
to each operating system. Refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide*
corresponding to your OS.

To avoid access conflict problems, be sure that the assigned name is
unique.

 

[Return to Command index](../../)
