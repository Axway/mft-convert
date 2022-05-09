{
    "title": "Use  interactive mode ",
    "linkTitle": "Using interactive mode",
    "weight": "130"
}CFTUTIL can be used in an interactive mode, which consists of several
operating modes that use the same CFTUTIL operations regardless of the operating system.

<span id="Data_entry_in_command_line"></span>

Data entry in command line
--------------------------

Enter the CFTUTIL commands via the keyboard. The output is sent to the
screen by default, or to a file. For more information see the CONFIG command.

`CFTUTIL ‘file_symb’file_in file_out`

In this syntax:

- ‘file_symb’ designates a character specific to
    each environment. Refer to the Transfer CFT Operating Guide that corresponds
    to your OS:


| OS |  file_symb |
| --- | --- |
| Windows | # |
| UNIX |  @ |


- file_in is a file
    containing the Transfer CFT commands
- file_out is a file
    into which the results of the commands are written

Specifying parameters is optional, the default values are the
standard input and output respectively.

The CFTUTIL module is activated in the Transfer CFT executable file
directory. The file_in and file_out designate the complete path or path
relative to the current directory.

On all systems:

- CFTUTIL standard
    input and standard output are equivalent to:
    -   CFTUTIL ‘file_symb’
        CFTIN CFTOUT
- CFTIN and CFTOUT
    are reserved words corresponding to the standard input and output
- Standard input
    and output to a file: CFTUTIL ‘file_symb’ CFTIN file_out
- File input and
    standard output: CFTUTIL ‘file_symb’ file_in CFTOUT
- File input, file
    output: CFTUTIL ‘file_symb’ file_in file_out

#### CONFIG TYPE=...,FNAME=...

Another way to change the input/output file is to use the CONFIG command with a syntax as displayed
here:

`CFTUTIL> CONFIG TYPE=OUTPUT, FNAME=file_out...> CONFIG   TYPE=INPUT, FNAME=file_in`

See the [CONFIG
command](../../../admin_intro/admin_config_commands/communication_media_concepts).
