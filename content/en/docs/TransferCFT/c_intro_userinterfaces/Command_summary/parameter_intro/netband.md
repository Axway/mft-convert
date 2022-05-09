{
    "title": "netband",
    "linkTitle": "netband",
    "weight": "2170"
}<span id="netband"></span>

### {{< TransferCFT/SystemTitle  >}}

****CFTSEND, CFTRECV****

****[ NETBAND = { 1...16 } ]****

The outgoing port range is controlled by the CFTNET object SRCPORTS
parameter. There is a maximum of 16 port ranges that can be defined for
this parameter. Selects the outgoing port range:·

- 1 selects the first
    range if any (Default)
- n selects the nth
    range, if that range exists, or the last range

If NETBAND is larger than the number of defined ranges, the last range is used.

****Example****

In the case of SRCPORTS=(6000-6009,6010-6019,6020-6030)·

If NETBAND = 3, the third value is used as the outgoing port range.

 

[Return to Command index](../../)
