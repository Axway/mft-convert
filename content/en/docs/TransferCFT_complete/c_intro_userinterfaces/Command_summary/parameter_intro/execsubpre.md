{
    "title": "execsubpre",
    "linkTitle": "execsubpre",
    "weight": "970"
}### execsubpre

#### CFTSEND, SEND

`[ EXECSUBPRE = { LIST &#124; FILE &#124; SUBF } ]`

This parameter defines the execution policy for the PREEXEC script. It determines how the transfer requests are treated, and the order in which this occurs.

- <span class="underline">LIST</span>: (default) triggers the preprocessing procedure first on the generic request, before launching the requests on the list
- FILE: triggers the preprocessing procedure for each request on the list as well as for the generic request
- SUBF: triggers the preprocessing procedure for each request on the list, except for the generic request

[Return to Command index](../../)
