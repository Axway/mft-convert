{
    "title": "nfname",
    "linkTitle": "nfname",
    "weight": "2200"
}<span id="nfname"></span>

### nfname

#### CFTSEND, SEND

**[NFNAME =**{***filename
&#124; \*filename*}]     {string
512, or string 80 depending on the PeSIT PROF setting}**

**PeSIT**

Name of the physical file at the receiver partner site.

If the file name is preceded by an asterisk \*, the receiver can choose
to keep the current transmitted name (in open mode) or rename the file. The file name
can be created from symbolic variables, or on z/OS systems correspond to the name of a
file with versions.

The file is transferred providing the following conditions are met:

- The
    receiver partner authorizes the sender site (requester or server) to set
    the physical name of the file to be received (this name is defined from
    parameter settings such as FNAME=&NFNAME)
- The
    file designated by NFNAME exists
- The receiver partner must support the use of PI37 as a network identifier, for example in Axway Gateway and SecureTransport
- The maximum length is 512 when PeSIT PROF=ANY, however this value it truncated to 80 characters when PeSIT PROF=CFT

#### CFTRECV, RECV

**[NFNAME =**{***filename*
}]     {string
512}**

**PeSIT**

Name of the physical file at the sender partner site.

The file name
can be created from symbolic variables, or on z/OS systems correspond to the name of a
file with versions.

The file is transferred providing the following conditions are met:

- The
    sender partner authorizes the receiver site (requester or server) to set
    the physical name of the file to be sent (this name is defined from
    parameter settings such as FNAME=&NFNAME)
- On z/OS system, the
    version number of the GDG file corresponds to the version number of the
    receiver file + 1

**SFTP**

Name of the physical file on the sender partner site. The file is transferred providing the sender partner authorizes the receiver site (requester or server) to set
the physical name of the file to be sent (this name is defined from
parameter settings such as FNAME=&NFNAME).

### Use symbolic variables with NFNAME                

The following variables may be used to form the NFNAME character string:

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
- &NIDF
- &BDATE,
    &BTIME, &BYEAR, &BMONTH, &BDAY

The ‘&’ character here replaces the char_symb character specific
to each operating system (refer to the {{< TransferCFT/axwayvariablesComponentShortName  >}} *Operations Guide*
corresponding to your OS).

[Return to Command index](../../)
