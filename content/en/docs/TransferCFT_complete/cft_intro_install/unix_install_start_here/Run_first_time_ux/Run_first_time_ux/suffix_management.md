{
    "title": "Suffix  management",
    "linkTitle": "Suffix management",
    "weight": "270"
}By default, during file send or receive operations, Transfer CFT uses
the file type, [FTYPE](../../../../../c_intro_userinterfaces/command_summary/parameter_intro/ftype),
to determine the action to be taken. However, Transfer CFT also features a mechanism called suffix management
which enables Transfer CFT to determine the type
of file processed from the file name. This topic describes:

- [Defining suffixes](#Defining)
    -   [Enabling suffix management](#Enabling)
- [Separating file name extensions](#Separati)

<span id="Defining"></span>

Defining suffixes
-----------------

For Transfer CFT a suffix is the rightmost part of the file name, and
may have up to eight characters.

To be recognized by Transfer CFT, a suffix must be declared in the *suffixes.def*
file located in the directory to which the CFTDIRDAT environment variable
points, usually the *&lt;installdir&gt;/runtime/data/* directory.

The suffix definition file is a text file that the user creates using
a text editor, *vi* for example. Each suffix definition must be in
the form: &lt;suffix&gt;=&lt;FTYPE&gt;

Where:

- The length of the
    suffix is less than or equal to eight characters
- [FTYPE](../../../../../c_intro_userinterfaces/command_summary/parameter_intro/ftype)
    is one of the file types recognized by Transfer CFT

Furthermore, the lines making up the *suffixes.def* file must comply
with the following rules:

- There can be only
    one suffix definition per line
- Suffixes can be
    defined with *wildcard* characters, which define either any character
    (?) or any character string (**\***)
- The suffix definitions
    are case-sensitive but the types are not. Type **t** is, therefore,
    identical to type **T**, but the suffix **.txt** is different from
    the suffix **.TXT**
- Empty lines and
    lines containing only spaces are ignored
- Comments can be
    inserted in this file using the \# character  
    Any text situated between a \# character and the end of the line will
    be considered to be a comment

Sample suffix definition file:

`## Sample suffix definition file#.doc=O           # MS-DOS   text file (param.doc for example).txt=T             # UNIX text file(cft.txt for example)*.bin=B          #   Binary file (fil.bin for example)*.dat?=B     # Binary   file (john.dat0,fred.data for example)`

`          #   ...`

<span id="Enabling"></span>

### Enabling suffix management

Suffix management is enabled by setting the FTYPE field to a space in
the CFTSEND or CFTRECV sections. In the Transfer CFT syntax, this space
must be placed between single quotes.

****Example****

`CFTSEND ID = DAT,FTYPE = ' ',MODE = REPLACE`

<span id="Separati"></span>

Separating file name extensions
-------------------------------

Transfer CFT provides a parameter option that allows the user to the separate the file name from the file extension, mimicking the Transfer CFT Windows functionality.

In Transfer CFT the name of a file is referred to as ROOT or FROOT, and the file extension (suffix) is FSUF or SUF. So for example, if you have a file called ****sample.txt**** and in Transfer CFT you define ****FROOT=sample.txt****, then in standard functioning:

- In Unix the FSUF is empty
- In Windows the FSUF is ****txt****

To enable the option to separate the file name and extension in Unix, set the following UCONF value to `yes`:

` cft.unix.parse_file_name_suffix=yes`

For more information on unified configuration parameters, see [UCONF: Unix-specific parameters](../uconf_unix).
