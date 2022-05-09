{
    "title": "Starting Transfer CFT in batch mode",
    "linkTitle": "Start the Transfer CFT IBM i Manager",
    "weight": "270"
}The CFTSTART program is used to start Transfer CFT IBM i without any required user action.

CFTSTART comprises:

```
ADDLIBLE LIB(CFTPGM) POSITION(\*FIRST)
ADDLIBLE LIB(CFTPROD) POSITION(\*FIRST)
CALL PGM(CFTSTART)
RMVLIBLE LIB(CFTPGM)
RMVLIBLE LIB(CFTPROD)
```
