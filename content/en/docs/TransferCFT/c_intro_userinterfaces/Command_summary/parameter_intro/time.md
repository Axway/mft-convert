{
    "title": "time",
    "linkTitle": "time",
    "weight": "3510"
}<span id="time"></span>

### {{< TransferCFT/SystemTitle  >}}

#### CFTCRON

`TIME =   { string &#124; @shutdown &#124; @startup   }`

Starts batch
commands.

- string
- @shutdown - starts a batch procedure after stopping CFT monitor services
- @startup - starts a batch procedure at the end of the {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization
    procedure when {{< TransferCFT/axwayvariablesComponentShortName  >}} starts but before any transfers procedure
