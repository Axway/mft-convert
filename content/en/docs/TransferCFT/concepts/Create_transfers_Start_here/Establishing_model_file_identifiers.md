{
    "title": "Establishing  model file identifiers",
    "linkTitle": "Establishing model file identifiers",
    "weight": "240"
}The mechanisms described in [Model file and physical file
concepts](../model_and_physical_file_concepts) assume an equality between the local IDF and the
IDF agreed upon between the partners.

If for some reason the partners are not able to agree
on a common value for the local identifier and remote identifier of the
model file to be transferred, the {{< TransferCFT/axwayvariablesComponentShortName  >}} provides a way
for establishing the correspondence between these identifiers. This is
based on the concept of an NIDF network identifier conveyed over the network.

This topic describes the model file identifier, IDF, processes in the
following states:

- Sender requester
- Receiver server
- Receiver requester
- Sender server
- Receiver/Requester
    in selective-receive mode

This mechanism, for establishing an IDF, is based on the principle described
in the figure below.

**Mechanism principle**

![](/Images/TransferCFT/Mechanism_principle.gif)

 

The correspondence between the locally known IDF and the NIDF sent or
received can be established:

- In the SEND or
    RECV transfer command through the use of the NIDF parameter
- In the CFTIDF object
    for a given partner and direction of transfer
- At the level of
    the server, in the CFTPROT object, the local server IDF being able to
    be deduced from the NIDF received

The mechanisms used to establish the NIDF/IDF correspondence are described
according to the function provided by {{< TransferCFT/axwayvariablesComponentShortName  >}} and *in
the order of priority* in which they are implemented:

- Sender/requester
- Receiver/server
- Receiver/requester
- Sender/server

<span id="Sender_requester"></span>

Sender requester
----------------

If the NIDF parameter is present in the SEND command, the value of the
NIDF which is sent when transferring the model file locally identified
by IDF, is the one defined in this parameter.

**NIDF in the SEND command**

![](/Images/TransferCFT/NIDF_SEND.gif)

If there is a CFTIDF command associated with the partner, the NIDF value
that is sent on transferring the model file identified by LOCALD is the
one defined in the NIDF parameter of this CFTIDF command.

**CFTIDF corresponding to the local IDF**

![](/Images/TransferCFT/CFTIDF_corrspnd_local_IDF.gif)

The value of the NIDF parameter of the SEND command takes precedence
over the value of the NIDF parameter of the CFTIDF command, as shown in
the following figure.

**Precedence of the SEND NIDF over
the CFTIDF command NIDF parameter**

![](/Images/TransferCFT/NIDF_SEND_NIDF_CFTIDF.gif)

If there is no CFTIDF command or NIDF parameter in the SEND command,
the value of NIDF sent is the one defined by the local IDF identifier
of SEND. This case has already been described in the section *Model
file identifier (IDF)/physical file (FNAME)*.

**Default NIDF**

![](/Images/TransferCFT/Default_NIDF.gif)

<span id="Receiver_server"></span>

Receiver server
---------------

On receiving an NIDF, {{< TransferCFT/axwayvariablesComponentShortName  >}} checks whether there is a CFTIDF
object associated with the sender partner (in this example DEM) and relating
to this transfer direction, the NIDF parameter value of which is the same
as the one received. If so, the associated local IDF is equal to the identifier
of this CFTIDF command.

**CFTIDF command with the same NIDF as the
one received**

![](/Images/TransferCFT/CFTIDF_with_same_NIDF_as_received.gif)

If there is no associated CFTIDF, {{< TransferCFT/axwayvariablesComponentShortName  >}} checks if the IDF parameter
has been set in the CFTPROT command for the reception protocol used. If
so, the local IDF is equal to the value of this IDF parameter.

You can define the value of the IDF parameter of CFTPROT using the substring
mechanism applied to the [symbolic
variables](../../../c_intro_userinterfaces/command_summary/symbolic_variables) &NIDF that is used to recover
the value of the NIDF received.

