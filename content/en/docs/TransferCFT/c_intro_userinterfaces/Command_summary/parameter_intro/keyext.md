{
    "title": "keyext",
    "linkTitle": "keyext",
    "weight": "1750"
}<span id="keyext"></span>

### keyext

#### CFTSSL

CFTSSL = VERIFY &#124; NONE

This parameter indicates if the key extensions
for the remote certificate must be checked or not.

- none  - no
    check is performed
- verify - the remote
    certificate is checked as follows:

<!-- -->

- Certificate
    version is greater or equal to 3
- Extended Key
    Usage is ServAuth or ClientAuth according to the mode (Server or Requester)·
- Key Usage is
    digitalSignature, keyEncipherment ou keyAgreement (if Extended Key Usage
    is ServAuth)·
- Key Usage is
    digitalSignature ou keyAgreement (if Extended Key Usage is ClientAuth)
