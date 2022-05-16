---

    title: reserv
    linkTitle: reserv
    weight: 2850

---
<span id="reserv"></span>

### reserv

#### CFTEXIT

**\[RESERV = {<u>8192</u> | <u>1024</u> |
n}\]   **{0..8192}
  {0..1024}

Size of the work area reserved for the user exit program. Enter the
size, in bytes.

This field only applies to the following EXITS:

- Access/EXEC:
    -   range {<span style="font-weight: bold;">****0 to 1024****</span>}
    -   default value
        <span style="font-weight: bold;">****1024****</span>
- file:
    -   range {<span style="font-weight: bold;">****0 to 8192****</span>}
    -   default value
        <span style="font-weight: bold;">****8192****</span>

This area is not used by the {{< TransferCFT/axwayvariablesComponentShortName  >}} interface. You can use it
to save the information that you consider necessary for the processing
relative to your program.

[Return to Command index](../../)
