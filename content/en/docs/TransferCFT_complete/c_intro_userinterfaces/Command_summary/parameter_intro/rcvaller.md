{
    "title": "rcvaller",
    "linkTitle": "rcvaller",
    "weight": "2840"
}<span id="rvcaller"></span>

### rcvaller

#### CFTPARM

****[RCVALLER = { STOP &#124; CONTINUE }]****

Action to perform if an error occurs when receiving available files
(with the FILE= ALL option).

- STOP: If an error occurs, regardless of the issue, the transfer stops
- CONTINUE:
    -   Transfers continue even if a DIAGI error between 600 and 654 occurs, or if a protocol selection error DIAGI=730 or DIAGP=ASE 39 occurs
    -   Otherwise, the transfer stops

[Return to Command index](../../)
