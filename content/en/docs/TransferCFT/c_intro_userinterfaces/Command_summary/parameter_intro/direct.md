---

    title: direct
    linkTitle: direct
    weight: 700

---
<span id="direct_CFTCAT"></span><span id="direct"></span>

### direct

#### SUBMIT, DELETE, END, HALT, KEEP, START, CFTXLATE, CFTETB, LISTCAT

****\[DIRECT = {<span style="text-decoration: underline;">BOTH</span>
| SEND | RECV} \]****

Transfer direction that applies to the table.

The possible values are:

- <span style="font-weight: bold;">****BOTH****</span> - Both send and receive transfers
    are taken into account (default, except for CFTETB)
- <span style="font-weight: bold;">****RECV****</span> - Limits the action to receive
    transfers
- <span style="font-weight: bold;">****SEND****</span> - Limits the action to send transfers

#### DISPLAY, CFTAPPL, RESUME

<span style="font-weight: bold;">****\[DIRECT = {****</span><span style="font-weight: bold;text-decoration: underline;">****CLIENT****</span><span style="font-weight: bold;">****| SERVER} \]****</span>

#### CFTSSL

****\[DIRECT = {<span style="text-decoration: underline;">CLIENT</span>|
SERVER} \]****

Security profile for the client mode.

#### CFTETB

****\[DIRECT = {SEND| RECV} \]****

Transfer direction.

 

[Return to Command index](../../)
