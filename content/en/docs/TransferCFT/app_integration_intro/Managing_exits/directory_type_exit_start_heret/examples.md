{
    "title": "Directory  exit examples",
    "linkTitle": "Directory exit examples",
    "weight": "380"
}<span id="Parameter_Settings"></span>

Parameter settings
------------------

With these {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter settings, a directory exit task, name
of the executable module: CFTEXIA, is activated each time Transfer CFT
is started.

Server mode:

- The user will have
    control if the identifier of the communication protocol to be used is
    psithd or psithe
- The user will not
    have control for the ODETTE protocol identifier. Dynamic partner creation
    applies to the psithd protocol identifier

Requester mode, if the partner is:

- Known to Transfer
    CFT, the user will have control if the associated CFTPART card contains
    the psithd or psithe protocol identifier
- Not known to Transfer
    CFT, the user will have control, the communication protocol (which can
    be modified) will have psithd as identifier

#### Example

`/* CFTPARM Card */cftparm     id     =       parm0          ,     part = local          ,     npart = local          ,     prot = (odette,psithd,psithe)          ,     .     .     .     mode = replace/* CFTPROT Cards */cftprot   id   = psithd`

`          type   = pesit`

`          prof   = extern`

`       `

`          sap   = 17501`

`          net   = net0`

`          dynam   = dynptn`

`          exita   = exa`

`          mode   = replace`

` `

`cftprot   id   = odette`

`          type   = odette`

`          sap   = 17502`

` `

`          net   = net0`

`          mode   = replace`

` `

`cftprot   id   = psithe`

`          type   = pesit`

`          prof   = any`

`       `

`          sap   = 17503`

`          net   = net0`

`          exita   = exa`

`          mode   = replace`

` `

`cftexit ..id = exa`

`          parm   = sample`

`          language   = C`

`          prog   = cftexia`

`          type   = access`

<span id="User_program_in_C"></span>

User program in C
-----------------

In this example, you want to take control only if the partner
is a {{< TransferCFT/axwayvariablesComponentShortName  >}} dynamic type, or non {{< TransferCFT/axwayvariablesComponentShortName  >}}.

In server mode, you want to
check the password and address of the calling partner in relation to a
non {{< TransferCFT/axwayvariablesComponentShortName  >}} partner base.

In
requester mode, you want to find in a non {{< TransferCFT/axwayvariablesComponentShortName  >}}
partner base, the name, network, sap and address of the remote partner
as well as the network name and password to give to this partner.

To simplify, all of the bases are loaded in the memory with
the EXIT task.

The user program is delivered with the {{< TransferCFT/axwayvariablesComponentShortName  >}} product as EXAXMP1.C
in the samples file.
