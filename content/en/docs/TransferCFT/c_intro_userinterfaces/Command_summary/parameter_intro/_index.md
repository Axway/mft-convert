---

    title: Transfer CFT  parameter index
    linkTitle: Parameter index
    weight: 200

---
All {{< TransferCFT/axwayvariablesComponentShortName  >}} parameters are listed in alphabetical order. Some parameters have different possible values depending on the object. Parameter
definitions and values are listed by object name when this applies to
a parameter.

Each parameter topic displays the command line format, a definition,
limitations, and default values when applicable.

****Example****

### acb parameter (heading)

#### CFTNET  (command where the parameter is used)

<span class="code">`[ACB = {ID value for this CFTNET   | string}] `</span> ( parameter
format, where the underlined value is the default value)

The resource identified to the access method. The default value is defined
by the ID value for the CFTNET object. (the parameter
definition)

<span id="CFT_naming_conventions"></span>

## {{< TransferCFT/axwayvariablesComponentShortName  >}} naming conventions

Respect the following conventions when completing values for Transfer
CFT object fields:

- <span style="font-weight: bold;">****Date****</span>:
    8-digit string in the YYYYMMDD format
- <span style="font-weight: bold;">****File
    name****</span>: 512 characters comprised of the full path - meaning the drive, path, root, and suffix
    -   The file system or operating system however imposes limitations such as unauthorized characters, length restrictions, and case sensitivity
- <span style="font-weight: bold;">****Identifier****</span>:
    alphanumeric string of 1 to 32 characters
- <span style="font-weight: bold;">****Mask****</span>
    : string containing wildcard characters (\* and ?)
- <span style="font-weight: bold;">****Time****</span>:
    2 to 8-digit string in the HHMMSSSS

[Return to Command index](../)
