{
    "title": "Specific  network functions",
    "linkTitle": "Specific network functions",
    "weight": "250"
}This topic presents the TCP network supported by {{< TransferCFT/axwayvariablesComponentShortName  >}} Windows,
and how to define the network parameters.

<span id="Supported_networks"></span>

Supported networks
------------------

TCP/IP  is supported for Transfer
CFT Windows.

You
should consult the README file that is delivered with the product for
more up to date information. If there is a contradiction between the README
file and this document, the README file information takes precedence.

The network descriptions in this section are not guaranteed to be exhaustive.
Additionally, the logical key can limit the maximum number of transfers.

<span id="Defining_network_parameters"></span>

Defining network parameters
---------------------------

You implement network functions by entering parameters into
a single file, `cftnet.conf`, located in the {{< TransferCFT/axwayvariablesComponentShortName  >}} `runtime\conf`
folder.

This file is made up of lines using the same syntax, each
of which corresponds to one function: typenet&lt;parameter&gt;=value,
where:

- typenet:
    is an element taking on one of the following values:
    -   TCP:
        A TCP/IP network process parameter
    -   &lt;parameter&gt;: An element containing the value of one of the specific parameters indicated
        in this documentation. For more information see Environment
        Variables
    -   value: An element that takes on a value belonging to the parameter stated
        in field and according to the documentation

****Comments****

To edit a line of comments in the file `CFTNET.CONF`, you can
place the ‘\#’ character in the first column of this line.

****Example****

`# this is a comment`

Indication of the path for the cftnet.conf
file

****Environment variable****

`CFTCFGPATH`

Environment variable defining the sub-folder where the cftnet.conf
file is located. By default, {{< TransferCFT/axwayvariablesComponentShortName  >}} searches for this file in the
application default folder.

{{< TransferCFT/axwayvariablesComponentShortName  >}} must be stopped when the cftnet.conf file is
created or modified.
