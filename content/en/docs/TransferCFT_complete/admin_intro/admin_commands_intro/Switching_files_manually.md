{
    "title": "Housekeeping for statistical files",
    "linkTitle": "SWITCH - Switch files manually",
    "weight": "330"
}Transfer CFT extracts certain details for each transfer that you perform if you have defined the account (ACCNT) feature. This information is stored in a statistical file if you have defined TYPE = FILE. To manage the contents of this file, you can specify two criteria in the CFTACCNT object to trigger a switch (MAXREC and SWITCH).

#### Automatic SWITCH 

You can customize the switch procedure depending on your production needs. The symbolic variable &FACCNT
represents the statistical file.

> **Note**
>
> Note: At least one of the two files (either the statistical or alternate statistical file) must be empty prior to starting Transfer CFT.

This example automatically executes the switch procedure at noon.

```
CFTACCNT ID=ACCNT0,TYPE=FILE,EXEC=<switch_procedure>,SWITCH=120000
```

#### Manual SWITCH 

To manually execute the switch procedure, use the command:

```
SWITCH TYPE=ACCNT
```
