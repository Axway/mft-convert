{
    "title": "Platform-specific characters and functions",
    "linkTitle": "Platform-specific characters and functions",
    "weight": "250"
}Certain characters have a unique meaning in commands as well as in the {{< TransferCFT/axwayvariablesComponentShortName  >}} configuration.

On z/OS and IBM i platforms you should check the page code used by your local system. For example, x'5B' is the pound character (£) if you are using the United Kingdom EBCDIC page code 285, but is the dollar character ($) in the French EBCDIC page code 297. For more information, please refer to the Wikipedia [code page](https://en.wikipedia.org/wiki/Code_page).


|   | Description  | Windows  | Unix | z/OS  | IBM i  | OpenVMS  |
| --- | --- | --- | --- | --- | --- | --- |
| char_file  | Logical name prefix | $  | _  | x'5B'<br/> 285 = £<br/> 297 = $ | x'4E'<br/> 285 = +<br/> 297 = + | No specific character;<br/> logical names are<br/> processed transparently by RMS |
| char_mask  | Wildcard character  | ?  | ?  | x' 6F<br/> 285 = ?<br/> 297 = ? | x'6F'<br/> 285 = ?<br/> 297 = ? | %x  |
| char_unit  | Separator character (volume)  | %  | \01  | x' 6C'<br/> 285 = %<br/> 297 = % | x '5E'<br/> 285 = ;<br/> 297 = ; | No volume concept |
| char_symb  | Symbolic variable prefix  | &amp;  | &amp;  | x'50'<br/> 285 = &amp;<br/> 297 = &amp; | x'6F'<br/> 285 = ?<br/> 297 = ? | &amp;  |
| file_symb  | Character introducing a file<br/> name passed to CFTUTIL as<br/> a parameter | #  | @  | x'7B’<br/> 285 = #<br/> 297 = £ | x'B1'<br/> 285 = [<br/> 297 = # | Either # or @  |
| char_directory  | Character introduced in the path<br/> name of the FNAME parameter<br/> (CFTRECV) from which a tree structure can be created | +  | +  | +<br/> Limited to<br/> USS files | +<br/> Limited to<br/> HFS files | +  |
| cont  | Continuation character in CFTUTIL commands  | -  | -  | -  | -  | -  |


> **Note**
>
> Note: OpenVMS is not available for Transfer CFT 3.10 using Flow Manager or Central Governance.
