---
title: "Catalog querying services "
linkTitle: "Catalog querying services"
weight: 240
---This service provides access to the {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog entries, for
querying and modification, and enables you to sort the selected catalog
entries. Additionally, you can sort the current selection in memory.

> **Note**
>
> The communication structure
> in Transfer CFT enables you to recuperate catalog fields, such
> as an identifier, that exceed 8 to 32 characters.


| Function | Use |
| --- | --- |
| OPEN | Open catalog file<br/> This function:<br/> • Allocates the catalog file<br/> • Opens the file<br/> • Reserves an internal control block<br/> • Initializes the internal block parameter |
| SELECT | Specify the selection criteria<br/> This function:<br/> • Checks the syntax used<br/> • Stores the selection criteria in the internal control block |
| SELECT240 | Specify the selection criteria<br/> This function:<br/> • Is available in CFT v2.4 and higher<br/> • Retrieves identifiers that are longer than 8 to 32 characters<br/> • Checks the syntax used<br/> • Stores the selection criteria in the internal control block |
| NEXT | Read next entry in the catalog<br/> This function:<br/> • Reads the next entry<br/> • Sets the "catalog entry data" area<br/> The first call to this function must be preceded by a SELECT. |
| NEXT240 | Read next entry in the catalog<br/> This function:<br/> • Is available in CFT v2.4 and higher<br/> • Retrieves identifiers that are longer than 8 to 32 characters<br/> • Reads the next entry<br/> • Sets the "catalog entry data" area |
| MODIFY | Modify the state of the current catalog entry or delete this entry from the catalog<br/> This function:<br/> • Retrieves the last entry read from the internal control block<br/> • Checks the state of this entry<br/> • Sends the modification request to {{< TransferCFT/axwayvariablesComponentShortName  >}} |
| SORT | Sort the selected catalog entries<br/> This function:<br/> • Close the catalog file<br/> • De-allocates the file<br/> • Frees the internal control block<br/> • Resets the internal control block parameter |
| DO | Execute the current selection and the requested sort in memory |
| CLOSE | Close catalog file |

