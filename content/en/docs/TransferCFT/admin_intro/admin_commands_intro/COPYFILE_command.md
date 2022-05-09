{
    "title": "COPYFILE  - Copy files off-line",
    "linkTitle": "COPYFILE - Copy files off-line",
    "weight": "270"
}The COPYFILE command is used
to copy a sequential file performing the following operations:

- Compression/decompression: the compressed file must always have a
    variable format

<!-- -->

- ASCII/EBCDIC translation
    and vice versa: the translation is performed with Transfer CFT’s
    internal tables

<!-- -->

- Record format or
    file type modification

Use the COPYFILE command to copy a sequential file. The COPYFILE command enables a file to compressed offline, before being
sent by Transfer CFT and a file to be decompressed after being received.

****Syntax****

{{% TransferCFT/snippets/copyfile%}}

****Limitation****

When using ICHARSET and OCHARSET, all file types are supported except Binary (B) and Variable (V).


| Parameter  | Description  |
| --- | --- |
| [CREATE](../../../c_intro_userinterfaces/command_summary/parameter_intro/create) | Output file creation option. |
| [IBLKSIZE](../../../c_intro_userinterfaces/command_summary/parameter_intro/iblksize)  | Defines the block size of the input file, in bytes.<br/>  |
| [ICHARSET]()  | Defines the input file encoding.  |
| [ICODE](../../../c_intro_userinterfaces/command_summary/parameter_intro/icode)  | Codes the input file data. Internal code managed by the system, either ASCII, or EBCDIC. |
| [Compression](../../../c_intro_userinterfaces/command_summary/parameter_intro/icomp). |
| [ICT](../../../c_intro_userinterfaces/command_summary/parameter_intro/ict)  | Type of input file data compression. The value of ICOMP must be compatible with the compression type. |
| [IFNAME](../../../c_intro_userinterfaces/command_summary/parameter_intro/ifname)  | Name of the input file to be copied. |
| [see comments](../../../c_intro_userinterfaces/command_summary/parameter_intro/ilrecl)<br/>  | For records of:<br/> • Fixed format (IFRECFM = F): input file record size<br/> • Variable format (IFRECFM = V): maximum record size |
| [IRECFM](../../../c_intro_userinterfaces/command_summary/parameter_intro/irecfm)<br/> see the specific Operations Guide | Input file record format:<br/> • F: fixed<br/> • V: variable<br/> • U: undefined |
| [ITYPE](../../../c_intro_userinterfaces/command_summary/parameter_intro/itype) | Input file type.<br/> Refer to the Operations Guide corresponding to your OS.  |
| [OBLKSIZE](../../../c_intro_userinterfaces/command_summary/parameter_intro/oblksize) OS | Output file block size (in bytes). The value indicated must be greater than the value of the OLRECL parameter. |
| [OCHARSET]()  | Defines the output file encoding.  |
| [OCODE](../../../c_intro_userinterfaces/command_summary/parameter_intro/ocode) | Codes the output file data. |
| [OCOMP](../../../c_intro_userinterfaces/command_summary/parameter_intro/ocomp)  | Compresses the output file data. |
| [OCT](../../../c_intro_userinterfaces/command_summary/parameter_intro/oct) | Type of output file data compression. The value of OCOMP must be compatible with the compression type. |
| [OFNAME ](../../../c_intro_userinterfaces/command_summary/parameter_intro/ofname) | Output file name. |
| [OLRECL](../../../c_intro_userinterfaces/command_summary/parameter_intro/olrecl) | Record formats, expressed in bytes. |
| [ORECFM](../../../c_intro_userinterfaces/command_summary/parameter_intro/orecfm)  | Output record format. |
| [OSPACE](../../../c_intro_userinterfaces/command_summary/parameter_intro/ospace) | Space to be reserved for the output file, in K-bytes (1 K-byte = 1024 bytes). |
| [OTYPE](../../../c_intro_userinterfaces/command_summary/parameter_intro/otype)  | Output file type. |
| [XLATE](../../../c_intro_userinterfaces/command_summary/parameter_intro/xlate)  | Defines the translation table used for the copyfile.  |
|   |   |


### Input and output parameter categories

Parameters beginning with I refer to input files.

Parameters beginning with O refer to the output files.

These two parameters may be classified into 5 sub-categories, as shown
in the following table.


| Parameter category  | Parameter concerned  |
| --- | --- |
| Execution control parameters  | CREATE  |
| Input data processing parameters  | ICT, ICOMP, ICODE, ICHARSET, XLATE |
| Parameters associated with the input file:<br/> • physical name<br/> • physical characteristics (global file)<br/> • physical characteristics (records) |  <br/> <br/> IFNAME<br/> ITYPE<br/> IRECFM, ILRECL, IBLKSIZE  |
| Output data processing parameters  | OCT, OCOMP, OCODE, OCHARSET |
| Parameters associated with the output file:<br/> • physical name<br/> • physical characteristics (global file)<br/> • physical characteristics (records) |  <br/> <br/> OFNAME<br/> OSPACE, OTYPE<br/> ORECFM, OLRECL, OBLKSIZE  |


<span id="Statistics"></span>

Statistics
----------

The utility prints out execution statistics.

The following table indicates the heading contents.


| Heading number  | Contents  |
| --- | --- |
| 1  | Complete name of the input file  |
| 2  | Coding of the input file data  |
| 3  | Input file compression type:<br/> • EXT (PeSIT non-SIT)<br/> • CFT (PeSIT CFT to CFT) |
| 4  | Input file compression value  |
| 5  | Number of records to be copied (input file)  |
| 6  | Input file size in K bytes (1)  |
| 6b  | Input file size in K bytes, if ILRECL is less than the actual size of the record (2)  |
| 7  | Complete name of the output file  |
| 8  | Coding of the output file data  |
| 9  | Compression type:<br/> • EXT (PeSIT non-SIT)<br/> • CFT (PeSIT CFT to CFT) |
| 10  | Output file compression value  |
| 11  | Output file size in K bytes  |
| 11b  | Output file size in K bytes, if padding (OLRECL &gt; ILRECL)  |
| 12  | Compression rate performed when copying the file  |
| 12b  | Expansion rate performed when copying the file  |


(1) (2): Headings 6 and 6b are mutually
exclusive. If heading 6b is displayed, heading 6 is not and vice versa.
