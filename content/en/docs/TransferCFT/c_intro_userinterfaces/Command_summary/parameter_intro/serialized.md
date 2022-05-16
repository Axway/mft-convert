---

    title: serialized
    linkTitle: serialized
    weight: 3160

---
### serialized

#### LISTCAT,  DISPLAY

****\[SERIALIZED ={ <u>All</u> | Yes | No } \]****

Filters the LISTCAT or DISPLAY output.

- All (default): Displays all records regardless of the <span class="code">`serial `</span>parameter definition.
- Yes: Displays only those records whose <span class="code">`serial `</span>parameter is set to a phase (Y or Z).
- No: Displays any records that do not have a defined <span class="code">`serial `</span>parameter (the space character).

****Example****

```
CFTUTIL listcat part = paris, Serialized = Yes
```

This example displays all serialized transfer records where the PART\[ner\] is PARIS.

See also, [Serial](../serial) and [Transfer serialization](../../../../app_integration_intro/transfer_serialization).

[Return to Command index](../../)
