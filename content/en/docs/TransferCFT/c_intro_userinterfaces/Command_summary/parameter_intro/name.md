---

    title: name
    linkTitle: name
    weight: 2080

---
<span id="name"></span>

### name

#### MQUERY, Object = cache

****NAME = {<u>CAT</u>
| COMMAND | CRON | DMZ | STAT}****

Name of the component to be queried.
This parameter can be set to:

- <span class="bold_in_para">****CAT****</span>: query of the catalog cache
- <span class="bold_in_para">****COMMAND****</span>: query of the command cache
- CRON
- DMZ
- STAT

<span id="name_CFTCOM"></span>

#### MQUERY, Object = system

****NAME = { CFTMAIN | CFTTRK | CFTTFIL | CFTCOM | CFTTPRO | CFTEXIT | CFTPRX | CFTDSCAN }****

The name of the process on which the MQUERY command is applied. If NAME is not set, the MQUERY command is sent to all processes.

#### CFTCOM, TYPE = FILE

****NAME = {filename
}    string
64****

This is the communication filename.

[Return to Command index](../../)
