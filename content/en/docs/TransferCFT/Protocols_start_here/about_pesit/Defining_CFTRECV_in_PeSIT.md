{
    "title": "Defining CFTRECV in PeSIT ",
    "linkTitle": "Defining CFTRECV in PeSIT",
    "weight": "200"
}The CFTRECV object contains the parameters controlling the reception
of data and the storage of the data received. This
topic describes the CFTRECV object parameters needed when using
the PeSIT xxternal protocol.

With the PeSIT external to SIT profile
protocol, the default values taken by the parameters associated with the
physical characteristics of the receiver file model (among others FSPACE,
FRECFM, FLRECL) are the ones formally sent by the sender partner (in particular
see the NSPACE, NRECFM, NLRECL parameters of the CFTSEND object).

In case of open operation, the FNAME parameter may also inherit the
NFNAME value of the sender partner.

The NCOMP parameter is used when the value of the RCOMP parameter of
the CFTPROT object is excessive for the file considered. For the list
of authorized values, refer to the description of the RCOMP parameter
of the CFTPROT object.

<span id="Identification_parameters"></span>

### Identification parameters

### PeSIT E

The RAPPL and SAPPL parameters identify the file receiver
and sender applications respectively. The RUSER and SUSER
parameters specify the file receiver and sender users respectively. These
identifications at protocol level are a new feature of PeSIT version E.
Transfer CFT Release 2.1.0 already supports this identification via its
specific extensions (PROF = CFT).

### PeSIT E CFT/CFT

For PeSIT E, the standard protocol only allocates eight characters to
carry these parameters. When in relation with a Transfer CFT however,
the maximum allowed length for RAPPL and SAPPL is 48 characters
and for RUSER and SUSER, it is 28 characters.

For some PeSIT profiles, the RAPPL, SAPPL, RUSER and SUSER parameters
can be sent to the partner during the selection phase.

<span id="About_the_RECV_object"></span>

About the RECV object
---------------------

The RECV command is used to request the reception of files sent from
a designated partner.

### PeSIT E, PeSIT E CFT/CFT

Transfer CFT supports the generic selection of one or more files (for
example, IDF=\*).

If the sending partner is a Transfer CFT monitor, the send transfers
must have been previously prepared via "hold" commands (SEND
STATE=HOLD).

If the receive command is RECV IDF=\*, FILE=ALL, a generic catalog entry
with the ‘K’ state and a diagnosis of DIAG="RECV ALL" is generated.
The transfers on hold (on the partner side) for sending to Transfer CFT
are then released one after the other.

On a GET request, if the Transfer CFT server does not find an outstanding
transfer or an implicit declaration to supply the requester, the error
code is set to 160 (660, "ASE 205" on the requester side). If
the Transfer CFT finds an outstanding transfer or an implicit declaration
but the file does not exist, the error code is set to 110 (610, "ASE
205" on the requester/receiver side).

When issuing a multiple file transfer request, a logical entry is maintained
in the catalog. Until the completion of the request, this entry keeps
the state K and the reason 'RECV ALL'.

When a file has been received in full, the end of transfer procedure
that is defined in the [EXECRF](../../../c_intro_userinterfaces/command_summary/parameter_intro/execrf)
parameter is executed.

- [STATE=K], [DIAGI=660]
    if the first transfer in the remote download returns a code 660. The error
    procedure [EXECRE] is submitted for the transfer in error. This transfer
    can then be resumed.
- [STATE=K], [DIAGI=610]
    if at least one of the file downloads failed, code 610.The failed transfers
    can be resumed. For each failed transfer, the error procedure [EXECRE]
    is submitted. For each transfer that is successfully completed, the end
    of transfer [EXERF] is submitted.
- [STATE=T] indicates
    that the download did not contain any code 610 errors. The last transfer,
    code 660, is automatically deleted from the catalog, without submitting
    the error procedure [EXECRE]. For each transfer, the end of transfer procedure
    [EXECRF] is submitted. The end of transfer procedure [EXECRF] is also
    submitted for the generic request.

The code DIAGI = 160 indicates a "no outstanding transfer or implicit
declaration" type of event. The code 110 indicates that “the file
could not be found”.

### PeSIT E CFT/CFT

The reception in open mode, characterized by the
NFNAME parameter, is an extension specific to Transfer CFT.

The table below summarizes the parameter values authorized as a function
of the various functional levels negotiated for the protocol.


| CFTRECV parameter for<br /> Functional negotiated v  | PeSIT E  | PeSIT E +<br /> Transfer CFT extensions  |
| --- | --- | --- |
| Generic IDF  | X  | X  |
| NFNAME, NFVER<br /> (reception in open mode)  | -  | X  |
| NIDF  | string14  | string32 |
| RAPPL  | string8  | string48  |
| RUSER  | string8  | string28  |
| SAPPL  | string8  | string48  |
| SUSER  | string8  | string28  |


-  X:  supported
- "-":  not supported
