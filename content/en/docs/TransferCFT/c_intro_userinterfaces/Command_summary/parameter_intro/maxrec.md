{
    "title": "maxrec",
    "linkTitle": "maxrec",
    "weight": "1920"
}<span id="maxrec"></span>

### maxrec

#### CFTACCNT

****[MAXREC = { <span class="underline">0</span> &#124; n} ]     {0...999999}****

Maximum number
of statistical file records.

- When this value is reached, automatic switching is performed.
- If MAXREC=0 (default), no automatic switching occurs. The only Transfer CFT limit is the limit imposed by the operating system file manager.

#### CFTLOG

****[MAXREC = {<span class="underline">0</span> &#124; n} ]       {0...999999}****

Maximum number of records written in the
log file.

- When this value is reached, automatic switching is performed.
- If MAXREC=0 (default), no automatic switching occurs relative to the
    number of records in the file.

[Return to Command index](../../)
