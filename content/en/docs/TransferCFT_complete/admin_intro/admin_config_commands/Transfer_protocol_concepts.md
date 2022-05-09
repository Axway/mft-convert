{
    "title": "Transfer  protocol ",
    "linkTitle": "CFTPROT - Transfer protocol ",
    "weight": "280"
}For computers to transfer files, a set of transfer rules must
be established. The CFTPROT object sets up these transfer rules, or file
transfer protocols. Transfer protocols include:

- PeSIT
- Odette

Related
topics

- Command syntax
    [CFTPROT](../../../c_intro_userinterfaces/command_summary#CFTPROT)
- Parameter list
    [CFTPROT](../../../c_intro_userinterfaces/about_cftutil/configuring_cft_start_here/cftprot_command_line)
- [Proxy
    and SOCKS protocol](../../../protocols_start_here/ipv6/use_proxy_and_socks_protocol)

<span id="About_the_CFTPROT_Transfer_Protocol"></span>

What is the CFTPROT object?
---------------------------

The CFTPROT object defines values to make a transfer with a partner
possible. The transfer protocol, either PeSIT or Odette, and
the data exchange protocol (TCP/IP) that you
use, must be supported by the local Transfer CFT and the partner Transfer CFT.

Use the CFTPROT object to:

- Select a protocol,
    either PeSIT or Odette, and a way of using this protocol, such
    as with or without compression, with or without restart possibility, and
    so on
- Specify certain
    parameters related to the use of network resources for protocol exchanges,
    such as the connection protocol selection mechanism (SAP parameter)

The access methods, or data exchange protocols, implemented is TCP/IP, which can only be used between two Transfer CFT
.

The following file transfer protocols are implemented:

- PeSIT
    D (SIT, External and CFT profiles)
- PeSIT
    E (ANY profile)
- ODETTE

A transfer with a partner is possible if the transfer protocol and the
data exchange protocol used are supported by the local Transfer CFT and the
Transfer CFT of the partner concerned and if there is no incompatibility in
operating choices (for example, PeSIT profile, protocol options, etc.).

For Transfer CFT, the association between a partner and the Transfer
CFT protocols which can be used is provided by the PROT parameter of the
CFTPART object.

The maximum number of CFTPROT objects managed by Transfer CFT is 32.

The protocol supported is specified by the TYPE parameter.

The CFTPROT object presents a set of parameters used to define all the
options of all the file transfer protocols supported. Some of these parameters
are common to all protocols; other parameters are only meaningful for
a given protocol, and therefore for a given TYPE. The CFTPROT object defines
the following:

- Parameters, that
    are common to all the protocols as described in the *CFTPROT object*
- Specific parameters,
    grouped by types of protocol, which are described in *CFTPROT TYPE =
    xxx*. The TYPE parameter can take one of the following values:
    ODETTE, PeSIT
- The parameter allowing
    the association of a protocol to a security profile, or SSL, as described
    in [Transport
    Security](../../../transport_security_start_here/configuring_transport_security_start_here)

> **Note**
>
> Note: In general, the
> parameters beginning with an S control the send transfers operation, and
> those beginning with an R control the receive transfers operation. This does
> not necessarily apply when transfers in different directions are performed
> in sequence during the same connection.

****Related topics****

[Transfers via a proxy and SOCKS protocol](../../../protocols_start_here/ipv6/use_proxy_and_socks_protocol)
