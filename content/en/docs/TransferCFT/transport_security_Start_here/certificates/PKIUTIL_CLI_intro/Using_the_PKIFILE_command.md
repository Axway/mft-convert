---
    title: "Using  the PKIFILE command"
    linkTitle: "Using PKIFILE"
    weight: 250
---Use the PKIFILE command to create, purge, or delete a local
certificate database.

****Syntax****

```
PKIFILE

`[MODE =  {REPLACE &#124; CREATE &#124; DELETE},]`

`FNAME =  string,`

```

## Modifying the local certificate database


| FNAME = string1..64 | Name of the local certificate database. |
| --- | --- |
| [MODE = REPLACE &#124; CREATE &#124; DELETE] | Action on the database.<br/> • REPLACE purges the database<br/> • CREATE creates the internal datafile if it does not already exist<br/> • DELETE deletes the database |

