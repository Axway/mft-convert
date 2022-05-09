{
    "title": "CIPHLIST",
    "linkTitle": "ciphlist",
    "weight": "390"
}<span id="ciphlist"></span>

### CIPHLIST

#### CFTSSL

****[CIPHLIST = {(num, num, ..)}]****

List of the algorithms supported.

Each value defines three algorithms:

- Authentication algorithm
- Encryption algorithm
- Sealing algorithm

****Server****

This list is compared with the list proposed by the client in order of preference, however the {{< TransferCFT/axwayvariablesComponentLongName  >}} order takes precedence over the server ciphlist.

****Client****

This list is submitted to the server which then makes its selection, depending on the client's preference.

[Return to Command index](../../)
