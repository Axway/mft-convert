{
    "title": "Symbols, environmental variables, and the Transfer CFT profile ",
    "linkTitle": "Symbols, environmental variables, and the profile ",
    "weight": "140"
}This
page describes the symbols, default values, and variables used by Transfer
CFT that are specific to each operating system and comprises:

- Specific
    symbols
- Default
    files used by CFTUTIL
- Setting the {{< TransferCFT/axwayvariablesComponentLongName  >}} profile file

Specific symbols
----------------


| Description  | Windows  | Unix  | z/OS  | IBM i  | **OpenVMS  |
| --- | --- | --- | --- | --- | --- |
| Logical name prefix (the character used to specify an environmental variable)<br/> If the file name begins with this character, this is a logical name interpreted by Transfer CFT. | $  | $ or _  | x'5B'<br/> 285 = £<br/> 297 = $ | x'4E'<br/> 285 = +<br/> 297 = + | No specific character;<br/> logical names are<br/> processed transparently by RMS |
| Wildcard character<br/> When using the STRJCMP method, a single character that must be an exact match with a character and is used in masks for groups of files and folder monitoring. | ?  | ?  | x' 6F<br/> 285 = ?<br/> 297 = ? | x'6F'<br/> 285 = ?<br/> 297 = ? | %x  |
| Separator character (volume)<br/> For example, a file name can be represented as follows: &lt;unit&gt;&lt;Separator&gt;&lt;unitc&gt;&lt;Separator&gt;&lt;path&gt;&lt;root&gt;&lt;suf&gt;<br/> In a z/OS environment: UNIT%UNITC%PATH.ROOT.SUF | none  | none  | x' 6C'<br/> 285 = %<br/> 297 = % | x '5E'<br/> 285 = ;<br/> 297 = ; | No volume concept |
| Symbolic variable prefix  | &amp;  | &amp;  | x'50'<br/> 285 = &amp;<br/> 297 = &amp; | x'6F'<br/> 285 = ?<br/> 297 = ? | &amp;  |
| Indirection file name prefix<br/> Used in a group of files or for an indirection file.<br/> For example, FNAME=&lt;Indirection prefix&gt;file name | #  | @  | x'7B’<br/> 285 = #<br/> 297 = £ | x'B1'<br/> 285 = [<br/> 297 = # | Either # or @  |
| Character introducing the path name of the FNAME parameter (CFTRECV) from which a tree structure is created.<br/> On Windows, for example, FNAME=pub\+dir1\dir2\ftest | +  | +  | +<br/> Limited to USS files | +<br/> Limited to HFS files | +  |


Default files used by CFTUTIL
-----------------------------


| File | Default<br/> Windows | Unix | z/OS | IBM i | OpenVMS |
| --- | --- | --- | --- | --- | --- |
| Parameters file  | $CFTPARM  | $CFTPARM |  $CFTPARM |  +CFTPARM |  CFTPARM |
| Partners file  | $CFTPART  |  $CFTPART |  $CFTPART |  +CFTPART |  CFTPART |
| Catalog file  | $CFTCATA |  $CFTCATA |  $CFTCAT |  +CFTCAT |  CFTCATA |
| Communication file  | $CFTCOM  |  $CFTCOM |  $CFTCOM |  +CFTCOM |  CFTCOM |
| Accounting file  | $CFTACCNT  | $CFTACCNT  | $CFTACCNT  | +CFTACCNT  | CFTACCNT  |
| Alternate accounting file  | $CFTACCNTA  | $CFTACCNTA  | $CFTACCNTA  | +CFTACCNTA  | CFTACCNTA  |
| Log file  | $CFTLOG  | $CFTLOG  | $CFTLOG  | +CFTLOG  | CFTLOG  |
| PKI database file  | $CFTPKU  | $CFTPKU  | $CFTPKU  | +CFTPKU  | CFTPKU  |


Setting the profile
-------------------

In order to run {{< TransferCFT/axwayvariablesComponentLongName  >}} components, you must execute the profile. When loading the Transfer CFT profile, files that are stored in the profile.d directory are also executed, and all defined environment variables are then available in the current environment. This enables you to use these variables in the Transfer CFT configuration or processing scripts.

You can regenerate the profile after executing the profile from the runtime directory as follows:

- Unix: cftruntime -p $CFTDIRINSTALL $CFTDIRRUNTIME
- Windows: cftruntime %CFTDIRINSTALL% %CFTDIRRUNTIME% -profile

For other platforms, please refer to the operating specific installation guide.

### Setting the profile file: Unix

You can set the environment variables using the `. .\profile` file located in the Transfer CFT runtime folder.

### Setting the profile.bat file: Windows

You can set the environment variables using the `profile.bat` file located in the Transfer CFT runtime folder.
