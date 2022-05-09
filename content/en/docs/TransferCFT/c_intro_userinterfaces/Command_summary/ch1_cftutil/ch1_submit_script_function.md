{
    "title": "Submit script function",
    "linkTitle": "Submit script function",
    "weight": "230"
}This section describes how to submit a script.

### _FMSUB

_FMSUB Function starts the execution of a batch file without waiting for the end of the execution of it.

```
_FMSUB PARM = (FILENAME)
RC = lrc
```

#### Parameters

- FILENAME: Character set is the name of the command file to execute.
- String1:
- String2:
- lrc name of a long integer variable that receives return code execution.

#### Examples

```
CHAR NAME = FILE_CMD, SIZE = 13, INIT = CONFIG.CMD
LONG NAME = RSC
_FMSUB PARM = (%FILE_CMD%), RC = LRC
```
