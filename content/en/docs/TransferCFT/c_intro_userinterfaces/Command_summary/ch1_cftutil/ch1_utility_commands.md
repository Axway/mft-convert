{
    "title": "Utility commands",
    "linkTitle": "Utility commands",
    "weight": "240"
}This section describes the utility commands.

- Capture
- Dump

### Capture

#### Syntax

CAPTURE command to redirect the screen output to a file.

```
CAPTURE FNAME = NAME,
        FACTION = ACTION
        MASK = NNN
```

#### Parameters

- NAME: Character string indicating the name of the output file. This file is a standard text file variable.
- FACTION: Character string indicating the method of opening the output file. The possible values ​​for FACTION are:
    -   APPEND
    -   ERASE
    -   DELETE
- NNN: Number 0 to 255 as a mask. This number can hide some output screens.
    -   128 displays only the messages from the PRINT command.

#### Example

```
CHAR name = TRACEFILE, size = 13, init = TRACE
CAPTURE FNAME = %TRACFILE%, FACTION=APPEND, MASK=128
```

### Dump

#### Syntax

The DUMP command displays the contents of a file or a variable.

```
DUMP NAME = name,
     FNAME = fname
     TYPE = t,
           LRECL = n,
     RECFM = V/F/U,
     ORG = SEQ/IND/PAR/DIR,
     ACC = a,
     KEYLEN = n,
     KEYPOS = n,
     SRECNUM = n,
     ERECNUM = n,
     MODE = HEXA/TEXT/OCTAL
```

#### Parameters

- Name: Variable to display. This parameter is exclusive fname parameter.
- Fname: The explicit name of the file to display.
- Mode: Display mode HEX, OCTAL or TEXT.
- The following parameters can only display the contents of a file.
    -   Org: File organization.
    -   lrecl: Length of file record.
    -   Recm: Record file format.
    -   Srecnum: The registration number from which the file contents are posted.
    -   Erecnum: The registration number from which the display is stopped.
    -   Keypos: The key position in the record (only one organization indexed).
    -   Keylen: The key length (only for an indexed organization).
    -   Type: File type for one character. The file type is system-dependent.

#### Features

ICMCIA value for the name parameter will display the ​​ICM work area.

#### Example

```
DUMP FNAME = test
```
