{
    "title": "Dynamic  partner in server mode",
    "linkTitle": "Dynamic partner in server mode",
    "weight": "370"
}In server mode, standard {{< TransferCFT/axwayvariablesComponentShortName  >}} operations consist of accepting
a transfer request only if the partner is pre-defined in the parameter
settings. This topic describes a mode where {{< TransferCFT/axwayvariablesComponentShortName  >}} can accept a
connection from a partner that is not already defined. This type of partner
is referred to as a **dynamic partner**.

{{< TransferCFT/axwayvariablesComponentShortName  >}} can accept connections from a correspondent not defined
in advance by:

- Using the mechanism
    for dynamic creation of a partner from a model partner
- Generating the
    partner’s characteristics in a directory type EXIT task
- Combining the 2
    mechanisms described above

Creating a partner from a model
-------------------------------

For incoming calls of unknown origin, where no local partner description
corresponds to the network name, the user defines a partner CFTPART
by default, in association with the definition of the protocol used CFTPROT.

This feature can be used by all protocols, but is not compatible with
the routing and switching mechanism.

> **Note**
>
> Note: For security reasons, the
> DYNAM = mechanism is rarely used
> without also executing additional checks in a directory EXIT task.

### Parameter settings

As far as parameter settings are concerned, the user sets up the mechanism
by:

- Adding the DYNAM
    parameter to the corresponding to the CFTPROT object
- Creating the definition
    of the model partner CFTPART
- Creating a network
    resource (CFTxxx) associated with the model partner

The DYNAM parameter gives the identifier of the model partner and, by
extension, the identifier of its network description.

The parameters associated with the partner description trigger the usual
checks. It is possible to define several dynamic partners associated with
as many protocols (CFTPROT).

#### Restrictions

A new dynamic partner cannot be created without stopping {{< TransferCFT/axwayvariablesComponentShortName  >}},
as the latter is linked to a protocol definition that cannot be modified.
However, modifications to the parameters of a model partner are immediate.

### Checks

#### CFTPART Partner Parameters

The password given is checked in relation to the (NRPASSW) password
of the model dynamic partner; these two passwords must bear the same value.
As this parameter is optional, it may happen that only one of them is
defined, in which case it is rejected.

The parameters IMINTIME and IMAXTIME, which define the call range to
the {{< TransferCFT/axwayvariablesComponentShortName  >}} are also checked.

### Partner network description

The normal check is performed on the resource class when the connection
is requested.

The connections of each partner are counted in respect of the network
name they give and are checked in relation to the CNXIN and CNXINOUT parameters
of the network description (CFTxxx command). Thus, if CNXIN is equal to
1, 2 partners giving the same network name cannot transfer simultaneously;
conversely, 2 partners giving different network names can transfer simultaneously.

When TCP type network resources (CFTTCP) are defined, the partner’s
number (host) may be checked if the VERIFY parameter is set. It is nevertheless
a good idea to set it to 0 (default value) so that the same model can
be used for several partners.

The parameters IMINTIME and IMAXTIME are also checked.

> **Note**
>
> Note: Apart
> from checking the resource class, the above checks can be made (or remade)
> in the directory type EXIT task, if the latter is defined in the protocol
> description (EXIT parameter of the CFTPROT command).

Putting a file at disposal on hold
----------------------------------

A file is put at disposal, which means on hold, for a partner by a transfer
request of the type:

```
SEND PART = part, IDF = idf, STATE = HOLD
```

Or an equivalent request via CFTAPI ...

As the dynamic partner is not known to {{< TransferCFT/axwayvariablesComponentShortName  >}}, the request is
not taken into account in "H" status, transfer postponed, unless
the parameter settings authorize the use of the dynamic partner and/or
the directory type EXIT task, CFTPROT object DYNAM and EXIT parameters.
If this is not the case, the request is set to "K" state as
in all cases of attempting to make a transfer to a partner that does not
exist.

The dynamic partner can receive the file put on hold, either by:

- Presenting itself
    under this name, or this network identifier
- Using an identifier
    conversion (network, local) provided by a directory type EXIT task
