{
    "title": "Internal commands",
    "linkTitle": "Internal commands",
    "weight": "200"
}The following sections describe the available internal commands. These internal commands are divided into four categories:

- Data processing: These commands are used to define and manipulate data (Assignments, Operations).
- Control interpretation: When inserted in the source text, these commands control its progress.
- Functions: Functions are commands that apply directly to the operating system, such as allocating a file. The syntax of a function expression, while respecting the general rules, is based on a single model. See [Functions](../ch1_functions).
- Utilities: The utility commands are high-level commands that use an internal sequence of functions, for example, displaying the contents of a file.

When describing a command in the following sections, the mandatory parameters appear in bold, and parameters in normal font are optional.

Data processing
---------------

A data item is a form of coded information. It is referenced by a keyword, which must be unique for all of the data. To improve the readability of the source text, the characters _ (underscore) and $ (dollar) can be used in the keyword.

Variables are globally visible. Once a variable is defined, it is available even if it nested in command files.

Declaring variables
-------------------

### INT

```
INT  NAME = NAME,
             INIT = VALUE,
```

### LONG

```
LONG NAME = NAME,
     INIT = VALUE
```

### CHAR

```
CHAR NAME = NAME,
     SIZE = SIZE,
     INIT = VALUE
```

### Parameters

- NAME: This parameter specifies the name assigned to the data.
- VALUE: This setting provided the initial value of the data. If the variable is numeric it is a number, if the variable is a CHAR is a string that can be in quotes.
- SIZE: This parameter indicates  the size only for the data type CHAR.

### Features

Variable names are not case sensitive, and you cannot the characters . (dot) and [ (left bracket) in a name. This restriction comes from redefined data notation (see REDEFINE command) or the principle of substitution data.

- The initial default values ​​(in the absence of parameter INIT) types INT and LONG are 0.
- For the data type CHAR, the size must be less than or equal to 256 to be initialized. This limitation comes from the size of the INIT parameter .
- The size of the NAME parameter is limited to 26 characters.

### Examples

Declaration of a string data type, named CHR1. This data is initialized, and its size is 15 characters.

```
CHAR    name =CHR 1, init = 'INITIAL VALUE'
```

Declaration of a string data type, named CHR2 that has a size of 4096. The data is not initialized.

```
CHAR    name =CHR2 , size = 4096
```

Declaration of a given integer named INT1, which is initialized to the value 3425.

```
INT     name =INT1, init = 3425
```

Declaration of a data type long integer named LONG1, which is initialized to the value 0.

```
LONG    name = LONG1
```

Predefined variables
--------------------

A predefined variable is a variable whose value is defined when loaded. These variables can be used without prior notification,and begin with the "_" character. The following variable is predefined:

- _CMDRET: This variable stores the return code from the last command.

Using data
----------

There are two ways to use data, either by:

- Substitution
- Setting the data

### Parameter

Some commands can accept a parameter takes as a value the name of a variable in order to update during the execution phase, the content of this variable. An example is the function _TIME, which updates a variable with the current time.

### Substitution

To substitute data use the variable name enclosed by the '%' character. The interpreter then replaces the variable name with the contents.

****Example****

The interpreter replaces% STRING% with the string value ABCDEF. The interpreted command becomes PRINT MSG = 'ABCDEF'.

```
CHAR   name = STRING, size = 8, init = ABCDEF
PRINT  MSG = '%STRING%'
```

Digital data processing
-----------------------

### Syntax

All digital data processing commands (integer and long integer) have a syntax modeled on the following:

```
id_cmd NAME = keyword,
      VALUE = value
```

### Parameters

- NAME: This parameter specifies the name of the data.
- VALUE: This parameter specifies a value acted on according to the command (assignment, operation).

### Command _MOV

The _MOV command assigns a value to a variable of type integer or long integer.

```
INT     name=INT1
INT     name=INT2, init=0
_MOV    name=INT1,value=10         / \*Assigns 10 to INT1\* /
_MOV    name=INT2,value=%INT1%     / \*Assigns INT2 to INT1\* /
```

### Command _ADD

The _ADD command adds a value to a variable content.

```
INT name=INT1,init=0
INT name=INT2,init=0
_ADD name=INT1,value=10        /\* Adds 10 to INT1 \*/
_ADD name=INT2,value=%INT1%    /\* Assigns INT2 to INT1\* /
```

### Command _SUB

The _SUB command subtracts a value of a variable.

```
INT name=INT1,init=100
INT name=INT2,init=1000
_SUB name=INT1,value=10 /\* Subtracts 10 from INT1 \*/
_SUB name=INT2,value=%INT1% /\* Subtracts INT1 from INT2 (results in INT2) \*/
```

### Command _MUL

The _MUL command multiplies the contents of a variable with a value.

```
INT name=INT1,init=1
INT name=INT2,init=1000
_MUL name=INT1,value=%INT1% /\* Squares the INT1 value \*/
_MUL name=INT1,value=10 /\* Multiply INT1 by 10 \*/
_MUL name=INT2,value=%INT1% /\* Multiplies INT2 by INT1 (result in INT2\*) /
```

If a variable exceeds its the maximum possible value for that variable, the result is unpredictable and also system dependent. In general though, the result is the multiplication of the variable.

### Command _DIV

The _DIV command divides by the variable value.

```
INT name=INT1,init=100
INT name=INT2,init=1000
_DIV name=INT1,value=10 /\* Divide INT1 by 10 \*/
_DIV name=INT2,value=%INT1% /\* Divide INT2 by INT1 (results in INT2) \*/
```

Division by 0 is not allowed. The value assigned to the variable is the quotient of the division (it cannot be null).

### Command _MOD

The command computes the modulus _MOD of a variable with respect to a value. The result is assigned to the variable.

```
INT name=INT1,init=100
INT name=INT2,init=1000
_MOD name=INT1,value=10 /\* INT1 modulus of 10 \*/
_MOD name=INT2,value=%INT1% /\* INT2 modulus of INT1 (results in INT2) \*/
```

### Command _AND

The _AND command performs a logical AND between the contents of a variable and a value. The result is assigned to the variable.

```
INT name=INT1,init=100
INT name=INT2,init=1000
_AND name=INT1,value=10 /\* INT1 and 10 \*/
_AND name=INT2,value=%INT1% /\* INT2 and INT1 (results in INT2) \*/
```

### Command _OR

The _OR command performs a logical OR operation between the contents of a variable and a value. The result is assigned to the variable.

```
INT name=INT1,init=100
INT name=INT2,init=1000
_OR name=INT1,value=10 /\* INT1 or 10 \*/
_OR name=INT2,value=%INT1% /\* INT2 or INT1 (results in INT2) \*/
```

### Command _NAND

The _NAND command performs a NAND logic between the contents of a variable and a value. The result is assigned to the variable.

```
INT name=INT1,init=100
INT name=INT2,init=1000
_NAND name=INT1,value=10 /\* INT1 nand 10 \*/
_NAND name=INT2,value=%INT1% /\* INT2 nand INT1 (results in INT2) \*/
```

### Command _XOR

The _XOR command performs a logical XOR operation between the contents of a variable and a value. The result is assigned to the variable.

```
INT name=INT1,init=100
INT name=INT2,init=1000
_XOR name=INT1,value=10 /\* INT1 xor 10 \*/
_XOR name=INT2,value=%INT1% /\* INT2 xor INT1 (results in INT2) \*/
```
