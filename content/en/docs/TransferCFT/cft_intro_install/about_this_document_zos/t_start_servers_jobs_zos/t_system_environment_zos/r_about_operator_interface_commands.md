{
    "title": " About operator interface commands",
    "linkTitle": "About operator interface commands",
    "weight": "250"
}This section describes how to set up the diagnosis commands, which enable you to perform a search of errors that can occur in Transfer CFT.

Operator interface interactions
-------------------------------

### Types of commands 

The z/OS operator interface for Transfer CFT supports two types of commands:

- The Transfer CFT commands described the Command Index

<!-- -->

- The diagnostic commands described in section Diagnostic commands

### Types of responses

The Transfer CFT operator interface for z/OS supplies the following responses:

- SGOP02I Command Complete dd/mm/yyyy, hh:mm:ss User=xxxxxx
    -   The command is processed without error.
- SGOP03E Command error,RC=xxxxxxxx
    -   The command ends with the return code xx.
- SGCM05E Unknown Transfer CFT command...
    -   Transfer CFT does not recognize the command.
- SGCM03E Command ignored
    -   The command is not processed.

****Related topics****

[Diagnostic commands]()

[The ? command]()
