---
    title: "Control commands"
    linkTitle: "Control commands"
    weight: 250
---This section describes the control commands.

- IF

### IF

#### Syntax

The IF command lets you write conditional processing. The IF command must always be coupled with the ENDIF command, and can be nested.

Any commands between the IF and ENDIF commands are executed if the condition of the IF test is true.

An IF statement can only perform numerical tests. You can combine two tests using a logical AND or OR.

```
IF NAME = VAR1,
   VALUE = VALUE1,
   TYPE = COND1
   OPER = OPERATOR,
   ONAME = VAR2,
   OVALUE = VALUE2,
   OTYPE = COND2
```

#### Parameters

- VAR1:The variable No. 1 whose content is tested with the No. 1 value.
- VALUE1: The string representing the value of test number 1.
- COND1: The string indicating the type of test to do on variable number 1. Test types allowed are:
    -   EQU: This tests that VAR is equal to VALUE.
    -   NEQ: This tests that VAR is different than VALUE.
    -   GEQ: This tests that VAR is greater than or equal to VALUE.
    -   GRE: This tests that VAR greater than VALUE.
    -   LES: This tests that VAR is smaller than VALUE.
    -   LEQ: This tests that VAR is smaller or equal to VALUE.
- OPERATOR: The logical operator AND or OR, if present in the test results.
- VAR2: Variable number 2 whose content is tested value number 2.
- VALUE2 chain representing the value of test number 2
- COND2 chain indicating the type of test to be done on the variable number 2. Test types allowed are:
    -   EQU: This tests that VAR is equal to VALUE.
    -   NEQ: This tests that VAR is different than VALUE.
    -   GEQ: This tests that VAR greater than or equal to VALUE.
    -   GRE: This tests that VAR greater than VALUE.
    -   LES: This tests VAR that is smaller than VALUE.
    -   LEQ: This tests that VAR is smaller or equal to VALUE.

#### Example

Using an IF statement for a single numeric test.

```
LONG name = COUNTER, INIT = 0
     IF name = COUNTER, VALUE = 10, type = LES
      PRINT msg = '%I%'
     ENDIF
```

Using IF with an AND condition between two numeric tests.

```
LONG name = I, INIT = 4
    IF name = I,VALUE=5,type=LEQ,OPER=AND,ONAME=I,OVALUE=4,OTYPE=GEQ
     PRINT msg = 'I included between 4 et 5'
    ENDIF
```

```
 
```
