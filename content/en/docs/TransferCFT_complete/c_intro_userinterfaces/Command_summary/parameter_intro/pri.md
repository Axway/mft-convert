{
    "title": "pri",
    "linkTitle": "pri",
    "weight": "2720"
}<span id="pri"></span>

### pri

#### CFTSEND, SEND, CFTRECV, RECV

******[PRI = {<span class="underline">128</span>
&#124; n}]** {0..255}]****

Defines the priority for transfer requests.

Priority works as follows:

1. Among the transfer requests in the catalog waiting for a resource (MAXTRANS, MAXCNX, partner sessions (CNXOUT)), Transfer CFT selects the transfer requests that have the highest priority.
1. Transfer CFT then activates the oldest from among these requests.

See *mintime* for transfer timing details.

- 128
    (default value)
- A value from 0 to 255 (highest priority)

 

[Return to Command index](../../)
