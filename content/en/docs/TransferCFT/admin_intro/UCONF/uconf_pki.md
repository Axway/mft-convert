---

    title: UCONF: PKI and PassPort PS options
    linkTitle: PKI and PassPort PS
    weight: 300

---
This topic presents the parameters to set to define {{< TransferCFT/axwayvariablesComponentShortName  >}} to PassPort PS server connectivity.  You can define this connectivity in {{< TransferCFT/axwayvariablesComponentShortName  >}} using
a command line window.

****PassPort PS parameters****

To enable {{< TransferCFT/axwayvariablesComponentShortName  >}} to PassPort PS server connectivity, use the UCONFSET
command to set the following parameters:

```
CFTUTIL
UCONFSET ID= pki.type, value=”passport”
CFTUTIL
UCONFSET ID= pki.passport.hostname, value=”xxxx”
CFTUTL
UCONFSET ID= pki.passport.port, value=”xxxx”
```

Refer to the [UCONF parameters](../uconf_directory) table for information on the <span class="code">`pki.passport `</span>type parameters.
