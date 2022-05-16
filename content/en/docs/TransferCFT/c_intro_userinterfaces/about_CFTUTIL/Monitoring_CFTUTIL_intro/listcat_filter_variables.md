---

    title: LISTCAT/DISPLAY - Statistical variables
    linkTitle: LISTCAT - Statistical variables
    weight: 320

---
Use the following variables after performing a LISTCAT or DISPLAY command to show the selected statistical information. For example, LISTCAT\_FREE displays the percentage of free records. Additionally, you can use a combination of these variables.

- \_LISTCAT\_SELECTED: displays the selected record(s) from the original LISTCAT command
- \_LISTCAT\_CAT: displays the total number of records
- \_LISTCAT\_FREE: displays the number of available records
- \_LISTCAT\_FREE\_P: displays the number of available records as a percentage of the total number of records

**Example**

```
CFTUTIL
...
LISTCAT
PRINT MSG='NbSelected = %_LISTCAT_SELECTED%'
PRINT MSG='NbTotal = %_LISTCAT_CAT%'
PRINT MSG='NbFree = %_LISTCAT_FREE%'
PRINT MSG='Percent free= %_LISTCAT_FREE_P%'
 
 
**Results**
------------------------------------
BCLPM SFT XX TGTG E0216242 1 1 0 CP 88%
BCLPRM RFT XX TGTG E0216242 1 1 0 CP 88%
2 record(s) selected
1000 record(s) in Catalog file
998 record(s) free (99%)
CFTU00I LISTCAT _ Correct ()
NbSelected = 2
CFTU00I PRINT _ Correct (MSG='NbSelected = 2')
NbTotal = 1000
CFTU00I PRINT _ Correct (MSG='NbTotal = 1000')
NbFree = 998
CFTU00I PRINT _ Correct (MSG='NbFree = 998')
Purcent free= 99
CFTU00I PRINT _ Correct (MSG='Percent free= 99')
```
