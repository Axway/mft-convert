{
    "title": "fast",
    "linkTitle": "fast",
    "weight": "1040"
}### fast

#### SHUT

****[FAST = {<span class="underline">NO</span> &#124; YES &#124; KILL}]****

Type of shutdown:

- YES: Immediate shutdown of {{< TransferCFT/axwayvariablesComponentShortName  >}}.
    All the transfers in process are interrupted and change
    to the D state. No pending transfer is activated.

<!-- -->

- NO: {{< TransferCFT/axwayvariablesComponentShortName  >}} completes all the transfers
    in process and shuts down. No new transfer is initialized.
- KILL: Immediate {{< TransferCFT/axwayvariablesComponentShortName  >}} shutdown occurs
    but without updating the transfer states.

[Return to Command index](../../)
