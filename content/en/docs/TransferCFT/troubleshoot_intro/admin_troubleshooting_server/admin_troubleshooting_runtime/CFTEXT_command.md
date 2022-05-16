---

    title: CFTEXT  - Extract file data 
    linkTitle: Extract file data
    weight: 350

---
You can use the <span id="About_the_CFTEXT_Command"></span><span style="font-weight: bold;">****CFTEXT****</span>
object to extract Parameter and Partner file data. CFTEXT generates, as output, a configuration command text used to reconstitute
the data of these files.

The resulting configuration
file is then submitted to CFTUTIL to:

- Rebuild a lost
    or damaged configuration
- Facilitate the
    exporting of a {{< TransferCFT/axwayvariablesComponentLongName >}} configuration to another computer
- Upgrade the Transfer
    CFT software when such an upgrade incorporates a file structure modification

The configuration command text generated is written to the CFTUTIL output
medium. To recover a CFTUTIL output, you can either redirect the standard
output or else redefine the output medium via the command CONFIG TYPE
= OUTPUT.

All parameter values are in UPPER CASE letters.

Command syntax: <span style="font-weight: bold;">****[CFTEXT](../../../../c_intro_userinterfaces/command_summary#CFTEXT)****</span>

QQQ\_QQQ\_CHECK command description

QQQ\_QQQ\_QQQ

Parameters

Description

<span style="font-weight: bold;">****[CFTEXT](../../../../c_intro_userinterfaces/command_summary#CFTEXT)****</span> command

Use this command to extract all or part of the data from
the parameter and partner files.

[ID](../../../../c_intro_userinterfaces/command_summary/parameter_intro/id) 

Identifier of the parameter to be extracted.

The value of this identifier is the value of the ID of
the command CFTxxxx corresponding to the TYPE parameter; this allows the
extraction to be limited:

- To an
    explicitly indicated value (identifier)
- Or to
    a group of values designated through the use of a mask using "wildcard"
    characters

When this parameter is not defined, all the occurrences
of the parameter type (defined by TYPE) are extracted.

[CONTENT](../../../../c_intro_userinterfaces/command_summary/parameter_intro/content)

Level of content included in output:

- BRIEF = Empty or default value parameters are omitted
- FULL = All parameters are included

[FOUT](../../../../c_intro_userinterfaces/command_summary/parameter_intro/fout) 

Name of the file to which the command’s standard output
will be redirected.

This generated file can then be interpreted directly by
CFTUTIL.

When this parameter is not filled, all occurrences of the
type parameter (defined in the TYPE parameter) are extracted.

[FPARM](../../../../c_intro_userinterfaces/command_summary/parameter_intro/fparm)

{see the comment |
filename} 

Except for TYPE = PART

Name of the Parameter input file.

Default value: default name of the Parameter file defined
for CFTUTIL for the system concerned. Refer to the Transfer CFT *Operations
Guide* that corresponds with your OS.

[FPART](../../../../c_intro_userinterfaces/command_summary/parameter_intro/fpart) 

{see the
comment | filename}\]

For TYPE = {ALL | PART}

Name of the Partner input file.

Default value: default
name of the Partner file defined for CFTUTIL for the system concerned.
Refer to the Transfer CFT <span class="italic_in_para">Operations Guide</span> that corresponds with
your OS.

[TYPE](../../../../c_intro_userinterfaces/command_summary/parameter_intro/type) 

This parameter defines the parameter type to be extracted.

****Example 1****

```
CFTEXT
```

Extraction of all data from the CFTPARM parameter and CFTPART
partner files.

********Example 2********

```
CFTEXT     TYPE    
=     SEND,
     ID     =    
FACT,
     FPARM     =    
mycftparm
```

Extraction of data concerning the model file to be sent
(CFTSEND command) with an IDF = FACT, from the file mycftparm.

********Example 3********

```
CFTEXT
TYPE = RECV,
ID = FACT\*
```

Extraction of the data concerning the model files to be
received (CFTRECV command) whose IDF value begins with the four letters
"FACT". The name of the Parameter file is the default name indicated.
Refer to the Transfer CFT Operations Guide that corresponds with
your OS.

********Example 4********

```
CFTEXT
TYPE = PART,
ID = MAGA\*
```

Extraction of the partner data corresponding to the CFTPART
commands, the identifier of which begins with the four letters "MAGA".
The Partner file name is the default name indicated. Refer to the Transfer
CFT Operations Guide that corresponds with your OS.

****Example 5****

Transfer
CFT application definition:

```
CFTEXT TYPE = APPL,
```