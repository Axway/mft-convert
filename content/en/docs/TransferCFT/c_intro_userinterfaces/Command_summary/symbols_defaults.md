---

    title: Specific symbols and default files
    linkTitle: Specific symbols and default files
    weight: 160

---
This
page describes the symbols, default values, and variables used by Transfer
CFT that are specific to each operating system and comprises:

- Specific
    symbols
- Default
    files used by CFTUTIL

## Specific symbols


| Description  | Windows  | Unix  | z/OS  | IBM i  |
| --- | --- | --- | --- | --- |
| Logical name prefix<br/> If the file name begins with this character, this is a logical name interpreted by Transfer CFT. | $  | $ or _  | x'5B'<br/> 285 = £<br/> 297 = $ | x'4E'<br/> 285 = +<br/> 297 = + |
| Wildcard character<br/> When using the STRJCMP method, a single character that must be an exact match with a character and is used in masks for groups of files and folder monitoring. | ?  | ?  | x' 6F<br/> 285 = ?<br/> 297 = ? | x'6F'<br/> 285 = ?<br/> 297 = ? |
| Separator character (volume)<br/> For example, a file name can be represented as follows: &lt;unit&gt;&lt;Separator&gt;&lt;unitc&gt;&lt;Separator&gt;&lt;path&gt;&lt;root&gt;&lt;suf&gt;<br/> In a z/OS environment: UNIT%UNITC%PATH.ROOT.SUF | none  | none  | x' 6C'<br/> 285 = %<br/> 297 = % | x '5E'<br/> 285 = ;<br/> 297 = ; |
| Symbolic variable prefix  | &amp;  | &amp;  | x'50'<br/> 285 = &amp;<br/> 297 = &amp; | x'6F'<br/> 285 = ?<br/> 297 = ? |
| Indirection file name prefix<br/> Used in a group of files or for an indirection file.<br/> For example, FNAME=&lt;Indirection prefix&gt;file name | #  | @  | x'7B’<br/> 285 = #<br/> 297 = £ | x'B1'<br/> 285 = [<br/> 297 = # |
| Character introducing the path name of the FNAME parameter (CFTRECV) from which a tree structure is created.<br/> On Windows, for example, FNAME=pub\+dir1\dir2\ftest | +  | +  | +<br/> Limited to USS files | +<br/> Limited to HFS files |


## Default files used by CFTUTIL


| File | Default<br/> Windows | Unix | z/OS | IBM i |
| --- | --- | --- | --- | --- |
| Parameters file  | $CFTPARM  | $CFTPARM |  $CFTPARM |  +CFTPARM |
| Partners file  | $CFTPART  |  $CFTPART |  $CFTPART |  +CFTPART |
| Catalog file  | $CFTCATA |  $CFTCATA |  $CFTCAT |  +CFTCAT |
| Communication file  | $CFTCOM  |  $CFTCOM |  $CFTCOM |  +CFTCOM |

