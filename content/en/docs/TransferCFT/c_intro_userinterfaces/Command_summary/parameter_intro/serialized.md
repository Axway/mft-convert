{
    "title": "serialized",
    "linkTitle": "serialized",
    "weight": "3180"
}### serialized

#### LISTCAT,  DISPLAY

****[SERIALIZED ={ <span class="underline">All</span> &#124; Yes &#124; No } ]****

Filters the LISTCAT or DISPLAY output.

- All (default): Displays all records regardless of the `serial `parameter definition.
- Yes: Displays only those records whose `serial `parameter is set to a phase (Y or Z).
- No: Displays any records that do not have a defined `serial `parameter (the space character).

****Example****

```
CFTUTIL listcat part = paris, Serialized = Yes
```

This example displays all serialized transfer records where the PART[ner] is PARIS.

See also, [Serial](../serial) and [Transfer serialization](../../../../app_integration_intro/transfer_serialization).

[Return to Command index](../../)
