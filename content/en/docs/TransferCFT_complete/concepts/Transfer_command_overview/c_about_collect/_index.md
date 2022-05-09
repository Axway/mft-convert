{
    "title": "About broadcast and collecting",
    "linkTitle": "File transfers",
    "weight": "300"
}Files from several partners can be collected
through a single RECV command, making reference to a list of partners
associated with this collection; this list is defined in the CFTDEST command.

The following illustration displays the relationships
to be established to perform a collection.

********Define a list
of partners********

**![](/Images/TransferCFT/Define_list_of_partner_RECV.gif)**

<span id="Broadcasting_lists"></span>

Broadcast and collection lists
------------------------------

<span id="About_the_Distribution_list"></span>

### About CFTDEST

The {{< TransferCFT/axwayvariablesComponentShortName  >}} object for broadcasting lists, CFTDEST, manages the list of partners for distribution and
collection operations. The list of partners can be described in one of
the following ways:

- Explicitly using
    a list type parameter
- Using a file in
    which the list is saved

These two methods are mutually exclusive. A partner that you include
in a list cannot itself be a broadcasting list.

{{< TransferCFT/axwayvariablesComponentShortName  >}} creates the following items in the catalog:

- A generic entry
    associated with the transfer command
- An entry for each
    partner in the list

> **Note**
>
> Note: The number of entries is not cross-checked against
> the number of partners in the list.

<span id="Definition_of_a_partner_list"></span>

### Partner list definitions

The CFTDEST object is used to specify a pseudo partner that references
a list of partners, in order to perform the following in a single command:

- file (or message)
    broadcasting to several partners

The broadcasting may be activated:

- by a local SEND
    command
- or by a SEND command
    from a remote partner, the local monitor acting as an intermediate site
- or file (or message)
    collection from several partners

The collection is activated by a local RECV command

This list of partners may be described:

- either explicitly,
    using the PART parameter
- or in a file in
    which this list is saved, the name of the file being defined by the FNAME
    parameter

These two methods are exclusive: the PART parameter and the FNAME parameter
may not be used simultaneously.

A partner designated in this list may not itself be a broadcasting list:
recursive partners are not permitted.

The records created in the catalog are:

- on the one hand,
    the record associated with the SEND or RECV command (transferred to this
    pseudo partner)
- and on the other,
    the records associated with the transfers to each partner in the list

When a catalog entry is generated, the number of available entries in
this catalog is not checked relative to the total number of partners in
the list.

Following a correct transfer to a partner, the associated record in
the catalog changes to the T state.

Once ALL the transfers corresponding to each of the partners described
in this list have been CORRECTLY completed, the record associated with
the SEND or RECV command then changes to the T state.

An end-of-transfer procedure can then be executed, provided it has been
defined:

- in the EXEC parameter
    of the SEND or RECV which initiated the transfer
- or (if this EXEC
    parameter is not defined) in the EXECSF parameter (for broadcasting) or
    EXECRF (for collection) of the associated CFTPARM command

The CFTDEST EXEC parameter indicates the procedure submit mode:

- when all transfers
    are terminated (default value)
- at the end of each
    transfer plus when all transfers are terminated

If an incident occurs during a transfer corresponding to one of the
partners in this list, the procedure processing transfer incidents (EXECSE
or EXECRE parameter of CFTPARM) is then executed. The record associated
with this transfer is set to the state corresponding to that of the incident.
The record associated with the SEND or RECV command in this case (an incident
in at least one transfer) remains in the K state.

The maximum number of CFTDEST commands managed is 200.
