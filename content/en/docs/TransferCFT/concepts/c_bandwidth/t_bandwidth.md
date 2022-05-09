{
    "title": "Managing bandwidth sessions",
    "linkTitle": "Managing bandwidth sessions",
    "weight": "210"
}This topic describes bandwidth session concepts. For parameter details, see the topic [About bandwidth control](../).

In {{< TransferCFT/axwayvariablesComponentShortName  >}}, bandwidth control can occur at the following levels:

- Global
- Class
    -   Partner
    -   Session

The use of minimal dynamic control implies that:

- Netbands can be reconfigured dynamically
- Partners can change netbands, but the new class only applies to new transfers

Every session, as shown here, is linked to a partner. Partner are linked to a netband, which can optionally link to a parent netband.

- n= netband
- p = partner
- s = session

![](/Images/TransferCFT/image.png)

Within a class (netband, partner, session), the bandwidth is shared evenly, and is proportionate to its relative rate. This ensures fairness between sessions for a partner, between partners in a netband, and between netbands in a netband. The following rules apply:

- A maximum rate can be specified for a partner, session, or netband level
- Every session and partner in a given bandwidth class (netband) have the same properties: rate, max-rate (in/out)
- A netband can be specified at each level (SEND/RECV, and transfer properties) (SEND/RECV, CFTSEND/CFTRECV, CFTPART, CFTPROT, CFTNET)
- In server mode, the initial negotiation occurs through the corresponding CFTPROT bandwidth class
- A partner can be present in two bandwidth classes, but in that case the system will act as if two separated partners exist

****Related topics****

- [About bandwidth control](../)
- [Bandwidth use cases](../r_use_cases_bandwidth)
