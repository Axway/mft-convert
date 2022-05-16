---

    title: About Transfer CFT services in COBOL
    linkTitle: Using Transfer CFT services in COBOL
    weight: 300

---
This book begins with <span style="font-weight: bold;">****this topic****</span>
which provides information about using the {{< TransferCFT/axwayvariablesComponentShortName  >}} services in COBOL.

The programming interface is implemented by the calling application
module link, with the {{< TransferCFT/axwayvariablesComponentShortName  >}} interface function module or modules.

The library of modules supplied provides everything programmers can
require.

This library also contains a programming example and the following COPY
CLAUSE: CFTAPI
to be included in the application which uses the {{< TransferCFT/axwayvariablesComponentShortName  >}} programming
interfaces.

<span id="Call Syntax"></span>

## Call syntax

```
CALL <span style="font-weight: bold;">****CFTx****</span> USING <verb>
<blk>
<param> <rc>
```

Where:

- CFTx indicates:

> -   CFTI:
>     {{< TransferCFT/axwayvariablesComponentShortName >}} catalog querying services
>
> <!-- -->
>
> -   CFTU:
>     transfer services with syntax analysis
> -   CFTC:
>     transfer services without syntax analysis

- &lt;verb> is the  command
    you want to process
- &lt;blk> is the internal control block
- &lt; is a character string of
    variable length that contains the function parameters>param
- &lt;rc> is the return code

The variables described in this documentation are defined in the <span style="font-weight: bold;">****cftapi.cop****</span> file supplied in the library
of delivered modules. The programming example and the corresponding COPY
files are shipped with the product.

## Return codes

The return codes are returned by the programming interfaces in the form
of mnemonics.

> **Note**
>
> It is strongly recommended that you test the return codes of services
> provided by the Transfer CFT programming interfaces through mnemonics,
> the corresponding values being able to change without notice.

The return codes are listed in the <span style="font-family: 'Courier New', monospace;font-weight: bold;">****cftapi.cop****</span>
source file.
