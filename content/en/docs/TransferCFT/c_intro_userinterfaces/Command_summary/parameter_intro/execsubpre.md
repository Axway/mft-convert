---

    title: execsubpre
    linkTitle: execsubpre
    weight: 980

---
### execsubpre

#### CFTSEND

`[ EXECSUBPRE = { LIST | FILE | SUBF } ]`

This parameter defines the execution policy for the PREEXEC script. It determines how the transfer requests are treated, and the order in which this occurs.

- <u>LIST</u>: (default) triggers the preprocessing procedure first on the generic request, before launching the requests on the list
- FILE: triggers the preprocessing procedure for each request on the list as well as for the generic request
- SUBF: triggers the preprocessing procedure for each request on the list, except for the generic request

[Return to Command index](../../)
