{
    "title": "About Transfer CFT services in COBOL",
    "linkTitle": "Using services in COBOL",
    "weight": "320"
}This book begins with ****this topic****
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

Call syntax
-----------

```
CALL CFTx
USING <verb>
<blk>
<param> <rc>
```

Where:

- CFTx indicates:

> -   CFTI:
>     {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog querying services
>
> <!-- -->
>
> -   CFTU:
>     transfer services with syntax analysis
> -   CFTC:
>     transfer services without syntax analysis

- &lt;verb&gt; is the  command
    you want to process
- &lt;blk&gt; is the internal control block
- &lt; is a character string of
    variable length that contains the function parameters&gt;param
- &lt;rc&gt; is the return code

The variables described in this documentation are defined in the ****cftapi.cop**** file supplied in the library
of delivered modules. The programming example and the corresponding COPY
files are shipped with the product.

Return codes
------------

The return codes are returned by the programming interfaces in the form
of mnemonics.

> **Note**
>
> Note: It is strongly recommended that you test the return codes of services
> provided by the Transfer CFT programming interfaces through mnemonics,
> the corresponding values being able to change without notice.

The return codes are listed in the ****cftapi.cop****
source file.
