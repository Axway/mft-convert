{
    "title": "Setting unified configuration  values",
    "linkTitle": "Modifying UCONF fields with CFTUTIL",
    "weight": "250"
}From a command line window, you can use the CFTUTIL utility to view
and modify UCONF parameters. To view the contents and types of variables, you can create a list.
Enter the command:

`CFTUTIL listuconf id=*,content=FULL`

To extract the contents of the Unified Configuration tool, enter the command:

`LISTUCONF CONTENT=EXTRACT, FOUT=out `

### Commands

**UCONFSET**:
modify a technical parameter value.

`UCONFSET id=PARAMETER-KEY,value=STRING`

****Example 1****

Set cft.idparm parameter:

`  UCONFSET   id=cft.idparm,value=IDPARM0`

****Example 2****

Set copilot.general.serverport parameter:

`  UCONFSET   id=copilot.general.serverport,value=8080`

**UCONFGET**:
retrieve a single technical parameter value

`UCONFGET id=PARAMETER-KEY`

**Example**

Get cft.idparm parameter:

`  UCONFGET   id=cft.idparm`

**UCONFUNSET**:
return a specified parameter to the default value

`UCONFUNSET id=_____`

**LISTUCONF**:
display multiple technical parameter values

`LISTUCONF id=PARAMETER-KEY-PATTERN,scope=ALL&#124;USER&#124;   &#124;DEFAULT,content=BRIEF&#124;FULL&#124;DEBUG`

****Example 1****

Display all Copilot parameters:

  LISTUCONF
id=copilot.\*

****Example 2****

Display all Copilot parameters that have
been modified:

  LISTUCONF
id=copilot.\*,scope=USER

****Example 3****

Display all parameters concerning copilot
with additional information:

  LISTUCONF
id=copilot.\*,content=FULL

****Example 4****

Use the [content](../../../c_intro_userinterfaces/command_summary/parameter_intro/content) parameter to define output properties:

`LISTUCONF CONTENT=EXTRACT&#124;DEBUG&#124;PROPS`

Where:

- EXTRACT: Output suitable to enter in CFTUTIL
- DEBUG: Additional information
- PROPS: Property-like output

****Example 5****

To output the content into a file use with the [FOUT](../../../c_intro_userinterfaces/command_summary/parameter_intro/fout) parameter:

`CFTUTIL LISTUCONF FOUT=fname`
