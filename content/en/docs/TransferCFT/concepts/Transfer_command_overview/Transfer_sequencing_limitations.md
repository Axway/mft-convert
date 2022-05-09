{
    "title": "Partner  call limitations",
    "linkTitle": "Partner call  limitations",
    "weight": "230"
}This topic describes the limitations on partner connections.

### Number of connections

For each partner, the CNXIN, CNXOUT and CNXINOUT parameters set the
limits in terms of the number of incoming, outgoing and accumulated connections,
for each partner access method.

### Global time slots

The IMINTIME, IMAXTIME, OMINTIME and OMAXTIME parameters of the CFTPART
command define the time slot for communicating with the partner in question,
for incoming and outgoing calls.

#### Requester mode

In requester mode, the following
cases must be considered:

- OMINTIME = OMAXTIME

This parameter setting is a device for setting the switching by the
intermediate partner (IPART).

- OMINTIME &gt; OMAXTIME

The parameter setting defines a time slot spread over 2 days. Outgoing
calls are authorized each day between OMINTIME and OMAXTIME the next day.

****Example****

OMINTIME = 2200 and OMAXTIME = 0500.  
Outgoing calls authorized to the specified partner at night, between 10
pm and 5 am.

- OMINTIME &lt; OMAXTIME

Outgoing calls are authorized each day between OMINTIME and OMAXTIME.

#### Requester mode

In server mode, the following cases
have to be considered:

- IMINTIME &gt; IMAXTIME

This parameter setting defines a time slot spread over two days. Incoming
calls are authorized each day between IMINTIME and IMAXTIME the next day.

- IMINTIME &lt; IMAXTIME

Incoming calls are authorized each day between IMINTIME and IMAXTIME
