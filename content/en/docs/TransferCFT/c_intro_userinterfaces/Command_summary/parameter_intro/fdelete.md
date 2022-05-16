---

    title: fdelete
    linkTitle: fdelete
    weight: 1130

---
### fdelete

#### CFTSEND/CFTRECV

****\[FDELETE = “ “ | “\*” | C | D | K | H | T | X \]****

Use FDELETE to enable file deletion based on the transfer state when deleting the catalog record transfer (using the DELETE command or the PURGE procedure).

FDELETE indicates the transfer statuses that can be deleted:

- “ “ default: The file is not deleted with the catalog record transfer.
- “\*”all statuses: The file is always deleted with the catalog record transfer.
- “CDKHTX”: Any combination of these states is possible. The file is deleted along with the catalog record if the transfer belongs to the state defined in this field.

[Return to Command index](../../)
