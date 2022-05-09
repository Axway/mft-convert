{
    "title": "CFTIDF  - Template to virtual file association",
    "linkTitle": "Template/virtual file association - CFTIDF",
    "weight": "240"
}<span id="About_the_CFTIDF_Command"></span>You can use this command to locally establish this correspondence
between the local IDF and the NIDF sent or received.

****Related
topics****

- Command syntax
    [CFTIDF](../../../command_summary#CFTIDF)
- Object concepts
    [Template
    to virtual file association](../../../../concepts/cft_configuration_concepts_start_here/network_file_identifier_concepts)


| Parameter  | Description  |
| --- | --- |
| [ID](../../../command_summary/parameter_intro/id) | Local identifier of the file on the local site (IDF).<br/> This identifier corresponds to the value of the ID parameter of the CFTSEND/CFTRECV command or to the IDF parameter of the SEND/RECV command. |
| [NIDF](../../../command_summary/parameter_intro/nidf) | File network identifier; value which is sent over the network.<br/> It is not possible, for the direction of a given transfer, to have multiple CFTIDF commands with an identical NIDF.<br/> Note: In standard PeSIT E, the NIDF is transported in the PI 12 (14 characters maximum). In PeSIT E between 2 Transfer CFTs, if the NIDF is longer than 14 characters, this NIDF is transported in the PI 99 (28 characters maximum), the value indicated in the PI 12 being truncated to 14 characters.<br/> string14PeSIT E<br/> string28 PeSIT E CFT/CFT<br/> string26 ODETTE |
| [PART](../../../command_summary/parameter_intro/part) | Local identifier of the partner for which the IDF/NIDF correspondence is valid.<br/> Same value as the value of the ID parameter of CFTPART. |
| [TYPE](../../../command_summary/parameter_intro/type) | Transfer direction for which this correspondence is valid.<br/> The values indicated are:<br/> • SEND for send transfers<br/> • RECV for receive transfers |

