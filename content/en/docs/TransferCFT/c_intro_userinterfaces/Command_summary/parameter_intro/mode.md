{
    "title": "mode",
    "linkTitle": "mode",
    "weight": "2010"
}<span id="mode"></span>

### mode

#### All {{< TransferCFT/axwayvariablesComponentShortName  >}} commands  

****[MODE = { <span class="underline">REPLACE</span> &#124; CREATE &#124; DELETE }]****

Action to do in the parameter or partner database. This parameter applies
to all commands that affect {{< TransferCFT/axwayvariablesComponentShortName  >}} databases. Possible values:

- REPLACE
    (Default value)
- CREATE
- DELETE

> **Note**
>
> Note: Applicable for CFTACCNT, CFTAUTH, CFTCAT, CFTCOM, CFTDEST, CFTEXIT, CFTFILE, CFTIDF,
> CFTLOG, CFTNET, CFTPARM, CFTPART, CFTPROT, CFTRECV, CFTSEND,
> CFTTCP, CFTTRACE, CFTXLATE.

#### DISPLAY

****[MODE = { ANY &#124; COLUMN &#124; LINE } ]****

- ANY / COLUMN: Displays in a column format
- Line: Displays in a more horizontal and spaced format

**** ****

#### TURN

****[MODE = { START &#124; CREATE &#124; ACT &#124; INACT }
]****

- INACT: temporarily stops automated calls to a given site
- ACT: reactivates automated calls after the INACT command
- START: manually triggers a single connection for changing direction
- CREATE: triggers an automatic, cyclic TURN

#### INACT, ACT

******[MODE =
{<span class="underline">BOTH</span> &#124; REQUESTER &#124; SERVER} ]******

Mode to be reactivated:

- BOTH
- REQUESTER
- SERVER

You can use the shortcuts B, R, and S in place of the keywords.

> **Note**
>
> Note: The MODE parameter is absolute. If you run ACT MODE=SERVER followed by
> ACT MODE=REQUESTER, the partner is not reactivated in both modes,
> only in REQUESTER mode (corresponding to the most recent command).

The CFTPART command’s STATE parameter is set to:

- ACTIVEBOTH after
    execution of ACT MODE=BOTH
- ACTIVESERV after
    execution of ACT MODE=REQUESTER
- ACTIVEREQ after
    execution of ACT MODE=SERVER

#### PKIENTITY

****[MODE = { <span class="underline">REPLACE</span> &#124; CREATE &#124; DELETE }]****

Action to do in the PKI database. Possible values:

- REPLACE
    (Default)
- CREATE
- DELETE

[Return to Command index](../../)
