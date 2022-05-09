{
    "title": "Partner naming conventions ",
    "linkTitle": "Partner naming conventions",
    "weight": "240"
}The parameter setting relative to partners makes a distinction between
the *network* names and *local* names. This
section describes the naming conventions to use in Transfer CFT.

<span id="Network_names"></span>

### Network names

The network names are the names the scope of which generally
covers all the communicating partners. Only these names are conveyed over
the network, and must consequently comply with the formats and sizes defined
by the protocols used. The parameters describing these names are described in the following table.

****Partner network names****


| Parameter  | Object  | Network name  |
| --- | --- | --- |
| NSPART  | CFTPART  | Name of the local Transfer CFT with regard to the remote partner described by this command  |
| NRPART  | CFTPART  | Name of the remote partner Transfer CFT  |
| NPART  | CFTPARM  | Default name of the local Transfer CFT with regard to the partners (default value of the NSPART parameter)  |


<span id="Local_names"></span>

### Local names

Local names are limited
to the local Transfer CFT, and are recognized as identifiers specific
to the {{< TransferCFT/headerfootervariableshflongproductname  >}}. The parameters describing these names are indicated in
the table below:

****Partner local names****


| Parameter  | Location  | Local name  |
| --- | --- | --- |
| ID  | CFTPART  | Uniquely identifies the partner and supplies the default value of the NRPART parameter  |
| IPART<br /> parameter setting at requester end  | CFTPART  | The local name identifying an intermediate partner (if using store and forward)  |
| IPART<br /> during transfer  | CFT CATALOG  |  • If there is no store and forward: remote partner identifier<br /> <br/> • If there is store and forward: store and forward site identifier (immediate party)  |
| PART  | CFTPARM  | Identifies the local Transfer CFT |
| PART  | CFT CATALOG  |  • If there is no store and forward: remote partner identifier<br /> <br/> • If there is store and forward (see the paragraphs below): store and forward site identifier (immediate party).  |
| SPART  | CFT CATALOG  | Designates the initial sending partner  |
| RPART  | CFT CATALOG  | Designates the final receiving partner  |


<span id="Using_reciprocal_recognition"></span>

### Using reciprocal recognition

Transfer CFT can use the names in NSPART and NRPART to apply reciprocal
recognition of partners over the network. This recognition mechanism works
by comparing the name that is received from a partner with the name recorded
in the Transfer CFT parameters.

****Reciprocal recognition mechanism****

The recognition mechanism is displayed
in the diagram below.

![](/Images/TransferCFT/reciprocal_recognition.gif)

*On  the server*, Transfer CFT
also provides the possibility of checking whether the requester
is authorized to connect to the network. This check compares
the requester’s password, set in NSPASSW and conveyed over the network, against
the password indicated in the NRPASSW parameter on the server.

The checks performed on connection are indicated in the following diagram.

![](/Images/TransferCFT/Checks_performed_on_connecting.gif)

That is, a CFTPART parameter is set for each
partner to be communicated with, where different NRPART parameters correspond
to the identifier of these partners, so each partner must identify itself
with a specific unique network identifier.

This principle is applies to this entire section.

However, for requester operations, a partner with the same NRPART
value shows that you can have several partner descriptions
each having the same NRPART value. This is a special operating mode which
should not be generalized.
