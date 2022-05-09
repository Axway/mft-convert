{
    "title": "name",
    "linkTitle": "name",
    "weight": "2090"
}<span id="name"></span>

### name

#### MQUERY, Object = cache

****NAME = {<span class="underline">CAT</span>
&#124; COMMAND &#124; CRON &#124; DMZ &#124; STAT}****

Name of the component to be queried.
This parameter can be set to:

- ****CAT****: query of the catalog cache
- ****COMMAND****: query of the command cache
- CRON
- DMZ
- STAT

<span id="name_CFTCOM"></span>

#### MQUERY, Object = system

****NAME = { CFTMAIN &#124; CFTTRK &#124; CFTTFIL &#124; CFTCOM &#124; CFTTPRO &#124; CFTEXIT &#124; CFTPRX &#124; CFTDSCAN }****

The name of the process on which the MQUERY command is applied. If NAME is not set, the MQUERY command is sent to all processes.

#### CFTCOM, TYPE = FILE

****NAME = {filename
}    string
64****

This is the communication filename.

[Return to Command index](../../)
