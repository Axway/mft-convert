---
    "title": "impl",
    "linkTitle": "impl",
    "weight": "1660"
---
<span id="impl"></span>

### impl

#### CFTSEND

**[IMPL  = { NO
&#124; YES}]**

Implicit send.

When {{< TransferCFT/axwayvariablesComponentShortName  >}} operates in sender server mode and there is no SEND
command (state=HOLD) entered in the catalog for this file identifier,
the IMPL parameter set to "YES" allows the {{< TransferCFT/axwayvariablesComponentShortName  >}} to make available the corresponding file, by automatically generating
a send request. This makes a file permanently available.

{{% TransferCFT/snippets/implicit_loop%}}

For the default model file,
set this parameter to NO. You can define two
CFTSEND objects with the same ID, with impl
= NO for one and impl = YES
for the other. Do this when the IDF is going to be used for implicit
send rather than explicit send.

[Return to Command index](../../)
