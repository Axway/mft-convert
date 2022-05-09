{
    "title": "ilrecl",
    "linkTitle": "ilrecl",
    "weight": "1630"
}<span id="ilrecl"></span>

### ilrecl

#### COPYFILE

****[ILRECL = {see the comment
&#124; n} ]   ****

****{0..32768}****

For records of:

- Fixed format (IFRECFM = F): input file record
    size
- Variable format (IFRECFM = V): maximum record
    size

ILRECL is expressed in bytes.

If ILRECL is less than the actual record length and if:

- The input file is compressed, processing is aborted
- The input file is not compressed, the record is
    truncated to the supplied length

If ILRECL is greater than the actual length of the record to be written
and if:

- The file is of fixed format (IRECFM = F), the
    record is padded with "spaces", i.e. according to the value
    of the ICODE parameter:
- x‘20’ if ICODE = ASCII
- x‘40’ if ICODE = EBCDIC
- The file is not of fixed format, this parameter
    is ignored.

*Default values:* for systems handling
the file record concept: actual size of the input file records.

For the systems below not handling this concept:


| **UNIX, Windows** | 512 for text files (ITYPE = T, X or O).<br /> 4096 for binary or variables files (ITYPE = {B &#124; V}). |
| --- | --- |


 

[Return to Command index](../../)
