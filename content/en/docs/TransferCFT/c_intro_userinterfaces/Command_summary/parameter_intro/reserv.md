---
    title: "reserv"
    linkTitle: "reserv"
    weight: 2870
---<span id="reserv"></span>

### reserv

#### CFTEXIT

**[RESERV = {<u>8192</u> &#124; <u>1024</u> &#124;
n}]   **{0..8192}
  {0..1024}

Size of the work area reserved for the user exit program. Enter the
size, in bytes.

This field only applies to the following EXITS:

- Access/EXEC:
    -   range {****0 to 1024****}
    -   default value
        ****1024****
- file:
    -   range {****0 to 8192****}
    -   default value
        ****8192****

This area is not used by the {{< TransferCFT/axwayvariablesComponentShortName  >}} interface. You can use it
to save the information that you consider necessary for the processing
relative to your program.

[Return to Command index](../../)
