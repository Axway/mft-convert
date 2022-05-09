{
    "title": "Transfer  identifiers",
    "linkTitle": "Transfer object identifiers",
    "weight": "200"
}{{< TransferCFT/axwayvariablesComponentShortName  >}} enables the transferring sequential files, or files seen
as such. These files can be accessed through one of the operating system
access methods . See [File locations: Model and physical files](../../create_transfers_start_here/model_and_physical_file_concepts).

When a transfer occurs, it is labeled with an identifier. There are
four types of identifiers that can correspond with a transfer, as described
below.

- Model
    file identifier
- Message
    identifier
- Transfer
    identifier
- Catalog
    identifier

<span id="Model_files__IDF"></span>

### Model files: IDF

Depending on the type of data to be sent, a model file identifier is assigned to each transfer, for example
IDF = INVOICES. Processing operations and default values for transfer
parameters and data file description parameters can be associated with
a model file identifier.

These processing operations and default values are indicated in the flow definition
CFTSEND and CFTRECV parameter setting commands, for sending and receiving
files respectively.

A CFTSEND and a CFTRECV command may correspond to each model
file. In the absence of these commands for a given transfer, {{< TransferCFT/axwayvariablesComponentShortName  >}} uses the default CFTSEND and CFTRECV command parameters, which are
valid regardless of the IDF.

An identifier corresponds to each flow definition. In the absence of this flow identifier, &lt;span class="mc-variable axway_variables.Component_Long_Name variable"&gt;Transfer CFT&lt;/span&gt; uses the flow default value.
&lt;/p&gt;

The default command is the command whose file identifier corresponds either to the:

- CFTPART
    IDF = parameter
- Or, if the above
    parameter is not included, to the CFTPARM DEFAULT
    = parameter

<span id="Messages__IDM"></span>

### Messages: IDM

A message is a character string specified by a SEND command and sent
by the {{< TransferCFT/axwayvariablesComponentShortName  >}} as a specific transfer. A message transfer does not make reference to a flow definitiona CFTSEND or CFTRECV command.

<span id="Transfer_identifier__IDT"></span>

### Transfer identifier: IDT

The transfer identifier is a label associated with each transfer, time
stamping, for a given partner and transfer direction.

<span id="Catalog_identifier__IDTU"></span>

### Catalog identifier: IDTU

The catalog identifier, IDTU
is a transfer unique local representation. It corresponds to a transfer
counter systematically implemented by Transfer CFT.

A transfer can always be uniquely identified locally, regardless of
the situation - simultaneous transfers, resumption after incidents, and
so on. When there are several {{< TransferCFT/axwayvariablesComponentShortName  >}}s on the same computer, uniqueness
is guaranteed between two {{< TransferCFT/axwayvariablesComponentShortName  >}}s sharing the same catalog.

#### Catalog status according to transfer state

For each of the following states there are certain actions that you can perform, as described in [Processing commands: general usage](../../about_transfer_processing/proc_commands).

- D - Available
- C - In process
- H - Hold
- K - Keep
- T - Terminated
- X - Executed

For more information, see [Transfer states](../delayed_transfers).
