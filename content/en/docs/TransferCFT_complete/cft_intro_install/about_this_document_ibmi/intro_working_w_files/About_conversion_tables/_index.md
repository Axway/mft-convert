{
    "title": "Conversion tables ",
    "linkTitle": "Conversion tables",
    "weight": "210"
}This section describes how to use a conversion table in Transfer CFT {{< TransferCFT/PrimaryForOS400  >}} in the following sections:

- Using a conversion table
- Configuration sample

Using conversion tables
-----------------------

During Transfer CFT operations conversion problems may occur when:

- A file to be transferred contains various special characters:  
          &#124;, !, \\, `, \#, ~, [, ], ^, {, }, /, $ and £

<!-- -->

- The transfer is performed between two heterogeneous systems with different character sets (CCSID) and the default conversion fails.

<!-- -->

- The transfer is performed between international sites.

### Default values

The default EBCDIC character set used by Transfer CFT has code 297 (EBCDIC France).

The default ASCII character set used by Transfer CFT is not fully compatible with code 850 (IBM multilingual personal computer). For more information refer to the *[Transfer CFT User Guide](../../../../concepts/transfer_command_overview/using_transcoding/use_extended_character_sets)*.

Consequently, two files supplied in the production library are used to enter and create a conversion table in Transfer CFT:

- TABEBAS: file to be used to convert EBCDIC into ASCII (generally for send operations)

<!-- -->

- TABASEB: file to be used to convert ASCII into EBCDIC (generally for receive operations)

These two files can be modified by DFU (Option 18 in PDM - Member Management).

### Creating the conversion table

To create the actual conversion table, you must run the make_tcd.c utility program after modifying the characters at fault: `call make_tcd.c parm('CFTPROD/tabaseb')`

The CFTPROD/tabaseb.x binary file is created: it constitutes the conversion table to be specified in the Transfer CFT configuration. The same applies to tabebas.x.

Configuration sample
--------------------

The following is a full Transfer CFT {{< TransferCFT/PrimaryForOS400  >}} configuration sample for a Windows system, which is a typical and frequent scenario.

```
Transfer CFT IBM i configuration
CFTXLATE MODE=REPLACE,
         ID=TABASEB,
         DIRECT=RECV,
         FNAME=CFTPROD/TABASEB.X
CFTXLATE MODE=REPLACE,
         ID=TABEBAS,
         DIRECT=SEND,
         FNAME=CFTPROD/TABEBAS.X
CFTSEND MODE=REPLACE, ID=……..,
         XLATE=TABEBAS,
          …………………….
 
CFTRECV MODE=REPLACE, ID=………,
         XLATE=TABASEB,
          …………………….
 
Transfer CFT WIN/NT configuration
============
 
cftrecv  id       = …………,
        fcode    = binary,  /\* to avoid needing conversion \*/
         …………………
 
cftsend  id       = …………,
        fcode    = binary,  /\* to avoid needing conversion \*/
         ………………….
```
