{
    "title": "Connection  process checks",
    "linkTitle": "Connection process checks",
    "weight": "250"
}This topic describes the Transfer CFT mechanism for performing checks
on connection.

Explicit parameter setting
--------------------------

The notation convention for the local identifier values in the examples
indicated in this paragraph (ID, IPART, PART parameters) begins with the
character ID as its scope is limited to the local Transfer CFT. For example:
IDPARM0, ID_LOCAL, ID_A.

The example below indicates the values of the parameters, defined in
[Partner naming](../partner_naming_conventions), in the parameter setting and catalog to allow parties
to mutually recognize each other.

Recognition mechanism for explicit parameter setting
----------------------------------------------------

![](/Images/TransferCFT/Recongnition_explicit_parameter_setting.gif)

To send a file with a defined IDF ID_EM to partner B, corresponding
command is:

`     SEND     PART       =     ID_B,          IDF       =     ID_EM,          ....`

To receive a file with a defined IDF (ID_REC) from partner B, corresponding
command is:

`     RECV     PART       =     ID_B,          IDF       =     ID_REC,          ....`

Implicit default parameter setting
----------------------------------

If the NSPART parameter of CFTPART is not defined, the corresponding
default value is that of the NPART parameter of CFTPARM. If the NRPART
parameter of CFTPART is not defined, the corresponding default value is
that of the ID parameter of this CFTPART command.

The parameter setting corresponding to the previous example is the one
indicated in the figure below.

Recognition mechanism: NSPART parameter of CFTPART not defined
--------------------------------------------------------------

![](/Images/TransferCFT/NSPART_undefined_CFTPART.gif)

If the NPART parameter of CFTPARM is not defined, the corresponding
default value is that of the PART parameter of this CFTPARM object. The
PART parameter is mandatory for this CFTPARM object.

The parameter setting corresponding to
the previous example is then as follows:

#### Recognition mechanism: PART parameter of CFTPARM not defined

![](/Images/TransferCFT/PART_not_defined_in_CFTPARM.jpg)

#### Using a dynamic partner in server mode

In server mode, Transfer CFT usually only accepts
a transfer request if the partner is pre-defined in the parameter settings.

It is, however, possible to accept connections from a partner not defined
beforehand:

- Either by using
    the mechanism for dynamically creating a partner from a model partner
    (DYNAM = model CFTPROT id)
- Or by generating
    the partner’s characteristics, for a connection, in a directory EXIT

The operating security mechanism checks the validity of the dynamic
partner. For additional information, refer to the Transfer CFT Managing
Security topics.
