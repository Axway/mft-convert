{
    "title": "CFTUTIL programming",
    "linkTitle": "CFTUTIL programming",
    "weight": "180"
}Elements
--------

### Comments

A comment is text that is delimited by the character pairs / \* and \*/, which is then ignored by the interpreter.

### Command syntax

An internal command or application consists of the following:

- ****A unique keyword**** that identifies the command (id_cmd). A keyword is a string composed of a maximum of 8 characters, and begins with an alphabetic character (A-Z, a-z).
- ****A list of arguments****

The general command syntax is:

```
<list_of_arguments>
```

An argument consists of the following elements:

- A keyword that is unique for the command, and which identifies a parameter (id).
- Value assigned to the parameter.

The syntax of an argument is:

```
<id> = <value>
```

Separate two arguments by a, (comma). The command syntax becomes:

```
[<id> = <value>,] \*<id> = <value>
The expression in brackets is repeated.
```

The syntax for a parameter value results from the parameter type. A parameter can accept more than one value at a time. Set the multiple values in brackets and separate the values by a , (comma).

```
<id> = ([<value>,] \* <value>)
The expression in brackets is repeated.
```

### Parameter type

A command parameter is one of the following types:

- Free
- Hexadecimal
- Numeric
- Binary
- Choice

### Free

The value may consist of any character. It may be limited or not by the apostrophe character '.

Demarcation is required when the value should contain blanks or commas. A character can be written in ASCII or EBCDIC as follows:

- /ooo o represents an octal character. For example, in ASCII, the character A (code 65) can be written /101. A blank is written /040.
- /xhh h is a hexadecimal character. For example, in ASCII, the character A (code 65) is written /x41.

```
id = 'VALUE WITH BLANKS'
id = 'V/x41LUE WITH BL/101NKS'
```

These two expressions are equivalent.

To enter the characters ' (apostrophe) or / (slash), precede these with the / character. This results respectively in / 'and / /.

```
id = 'J /' APOSTROPHE '
```

### Hexadecimal

A value may consist of any character hexadecimal (0-9, A-F, a-f).

```
id = 0123456789ABCDEFabcdef
```

### Numeric

A value may consist of any numeric character (0-9).

```
id = 0123456789
```

### Binary

A value may consist of any binary character (0-1).

```
id = 0110010100
```

### Select

The value must belong to a predefined list such as, a parameter whose possible values ​​are 'YES' or 'NO'.

```
id = YES
```

Analyzing the source text
-------------------------

A source language is comprised of he following elements:

- syntax
- semantics

The syntax specifies underlying language grammatically, and the semantics clarify the meaning attributed to these language elements.

The core of the shell is limited to parsing the source text. Language processing involves two steps:

- lexical analysis
- parsing

The purpose of the lexical analysis is to transform the source text to make it easier for the parser to handle. Lexical analysis can, for example, signal the use of unnecessary blanks, comments, and the delineation of simple syntax units.

The parser is designed to recognize whether the proposed text belongs to the language. Semantic errors, which refers to those that relate to what the text means, are only detected at runtime.

Conversely, parsing the source text creates a coded representation that is easier to handle, as it is more suited to a programming language. This coded representation is similar to the C language structure (text object).
