{
    "title": "About exit  lists",
    "linkTitle": "Exit list",
    "weight": "290"
}An exit list is an optional component supplied with certain Transfer
CFT products. The Exit list is a file exit designed to allow remote partners to consult
the {{< TransferCFT/axwayvariablesComponentShortName  >}} server's catalog. The {{< TransferCFT/axwayvariablesComponentShortName  >}}
Exit list is an EXIT used for consulting the catalog.

The Exit list is not supplied with all {{< TransferCFT/axwayvariablesComponentShortName  >}} products. In addition,
the form in which EXIT is delivered, as object, executable or other code,
depends on the specific product configuration.

![](/Images/TransferCFT/exit_list.png)

The following section explains the high-level steps when using an exit
list.

<span id="Set_the_exit_parameters"></span>

### 1. Set the exit parameters

Activate the exit list by setting the parameters as shown in the example below.

****Remote
requester partner****

No specific parameter setting.

****Server sender site****

```
CFTSEND  ID
= IDFexit,
IMPL = YES,
EXIT = IDExitList
```
```
CFTEXIT  ID
= IDExitList,
LANGUAGE = C,
PARM = FileName,
RESERV = 16384,
PROG = FileExe
```

> **Note**
>
> Note: We recommend that you use the value 16384 for the RESERV
> and not modify this parameter value.

<span id="Prepare_the_selection_criteria_file"></span>

### 2. Prepare the selection criteria file

After you have set the parameters for the sender site,
load the ****selection criteria**** file.
The selection commands contained in this file are described in the section *Selection
criteria file*. You must load this file on the server **before**
requesting the catalog.

The LOGICAL name of the ****Selection
criteria**** must be the CFTEXIT command identifier. For z/OS environments, the correspondence between the physical name and the
logical name must be given by the JCL initiating {{< TransferCFT/axwayvariablesComponentShortName  >}}.

The name of the ****Selection criteria****
file may be defined by  either the:

- Physical name in
    the PARM parameter, *or*

<!-- -->

- Logical name, which
    must be the CFTEXIT object identifier

<span id="Request_catalog"></span>

### 3. Make a catalog request

After loading the file, you can make a catalog
request. When you make a file reception request, use the CFTSEND object
identifier that is associated with the Exit list as the IDF.

****Example****

```
CFTUTILRECV     PART
= &part,
IDF = IDFexit
```

The server:

- Recognizes the
    request for the catalog list using the transfer IDF
- Processes the ****Selection criteria**** file
- Consults the catalog
- Sends the entries
    selected from the catalog to the requesting partner as fixed-length records.
    The selection implicitly chooses only catalog entries concerning this
    partner. This means that
    the Exit list does not allow a partner to consult the transfers sent to
    other partners.