The value of the IDF parameter of CFTPROT may also be defined with an
explicit value.

**Local IDF assigned using the CFTPROT IDF
for a received NIDF**

![](/Images/TransferCFT/LoIDF_assgnd_IDF_PROT_NIDF_recd.gif)

If {{< TransferCFT/axwayvariablesComponentShortName  >}} does not find a CFTIDF command having the same NIDF
value as the one defined by the NIDF received, nor an IDF parameter in
the associated CFTPROT command, it assigns the value of NIDF for the local
IDF.

If there is no such command with an identifier IDF, {{< TransferCFT/axwayvariablesComponentShortName  >}} associates
the reception with the command CFTRECV ID = &lt;*default*&gt;, where
the value &lt;*defaul*t&gt; is the value of the DEFAULT parameter
of CFTPARM.

Before using the default CFTRECV value, {{< TransferCFT/axwayvariablesComponentShortName  >}} checks if the IDF
is defined in the CFTPART command. For more information see [IDF
CFTPART](../../../c_intro_userinterfaces/command_summary/parameter_intro/idf).

**Local IDF assigned by default for a received
NIDF**

![](/Images/TransferCFT/LoIDFassgnd_by_default_for_NIDFrec.gif)

<span id="Receiver_requester"></span>

Receiver requester
------------------

****This
scenario is only applicable for the protocols PeSIT D CFT profile, PeSIT E.****

If the NIDF parameter is present in the RECV command, the NIDF value
that is sent on the request to receive the model file locally identified
by IDF is the one defined in this parameter.

**NIDF in RECV**

![](/Images/TransferCFT/NIDF_in_RECV.gif)

If there is a CFTIDF command associated with the partner (server called
in the SERV example), the NIDF value that is sent on the request to receive
the model file locally identified by IDF is the one defined in the CFTIDF
command NIDF.

**CFTIDF command**

![](/Images/TransferCFT/CFTIDF_command.gif)

The value of the NIDF parameter of the RECV command takes precedence
over the CFTIDF command NIDF.

**Precedence of the NIDF parameter of the
RECV command over CFTIDF**

![](/Images/TransferCFT/NIDF_RECV_CFTIDF.gif)

If there is no CFTIDF command or NIDF parameter in the RECV command,
the value of the NIDF sent is the one defined by the IDF local identifier
of RECV. This case has already been described in the section *Model
file identifier (IDF)/physical file (FNAME)*.

**Default NIDF**

![](/Images/TransferCFT/Default_NIDF.gif)

<span id="Sender_server"></span>

Sender server
-------------

****This
scenario is only applicable for the protocols PeSIT D CFT profile, and PeSIT E.****

On receiving an NIDF, {{< TransferCFT/axwayvariablesComponentShortName  >}} checks to see if there is a locked
SEND command having an NIDF parameter with the same value as the received
NIDF. If so, the associated local IDF is equal to the identifier of this
command.

**Locked SEND command with NIDF parameter
equal to the NIDF received**

![](/Images/TransferCFT/Lock_SEND_NIDF_equal_NIDF_rec.gif)

If no such locked SEND command is found, {{< TransferCFT/axwayvariablesComponentShortName  >}} checks to see
if there is a CFTIDF command associated with the partner that made the
receive request (DEM in the example) and relating to this transfer direction,
the NIDF parameter value of which is the same as the one received. If
so, the associated local IDF is equal to the identifier of this CFTIDF
command.

If there is a locked SEND command having the same local IDF (with an
undefined NIDF), the transfer relates to this IDF and the command is unlocked.
If not and there is a command CFTSEND IMPL = YES having the same local
IDF, this command activates the sending of this local IDF.

If no locked SEND command or CFTSEND IMPL = YES corresponds to this
local IDF, the server informs the requester that no file associated with
the requested NIDF is available for sending.

**CFTIDF command associated with the received
NIDF**

