{
    "title": "Configure  the environment",
    "linkTitle": "Configuring the environment",
    "weight": "330"
}This topic describes how to configure the environment for a **file
type exit**. Before you submit an exit, you must customize the following
{{< TransferCFT/axwayvariablesComponentShortName  >}} objects:

- CFTSEND: to define
    the parameters associated with the EXIT on sending the file
- CFTTRECV: to define
    the parameters associated with the EXIT on receiving the file
- CFTEXIT: to describe
    the EXIT environment and how this EXIT is activated

<span id="Defining_the_CFTSEND_CFTRECV_objects"></span>

### Defining the CFTSEND/CFTRECV objects

#### Syntax

```
CFTSEND/CFRECV  
ID = *identifier*,
EXIT = *identifier*,
...
```

#### Parameters

`ID = identifier`

File identifier.

`EXIT = identifier`

File type EXIT identifier.

This EXIT parameter establishes the link with the CFTEXIT object described
below. The identifier can include the symbolic variables &IDF, &GROUP
et &PART.

<span id="Defining_the_CFTEXIT_object"></span>

### Defining the CFTEXIT object

#### Syntax

```
CFTEXIT 
ID = identifier,
TYPE = FILE,
[FORMAT = { V23
&#124; V24 }]
[LANGUAGE = {COBOL &#124; C},]
[MODE = {REPLACE &#124; CREATE &#124; DELETE},]
[PARM = string,]
[PROG = {CFTEXIT &#124; string},]
[RESERV = {8192 &#124; n},]
[WAITTASK = {1441 &#124; n}]
```

****Related topics****

- [CFTSEND](../../../../concepts/cft_configuration_concepts_start_here/default_send_template_concepts)
- [CFTRECV](../../../../concepts/cft_configuration_concepts_start_here/default_receive_template_concepts)
- [CFTEXIT](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftexit)
