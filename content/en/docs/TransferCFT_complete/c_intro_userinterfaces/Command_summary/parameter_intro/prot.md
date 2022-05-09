{
    "title": "prot",
    "linkTitle": "prot",
    "weight": "2760"
}<span id="prot"></span>

### prot

#### CFTPART

**PROT = (*identifier* &#124; *mask,* *identifier* &#124; *mask*,..)**

List of communication protocols (CFTPROT ID identifiers) authorized
to communicate with this partner.

The maximum number of protocols supported is 4.

For simultaneous transfers with this partner, {{< TransferCFT/axwayvariablesComponentShortName  >}} requires all the transfers to be performed according to the same protocol.

The value of the PROT parameter may include "wildcard" characters
(mask). This syntax is only used in server mode. This means that the {{< TransferCFT/axwayvariablesComponentShortName  >}}
will accept any protocol, the identifier of which corresponds to the generic
value thereby defined.

****Example 1****

PROT = (prot1 , \*)

****Example 2****

PROT = (prot1 , prot?)

The ‘?’ character designates the char_mask character defined in the
{{< TransferCFT/axwayvariablesComponentShortName  >}} Operations Guide corresponding to your operating system.

*In requester mode*: only the prot1
protocol is used.

*In server mode*, the communication
may be established:

in the first example, by all the protocols defined at the level of the
monitor

in the second example, by all the protocols beginning with the letters
"prot"

#### CFTPARM

**PROT = (*identifier,identifier*,..)**

#### SEND / RECV

******Requester mode only******

**PROT = (*identifier*)**

Enter an identifier of up to 32 characters.

This parameter allows you to chose one of the protocols defined for the partner (CFTPART) when using the SEND or RECV command. If the PROT parameter is not among the protocols defined in the partner's protocols list (CFTPART), the transfer is rejected and an error message displays. For any attempts by the transfer to reconnect, the retry is only for this same protocol - meaning that it does not retry for other protocols. The transfer is rejected when the maximum number of retries is reached.

[Return to Command index](../../)