![](/Images/TransferCFT/CFTIDF_assoc_NIDF_recvd.gif)

If a locked SEND command is found with an NIDF parameter having the
same value as the one received, a CFTIDF command having the same NIDF
value is not take

If there is no CFTIDF object associated with the CFTIDF received, Transfer
CFT checks to see if for the protocol used for the reception request,
the IDF parameter has been set in the associated CFTPROT. If so, the local
IDF is equal to the value of this IDF parameter. The value of the IDF
parameter of CFTPROT may be defined using the substring mechanism applied
to the symbolic variable &NIDF which is used to recover the value
of the NIDF received. This mechanism is described in the *Symbolic variables*
paragraph. The value of the CFTPROT IDF parameter may also be defined
with an explicit value.

{{< TransferCFT/axwayvariablesComponentShortName  >}} then checks whether there is a locked SEND command having
the same local IDF, with an undefined NIDF. If this is the case, the corresponding
send transfer is performed. If not, if checks whether there is an implicit
CFTSEND object having the same local IDF and in this case, the corresponding
send transfer is performed.

If no locked SEND command or CFTSEND IMPL = YES command corresponds
to this local IDF, the server informs the requester that no file associated
with the requested NIDF is available for sending.

**Local IDF assigned using the CFTPROT IDF
for a received NIDF**

![](/Images/TransferCFT/LoIDFassgndCFTPROT_IDF_for_NIDFrec.gif)

<span id="Receiver_Requester_in_selective_receive_mode"></span>

Receiver/Requester in selective-receive mode
--------------------------------------------

**This
scenario is only applicable between two {{< TransferCFT/axwayvariablesComponentShortName  >}}s using
one of the protocols - PeSIT D CFT profile, or PeSIT E.**

During a selective receive transfer, the NIDF sent by the receiver requester
(during a SELECT REQUEST) is equal to the value of the generic IDF even
if the NIDF parameter was defined in the RECV command.

The server unlocks the sending of the files defined in the SEND STATE
= HOLD commands having:

- An NIDF parameter
    value corresponding to the generic IDF mask requested
- Or, if this NIDF
    parameter is not defined, an IDF value corresponding to this mask

The value of the NIDF received by the receiver/requester on this reception,
is equal to the value defined for the local IDF in the SEND locked at
the level of the server.

On receiving this IDF, the requesting monitor checks whether there is
a CFTIDF object having the same NIDF. If this is the case, the local IDF
at the requester end is equal to the value of the ID parameter of this
CFTIDF.

This local IDF may be used to assign a name to the file received as
indicated in the following example.

**Requester end CFTIDF with an NIDF equal
to the received NIDF received -  server
end has unlocked transfers**

![](/Images/TransferCFT/CFTIDF_NIDF_equal_NIDF_rec_serverunlocked.gif)

If there is no CFTIDF command, {{< TransferCFT/axwayvariablesComponentShortName  >}} checks to see if, for the
protocol used for the reception request, the IDF parameter has been set
in the associated CFTPROT command.

If so, the local IDF is equal to the value of this IDF parameter. The
value of the IDF parameter of CFTPROT may be defined using the substring
mechanism applied to the symbolic variable &NIDF which is used to
recover the value of the NIDF received.

See also [Symbolic
variables](../../../c_intro_userinterfaces/command_summary/symbolic_variables).

The value of the CFTPROT IDF parameter may also be defined with an explicit
value.

In the example below, the local IDF value is defined by the substring
mechanism. The corresponding character string is the 2-character sub-string
extracted from the 11th character onwards of the NIDF string received.

**Local IDF defined by the CFTPROT IDF**

![](/Images/TransferCFT/LoIDF_by_PROT_IDF.gif)

If there is neither a CFTIDF command nor an IDF parameter defined in
CFTPROT, the local IDF takes the NIDF value.

****Local IDF defined by default for the received
NIDF****

![](/Images/TransferCFT/UNDEF_predefined_value.gif)
