---
title: "Typographical  conventions"
linkTitle: "Typographical conventions"
weight: 160
--- The typographical conventions specify the
syntax to use in {{< TransferCFT/axwayvariablesComponentShortName  >}} commands, the
parameters and their values. These rules apply equally for any additional
parameter information, or information pertaining to the operating system
or the transfer protocols.

> **Note**
>
> Use the Command
> index to help locate a command, and the Parameter
> index for parameter details.

<span id="Command_description"></span>

## Command description

The description of each command is generally organized in parts:

- Functional description
- List indicating
    the general syntax of parameters passed with a command, grouped by categories

****Example****

```
CFTFILE [MODE = {CREATE
&#124; DELETE},]
FNAME = filename,
TYPE = {PARM
&#124; PART &#124; CAT &#124; LOG &#124; ACCNT
&#124; COM},
```

- Detailed description
    of parameters, in alphabetical order

FNAME =
filename (Name
of the file to which the command applies)

- Generic example
    of parameter setting. Parameter setting examples are indicated in New
    Courier typeface.

Example of creating a parameter file:

`CFTFILE TYPE = PARM,    MODE = CREATE,    FNAME = filename`

<span id="Parameter_description"></span>

### Parameter description

There are two types of parameters mandatory and optional:

- Mandatory parameters
    are followed by the word (*Mandatory*)
    in italics
- Optional parameters
    (placed between square brackets [ ])

Each parameter description applies the following general syntax:

| PARAMETER = value(s) | Information | SPECIFIC |
| - - - | - - - | - - - |
| Definition of the parameter | Indication of any additional information available for the value defined. | Indication of the field of application and any usage restrictions for the parameter |

The information and specifics fields are optional.

****Example****

```
COPYFILE
OFNAME
= filename
```

In this command, the OFNAME
parameter is mandatory. Its value corresponds to a filename.

Example

```
CFTSEND [NFNAME =
filename]       PeSIT
E CFT/CFT
```

In this object, the NFNAME
parameter is:

- Used to indicate
    a physical filename
- Optional with no
    default value, the optional nature being indicated by square brackets
    [ ]
- Reserved for transfer
    cases in PeSIT E
    profile between two {{< TransferCFT/axwayvariablesComponentShortName >}}s

<span id="Parameter_value_notation_conventions"></span>

### Parameter value notation conventions

The notation conventions generally used to describe parameter values
are listed in the following table.

