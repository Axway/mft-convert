{
    "title": " Network  resources ",
    "linkTitle": "CFTNET - Network resources ",
    "weight": "260"
}The
**Network resources** object corresponds to the CFTNET object in the command line operations.

****Related
topics****

- Command syntax
    [CFTNET](../../../c_intro_userinterfaces/command_summary#CFTNET)
- Parameter list
    [CFTNET](../../../c_intro_userinterfaces/web_copilot_ui/conf_intro/cftnet)

<span id="What_is_a_local_network_resource_"></span>

What is a local network resource?
---------------------------------

The local network resources object:

- Defines a network
    resource which, for the {{< TransferCFT/axwayvariablesComponentShortName  >}}, is an entity through which connections
    can be established
- Supplies the Transfer
    CFT with a number of items of information which are required to access
    the network of the type indicated, TCP/IP and so on, through
    a resource

This object includes:

- Parameters used
    to manage the {{< TransferCFT/axwayvariablesComponentShortName  >}} internal facilities: control of connection
    establishing mechanisms for example
- Parameters describing
    the network environment characteristics defined by the local operating
    system
- Parameters used
    to establish connections between:
    -   System
        parameters which can make reference to one or more system software components
        (drivers used at card level, and so on)
    -   Transfer
        CFT

When, for a given system or for a given type of network, the environment
description proves to be too complex or too specific, you should group
the corresponding parameters in a separate configuration file (or initialization
program) which is then referenced by the CFTNET object.

Some of the parameters described below are for general use, while others
are only used by a specific system and/or network access method. The parameters
whose meaning is common to all networks are described in the CFTNET -
Generic command paragraph. The specific parameters, grouped by network
type, are then described in the *CFTNET TYPE = xxx topic*.

The TYPE parameter takes the value TCP/IP.

The check for the maximum number of CFTNET objects managed, performed
during the parameter updating phase, may differ in certain environments
from the check performed during {{< TransferCFT/axwayvariablesComponentShortName  >}} execution. When Transfer
CFT detects an excessive number of CFTNET objects, the following message
appears: CFTP16F
CFTNET id for CFTPARM id_Not loading in memory

<span id="How_does_the_CFTNET_object_work_"></span>

How does the CFTNET object work?
--------------------------------

CFTNET is the network local resource declaration. It is used to define
{{< TransferCFT/axwayvariablesComponentShortName  >}} network resources. CFTNET also supplies the {{< TransferCFT/axwayvariablesComponentShortName  >}}
with information required to access the required network for the type
indicated.

The CFTNET objects define the network parameters for partners in a given
group. This information includes:

- Network
    location of the {{< TransferCFT/axwayvariablesComponentShortName  >}} partner
- Time slots
    for calls on the network
- Connection
    retry mechanisms
- Maximum number
    of connections with each partner

<span id="Defining_a_network_and_protocol_environment"></span>

Defining a network and protocol environment
-------------------------------------------

To define the network and protocol environment, you must create an environment
with the following definitions and links.

These links enable relationships between:

- The resources
    which can be used by {{< TransferCFT/axwayvariablesComponentShortName  >}}

Each of these resources is defined by a CFTNET command.
The associated identifier - ID parameter - has the same value as the one
defined for the NET parameters.

A resource is characterized by:

- Type of
    network accessed
- The protocols
    which can be used by the PROT
    parameter of the CFTPARM object

Each of these protocols is defined by a CFTPROT object.

A CFTPROT object references a class of resources designated
by an identifier NET parameter.

- The network
    description used to connect to a given partner

This description is defined in the CFTTCP object,
where TCP indicates
the type of network. The network
and protocol characteristics used to perform a transfer to a given partner.

These characteristics are referenced in the CFTPART
object such that:

- PROT
    parameter references the CFTPROT used
