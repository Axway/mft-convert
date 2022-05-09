{
    "title": "Partner  network and protocol connections",
    "linkTitle": "Partner network and protocol connections",
    "weight": "230"
}A SEND or RECV command first results in opening a session with the partner
server. Transfer CFT provides the possibility of defining several partner
access paths: logical paths for protocols and physical paths for network
access methods (network addresses, intermediate partners).

To reach a partner, Transfer CFT (requester) first selects the first
protocol declared in the list (PROT parameter of the CFTPART object) with
which an access point on the remote partner’s site is associated, the
SAP.

This protocol defines both a transfer protocol (TYPE and PROF parameters
for PeSIT of the CFTPROT object) and an access method (NET parameter of
CFTPROT pointing to a CFTNET object), in other words a data exchange protocol
and local network resources.

The partner must therefore be able to be accessed through this access
method with at least one of the resources of this class. Commands such
as CFTTCP define the parameters (network addresses in particular) for
accessing this partner (ID parameter) for this class (CLASS parameter).

The links to be established between the commands to be parameterized
are shown in the figure below.

### Partner connection mechanisms

![](/Images/TransferCFT/Partner_connection_establishing_mechanisms.png)

On the server, the same links have parameters set with the
CFTPART object corresponding to the characteristics of the requester partner.

When the required correspondent cannot be reached via the network facilities
described in the parameter settings, Transfer CFT may trigger the following
mechanisms, depending on the type of error detected:

Retry on all the
resources of a given class  
The CFTNET objects for this type of communication system, for the value
of the CLASS parameter, defined by the first CFTNET object,

<!-- -->

Iterations on the
address  
The RETRYW, RETRYN and RETRYM parameters of the commands
define the number of connection attempts, the interval between attempts
on an address of the access method used,

<!-- -->

Succession of addresses  
If certain partner network definitions have several addresses, each address is used successively as long as the
connection attempts fail,

<!-- -->

Succession of Transfer
CFT protocols  
The successive use of the protocols assigned to the partner, the PROT
parameter of the CFTPART object, as long as the connection fails,

<!-- -->

File store and
forward by the IPART intermediate partner if direct connection is unsuccessful  
The specific file store and forward mechanisms are described in the
next paragraph.

<span id="Application_connection_SAP"></span>

### Application connection SAP

Transfer CFT can authorize several application protocols per partner
(PeSIT, ODETTE). A requester Transfer CFT must consequently address:

- A computer: network
    address
- An application:
    a protocol of the corresponding Transfer CFT

Transfer CFT (server) is seen as several applications by
network access methods (one application per protocol); it thereby designates
its "access points" for incoming protocols, i.e. Service Access
Points (SAP). Each SAP is associated with one and only one Transfer CFT
protocol; so, from the SAP value presented with the connection request,
Transfer CFT can determine the protocol that it has to use.

This general statement,
must be made according to the network access methods used.

More specifically:

- At the requester
    end - remote SAP:
    -   When the
        transfer is activated, the requester Transfer CFT chooses a protocol (PROT
        parameter of CFTPART), and consequently an application protocol and a
        network access method.
    -   The SAP
        parameter of the CFTPART object determines the partner application access
        point.
- At the server end - local SAP:
    -   The SAP
        value allows Transfer CFT to define the associated protocol, by linking
        this value with those of the SAP parameters of the CFTPROT objects
    -   The Transfer
        CFT checks whether this protocol (application and network access
        method) is valid for the requester partner, the CFTPART PROT parameter
    -   The network
        interface informs Transfer CFT of the calls presenting one of the SAP
        parameters that the server Transfer CFT previously defined for it, during
        the initialization phase

****Example of the protocol recognition
in TCP****

This figure illustrates TCP example of the protocol recognition
principle (SAP).

![](/Images/TransferCFT/Protocol_recognition_SAP.png)

In this example:

1. The server call is made in TCP.
1. The user wants to use the PeSIT protocol. For this protocol, the SAP parameter is sent as an TCP port.
1. The server initiates a TCP connection using serveraddr and the port (SAP). The Transfer CFT server defines the protocol (PESIT) on the basis of the SAP value.
1. It checks that the requester is described as supporting this protocol, the PROT parameter of CFTPART, and this network access method (there is a CFTTCP command for the requester).

The figure below provides a schematic overview of the:

- Links to be established
    between the various commands to be set at the requester end
- SAP as a TCP port

****Schematic overview****

![](/Images/TransferCFT/Schematic_overview.png)
