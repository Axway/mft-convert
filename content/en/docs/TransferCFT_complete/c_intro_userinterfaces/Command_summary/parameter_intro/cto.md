{
    "title": "cto",
    "linkTitle": "cto",
    "weight": "560"
}<span id="cto"></span>

### cto

#### CFTPROT, TURN

****Odette (OFTP)****

Minimum duration (in minutes) of the session, Cycle Time Out.

At the end of a transfer, the wait timeout for a new transfer is recalculated
depending on:

- the time (hour) for opening the session
- the current time
- the wait delay before disconnection (DISCTS for
    the protocol)
- the duration of the session (CTO)

The session is liberated if no transfer was initiated by the remote
partner during the indicated duration.

****PeSIT E****

`CTO  =  {1&#124;n}                                    {1….32767}`

 

[Return to Command index](../../)