| Description  | Notation  | Example  |
| - - - | - - - | - - - |
| List of possible values  | {value, value}  | {filename, string}  |
| Choice  | {1 &#124; 2}  | {CREATE &#124; DELETE}  |
| Numeric field<br /> (value indicated between a and b)  | {a..b}  | {0..255}  |
| Default value  | underlined  | {CREATE &#124; DELETE}  |
| Optional parameter  | [PARAMETER]  | [NFNAME = filename]  |
| {value&#124;text} must be indicated  | italic  | filename  |
| Mandatory parameter | (Mandatory) |   |

<span id="Generic_type_parameter_values"></span>

### Generic- type parameter values

The conventions used for generic- type values are listed in the following
table.

| Description  | Notation  | Example  |
| - - - | - - - | - - - |
| Character type value: one single character  | c | FTYPE = c  |
| Numeric type value: numeric character string  | n | FLRECL = n  |
| Character string value: series of alphanumeric characters or series of characters between quotes  | string | SAP = string  |
| If the parameter is mandatory: string containing between 1 and n characters inclusive<br/> If the parameter is optional: string containing between 0 and n characters  | stringn | PUNAME = string10<br /> String containing between 1 and 10 characters<br /> <br /> [COMMENT = string32]<br /> String containing between 0 and 32 characters  |
| String containing between n and m characters | stringn..m  | LUNAME = string3..8<br /> String containing between 3 and 8 characters  |
| String containing exactly n characters | stringn  | KEY = string21<br /> 21- character string  |
| Constant- type value (preset)  | VALUE  | TYPE = PESIT  |
| Password type value: string containing between n and m characters | string  |   |

The string notation is used generically, in lists indicating
the general parameter syntax for example. The stringn,
stringn..m
and stringn
notations are used in the detailed parameter descriptions.

### HELP command use and conventions

When using the CFTUTIL HELP command, as shown in the example below, the following rules apply to parameter values:

- If `STRING `is in upper case, the parameter value is not case sensitive
- If `String `is mixed case, the parameter value is case sensitive
- If `STRING or "String"`, the parameter value is only case sensitive when enclosed in quotes

****Example****

```
CFTUTIL help cmd=cftsend, content=detail

COMMAND CFTSEND USAGE
 MODE STRING max_length=7 Action to do in the parameter or partner base <REPLACE>
 'CREATE'

   ...
 PRESTATE STRING
max_length=0 The transfer phase step as it enters the A phase < > (' ','DISP','HOLD')
   ...
 RAPPL STRING or "String"
max_length=48 Identifier of the file receiver application
  ...
 RPASSWD String
max_length=32 Password for the user who is receiving the file

```

### Using single quotes and double quotes

The following conventions apply when using double quotes (hereafter referred to as quotes) " " and single quotes ' ' in CFTUTIL commands:

- Enclosing in quotes " " keeps case sensitivity for applicable parameters as listed in the **Note** below.
- Enclosing in a single quote ' ' keeps spaces in the value

> **Note**
>
> Parameters affected by the quote sensitivity include: FLOWNAME, RAPPL, RUSER, SAPPL, SOURCEAPPL, SUSER, TARGETAPPL, NRPASSW, NSPART, NSPASSW.

Lastly, to use the quote character in a parameter string you have to repeat it, for example:

```
PARM = 'xxx,''yyy'',zzz',
```
<span id="Parameter_values_concerning_preset_categories"></span>

### Parameter values for preset categories

The conventions used for values concerning preset categories are listed
in the following table.

| Description  | Notation  |
| - - - | - - - |
| Compression: numeric value between 0 and 15 indicating the compression algorithm  | cpr  |
| Date: 8- digit string | YYYYMMDD  |
| File name: 512 characters including the drive, path, root, suffix, where limitations are imposed by file system or operating system (such as list of unauthorized characters , length, case- sensitivity, etc.) | filename  |
| Identifier: alphanumeric string of 1 to 32 alphanumeric characters and additional characters:<br/> @ # &amp; % ! : - _ + \ / &#124; ? { } [ ] ; * &lt; &gt; ~ ^ | identifier  |
| Mask: string containing wildcard characters (* and ?) :<br/> When referring to ReGEX expressions, other value are possible. | mask  |
| Time: string containing 2 to 8 digits  | HHMMSSSS |
| Transfer identifier assigned by {{< TransferCFT/axwayvariablesComponentShortName  >}}  | transid  |

<span id="OS_specificities"></span>

### OS specificity

In the parameter syntax listings, values that are OS specific indicated
as follows: OS

Example: ACCID
= n                              OS

In the detailed description of a parameter, the operating system(s)
for which this parameter is pertinent is (are) indicated in detail, for
the operating system concerned.

Example: ACCID
=n           

MVS

The default value of a parameter may differ from one system to another;
this is indicated as follows: Dft: OS

Example: [FCODE    
= {BINARY &#124; EBCDIC &#124; ASCII}          Dft:
OS

<span id="Protocol_dependent_parameters"></span>

### Protocol dependent parameters

In the parameter syntax listings, the parameter values that are dependent
on one or more protocols are indicated as follows: PROTOCOL

****Example****

`[NSPACE       = n,]               PROTOCOL`

In the parameter description, the protocol concerned is
indicated explicitly. This notation is used, for example, in the following
cases:

- Associated parameter
    is only defined for certain protocols

Example: [NSPACE = {value of FSPACE
&#124; n},]           ODETTE,
PeSIT

- Length of the value
    of the associated parameter differs from one protocol to another.

****Example  
****

`[NSPASSW = string]string8        PeSITstring8       ODETTE      string22`

The default value of a parameter may differ from one protocol to another;
this is indicated as follows: Dft: PROTOCOL

****Example****

`[NCODE = {see the comment   &#124; BINARY &#124; EBCDIC &#124; ASCII}]      Dft: PROTOCOL`

The default value of a parameter may differ from one PeSIT protocol
profile to another; this specificity is indicated as follows: Dft:
PROFILE

****Example****

`[DISCTS     = n]                           Dft: PROFILE    `

If the parameter in the command, in this case CFTPROT TYPE=PeSIT, is
not defined, the default value of the profile is used.

<span id="Protocol_variants"></span>

### Protocol variants

Specificity concerning the PeSIT protocol. A protocol dependent parameter may involve one or more of the PeSIT
protocol variants, indicated as follows:

| Protocol  | Description  |
| - - - | - - - |
| PeSIT | PeSIT protocol (standard) |
| PeSIT CFT/CFT | PeSIT protocol used between two {{< TransferCFT/axwayvariablesComponentShortName  >}}s |

<span id="Command_syntax"></span>

### Command syntax

The parameter setting commands are presented in the following format:

| Syntax | Command syntax listed here [see parameters below] |
| - - - | - - - |
| Description | Each parameter setting command generates one or more binary records in the PARAMETER or PARTNER file. |
| Parameters | Parameter name and abbreviation |
| id | identifier {0..32768}<br/> Identifies the object described by the parameter setting command. |
| mode | Describes the operation to be performed on the PARAMETER or PARTNER files.<br/> • REPLACE (default): modify the associated record or records, or create them if they do not exist; this is the default value (default)<br/> • CREATE: add one or more records<br/> • DELETE: delete one or more records |
| Usage rules | All the parameters required to identify the file must be specified, except in the case of DELETE where the ID parameter is sufficient.<br/> When you select REPLACE the following occurs:<br/> • If the CLASS parameter is modified a new record is created<br/> • If the CLASS parameter is not modified the new record overwrites the existing one<br/> Only the parameters specified in the command are taken into account. Default value are used for unspecified parameters.<br/> The comment for the MODE parameter is common to all the parameter setting commands and is not repeated on the following pages. |
| Example | When available, an example is listed here. |
| Syntax | Command syntax listed here [see parameters below] |

<span id="Command_reply_format"></span>

### Command reply format

****Example****

The report `CFTU94I   SHUT _Correct `indicates that the SHUT command was correctly entered
in CFTUTIL.

### Referencing environment variables

You can reference a environment variable in the Transfer CFT configuration using the `%env:<variable>%`, for example, `FNAME=%env:CFTDIRPUB%/FTEST`. This value is replaced with the actual value when you define the object and is never used again as a variable.

### Referencing UCONF parameters

You can reference a UCONF parameter in the Transfer CFT configuration using the `%uconf:<parameter>%`, for example, `PART=%uconf:cft.instance_id%`. This value is replaced with the actual value when you define the object and is never used again as a variable.
