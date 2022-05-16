---

    title: Define translation tables
    linkTitle: Conversion tables CFTXLATE
    weight: 190

---
This topic describes the Transfer CFT translation table concept and how to use the corresponding CFTXLATE object to manage machine language translation. You can create and use a translation table between 2 computers each
using a different alphabet. Each table is built using a file containing a unique 256-character
record, where through its position and value each character defines
the correspondence between two alphabets.

You can use a new translation table when sending or receiving a file, and ONLINE to the data sent as the transmission
proceeds, or OFF LINE to any file using the COPYFILE command.

- [Create a new translation table](#Create)
    -   Delivered tools
    -   Reduced alphabets
- [Define a translation and execute a transfer](#Define)
- [Limitations](#Limitati)
- [CFTXLATE command syntax](../../../c_intro_userinterfaces/command_summary#CFTXLATE)
- [Code pages and translation tables](#Code2)
- Default translation tables

<span id="Create"></span>

## Default translation tables

By default, {{< TransferCFT/axwayvariablesComponentShortName  >}} provides 4 internal ASCII/EBCDIC translation
tables, two for each transfer direction. These tables are bijective. These correspond loosely to translation between code pages EBCDIC 1047 and ASCII 437, with some small differences.

## Create a new translation table

Begin by creating a translation table file, and modify as needed. You can use the [OS specific delivered tool](#Use2) to help with this task. Or use the following procedure:

1. Create a file as shown in the following example, containing hexadecimal characters from 00 to FF.

```
00 01 02 03 04 05 06 07 - 08 09 0A 0B 0C 0D 0E 0F
10 11 12 13 14 15 16 17 - 18 19 1A 1B 1C 1D 1E 1F
20 21 22 23 24 25 26 27 - 28 29 2A 2B 2C 2D 2E 2F
30 31 32 33 34 35 36 37 - 38 39 3A 3B 3C 3D 3E 3F
40 41 42 43 44 45 46 47 - 48 49 4A 4B 4C 4D 4E 4F
50 51 52 53 54 55 56 57 - 58 59 5A 5B 5C 5D 5E 5F
60 61 62 63 64 65 66 67 - 68 69 6A 6B 6C 6D 6E 6F
70 71 72 73 74 75 76 77 - 78 79 7A 7B 7C 7D 7E 7F
80 81 82 83 84 85 86 87 - 88 89 8A 8B 8C 8D 8E 8F
90 91 92 93 94 95 96 97 - 98 99 9A 9B 9C 9D 9E 9F
A0 A1 A2 A3 A4 A5 A6 A7 - A8 A9 AA AB AC AD AE AF
B0 B1 B2 B3 B4 B5 B6 B7 - B8 B9 BA BB BC BD BE BF
C0 C1 C2 C3 C4 C5 C6 C7 - C8 C9 CA CB CC CD CE CF
D0 D1 D2 D3 D4 D5 D6 D7 - D8 D9 DA DB DC DD DE DF
E0 E1 E2 E3 E4 E5 E6 E7 - E8 E9 EA EB EC ED EE EF
F0 F1 F2 F3 F4 F5 F6 F7 - F8 F9 FA FB FC FD FE FF
```

You can use the following `CFTXLATE `command to generate an ASCII CP437 to EBCDIC CP1047 translation table.

```
CFTXLATE ID=CP437TOCP1047,FCODE=ASCII,NCODE=EBCDIC,DIRECT=BOTH,TABLE=00010203372D2E2F1605250B0C0D0E0F-
10111213B6B5322618191C27071D1E1F-
405A7F7B5B6C507D4D5D5C4E6B604B61-
F0F1F2F3F4F5F6F7F8F97A5E4C7E6E6F-
7CC1C2C3C4C5C6C7C8C9D1D2D3D4D5D6-
D7D8D9E2E3E4E5E6E7E8E9ADE0BD5F6D-
79818283848586878889919293949596-
979899A2A3A4A5A6A7A8A9C04FD0A13F-
68DC5142434447485253545756586367-
719C9ECBCCCDDBDDDFECFC4AB1B2BFFF-
4555CEDE49699A9BABAFB0B8B7AA8A8B-
2B2C092128656264B438313433708024-
22172906202A46661A35083936303A9F-
8CAC7273740A757677231514046A783B-
EE59EBEDCFEFA08EAEFEFBFD8DBABCBE-
CA8F1BB93C3DE19D90BBB3DAFAEA3E41
```

In the Transfer CFT UI or Swagger REST API, you can use the format:

```
CFTXLATE ID=CP437TOCP1047,FCODE=ASCII,NCODE=EBCDIC,DIRECT=BOTH,TABLE=00010203372D2E2F1605250B0C0D0E0F-
00010203372D2E2F1605250B0C0D0E0F10111213B6B5322618191C27071D1E1F405A7F7B5B6C507D4D5D5C4E6B604B61F0F1F2F3F4F5F6F7F8F97A5E4C7E6E6F7CC1C2C3C4C5C6C7C8C9D1D2D3D4D5D6D7D8D9E2E3E4E5E6E7E8E9ADE0BD5F6D79818283848586878889919293949596979899A2A3A4A5A6A7A8A9C04FD0A13F68DC5142434447485253545756586367719C9ECBCCCDDBDDDFECFC4AB1B2BFFF4555CEDE49699A9BABAFB0B8B7AA8A8B2B2C092128656264B43831343370802422172906202A46661A35083936303A9F8CAC7273740A757677231514046A783BEE59EBEDCFEFA08EAEFEFBFD8DBABCBECA8F1BB93C3DE19D90BBB3DAFAEA3E41
```

Or alternatively, you can use the following `CFTXLATE `command to generate a ASCII CP850 to EBCDIC CP1047 translation table.

```
CFTXLATE ID=CP850T0CP1047,FCODE=ASCII,NCODE=EBCDIC,DIRECT=BOTH,TABLE=00010203372D2E2F1605250B0C0D0E0F-
10111213B6B5322618191C27071D1E1F-
405A7F7B5B6C507D4D5D5C4E6B604B61-
F0F1F2F3F4F5F6F7F8F97A5E4C7E6E6F-
7CC1C2C3C4C5C6C7C8C9D1D2D3D4D5D6-
D7D8D9E2E3E4E5E6E7E8E9ADE0BD5F6D-
79818283848586878889919293949596-
979899A2A3A4A5A6A7A8A9C04FD0A13F-
68DC5142434447485253545756586367-
719C9ECBCCCDDBDDDFECFC4AB1B2BFFF-
4555CEDE49699A9BABAFB0B8B7AA8A8B-
2B2C092128656264B438313433708024-
22172906202A46661A35083936303A9F-
8CAC7273740A757677231514046A783B-
EE59EBEDCFEFA08EAEFEFBFD8DBABCBE-
CA8F1BB93C3DE19D90BBB3DAFAEA3E41
```
<span id="Use2"></span>

### Delivered tools

Transfer CFT is delivered with a platform specific tool to help you create an XLATE table.

- Mainframes - A JCL is provided to help with creating both local and remote tables. Refer to the CFTXLATE member in the installation library.

- UNIX/Windows - Use the Axway delivered <a href="" class="MCTextPopup popup popupHead">xvi utility<span class="MCTextPopupBody MCTextPopupBody_Closed needs-pie popupBody" aria-hidden="true"><span class="MCTextPopupArrow"> </span>Use this utility to update a conversion table.
    </span></a> located in the <span class="code">`home/bin`</span> folder.

    xvi syntax

    xvi \[-d | -a | -e | -l &lt;file> \] &lt;table>

    **Standard use**

    xvi &lt;table>: updates a valid existing &lt;table> (256 characters)

    **Advanced use**

    -d: displays a valid existing table in ASCII

    -a: creates a &lt;table> to convert from ASCII to EBCDIC (if CFTXLATE command table exists, it is overwritten)

    -e: creates a &lt;table> to convert from EBCDIC to ASCII (if CFTXLATE command table exists, it is overwritten)

    -l: creates a &lt;table> from an ASCII &lt;file>, generally the file from option -d (if the table exists, it is overwritten)

The correct base table means that a table behaves exactly the same as the current default mapping from EBCDIC to ASCII.

<span id="Reduced"></span>

### Reduced alphabet

You can also manually create a translation table for a reduced alphabet.
The invalid characters are associated with the character &lt;DEL> of
the target alphabet.

<span id="Define"></span>

## Define a translation and execute a transfer

Use the CFTXLATE object to define translation tables between 2 alphabets. A definition includes:

- Transfer direction (DIRECT: SEND, RECV, or BOTH)
- File data code
    type (FCODE: ASCII or EBCDIC)
- Data network
    code type (NCODE: ASCII or EBCDIC)
- Translation table file name (FNAME)

If DIRECT = SEND (or = BOTH), the file (FNAME) contains the description
of the send translation table (FCODE to NCODE) and if DIRECT = RECV (or
= BOTH), it contains the description of the receive table (NCODE to FCODE).

> **Note**
>
> If FCODE or NCODE are BINARY no translation
> takes place.

The identifier
of these tables is the value of the DEFAULT parameter of the CFTPARM command.
It is, however, possible to replace, modify or delete these internal tables
by CFTXLATE commands having this identifier. When {{< TransferCFT/axwayvariablesComponentShortName  >}} does not find an associated table for these 4 transfer
criteria, it looks for the default table (value of the DEFAULT
parameter of the CFTPARM command, for the XLATE parameter).

If the values of the FCODE and NCODE parameters are different, Transfer
CFT has to find a valid translation table. It is consequently not authorized
to delete a default translation table.

### Create a new CFTXLATE

Create the following new object, which you can use instead of the default in file transfers. In this example, for a given partner you want to use explicitly this translation table.

```
cftxlate id=new, fcode=ebcdic, ncode=ascii, direct=both, mode=create, fname=cp1047tocp437
```

If you created a translation table that maps ASCII to ASCII, for example, remember to set both the ncode and fcode to ASCII.

```
cftxlate id=new, fcode=ascii, ncode=ascii, direct=both, mode=create, fname=cp850tocp437
```

### Override default CFTXLATE

Create the following objects one for each translation direction. These new objects override the default translation table.

```
cftxlate id=bin, fcode=ebcdic, ncode=ascii, direct=both, mode=create, fname=cp1047tocp437cftxlate id=bin, fcode=ascii, ncode=ebcidic, direct=both, mode=create, fname=cp437tocp1047
```

### Execute the transfer command

Translation tables are used if FCODE and NCODE are not set to BINARY. When sending a file, Transfer CFT translates FCODE to NCODE. When receiving a file, Transfer CFT translates from NCODE to FCODE.

In the following example, the translation table that is specified by the xlate parameter is used instead of the default translation table.

```
send part=paris, idf=myflow, xlate=cp437tocp1047, fcode=ascii, ncode=ebcdic
```

However in the following example the default translation tables are used, provided the send template model does not include an xlate definition.

```
send part=paris,idf=myflow, fcode=ascii, ncode=ebcdic
```

A translation table is used if one of the following conditions are met, in the
order of priority indicated:

- the table identifier
    has been indicated in the SEND/CFTSEND command  
    (SEND XLATE = identifier) or RECV/CFTRECV command
- the table identifier
    has been indicated in the CFTPART command  
    (CFTPART XLATE = identifier)
- the table identifier
    is the default identifier of the {{< TransferCFT/axwayvariablesComponentShortName >}}  
    (CFTPARM DEFAULT = identifier)

And if all the following conditions are met:

- transfer direction
    corresponding to the DIRECT parameter
- data code specified
    for this file in the FCODE parameter
- network data
    code in the NCODE parameter

<span id="Limitati"></span>

## Limitations and usage restrictions

- {{< TransferCFT/PrimaryCGorUM >}} **interoperability** Central Governance allows you to use the default translation tables, but not to create new translation tables via the Central Governance user interface. To create and use additional tables, in Transfer CFT  manually create translation tables as described in this section, and refer to them using the XLATE parameter in a SEND or RECV command.
- In the event of file store and forward by partner, there is no translation
    on the intermediate site.
- You cannot override the default translation table when using COPYFILE.

<span id="Code2"></span>

## Code pages and translation tables

The following tables provide basic mapping printable hexadecimal characters for translation as a reference.

<span id="Code"></span>

### Code pages

#### Printable hexadecimal EBCDIC (CP1047)


|   | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | A | B | C | D | E | F |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 4 |   |   | ƒ | „ | … |   | ? | † | ‡ | ¤ | › | . | &lt; | ( | + | | |
| 5 | &amp; | ‚ | ˆ | ‰ | Š | ¡ | Œ | ‹ | ? | á | ! | $ | * | ) | ; | ^ |
| 6 | - | / | ? | Ž | ? | ? | ? | ? | € | ¥ | ? | "," | % | _ | &gt; | ? |
| 7 | ? | ? | ? | ? | ? | ? | ? | ? | ? | ` | : | # | @ | ' | = | """" |
| 8 | ? | a | b | c | d | e | f | g | h | i | ® | ¯ | ? | ? | ? | ñ |
| 9 | ø | j | k | l | m | n | o | p | q | r | ¦ | § | ‘ | ? | ’ | ? |
| A | æ | ~ | s | t | u | v | w | x | y | z | ­ | ¨ | ? | [ | ? | ? |
| B | ª | œ | ? | ú | ? | ? | ? | ¬ | « | ? | ? | ? | ? | ] | ? | ? |
| C | { | A | B | C | D | E | F | G | H | I | - | “ | ” | • | ¢ | ? |
| D | } | J | K | L | M | N | O | P | Q | R | ? | – | ? | — | £ | ˜ |
| E | \ | ö | S | T | U | V | W | X | Y | Z | ý | ? | ™ | ? | ? | ? |
| F | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | ? | ? | š | ? | ? |   |


#### Printable hexadecimal ASCII (CP437)


|   | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | A | B | C | D | E | F |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 2 |   | ! | " | # | $ | % | &amp; | ' | ( | ) | * | + | , | - | . | / |
| 3 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | : | ; | &lt; | = | &gt; | ? |
| 4 | @ | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O |
| 5 | P | Q | R | S | T | U | V | W | X | Y | Z | [ | \ | ] | ^ | _ |
| 6 | ` | a | b | c | d | e | f | g | h | i | j | k | l | m | n | o |
| 7 | p | q | r | s | t | u | v | w | x | y | z | { | | | } | ~ |   |
| 8 | Ç | ü | é | â | ä | à | å | ç | ê | ë | è | ï | î | ì | Ä | Å |
| 9 | É | æ | Æ | ô | ö | ò | û | ù | ÿ | Ö | Ü | ¢ | £ | ¥ | P | ƒ |
| A | á | í | ó | ú | ñ | Ñ | ª | º | ¿ | ¬ | ¬ | ½ | ¼ | ¡ | « | » |
| B | ¦ | ¦ | ¦ | ¦ | ¦ | ¦ | ¦ | + | + | ¦ | ¦ | + | + | + | + | + |
| C | + | - | - | + | - | + | ¦ | ¦ | + | + | - | - | ¦ | - | + | - |
| D | - | - | - | + | + | + | + | + | + | + | + | ¦ | _ | ¦ | ¦ | ¯ |
| E | a | ß | G | p | S | s | µ | t | F | T | O | d | 8 | f | e | n |
| F | = | ± | = | = | ( | ) | ÷ | ˜ | ° | · | · | v | n | ² | ¦ |   |


<span id="Translat"></span>

### Translation tables

This section demonstrates how the translation table works. In a translation table, the "s" character takes the value "t":

- "s" is
    the value of the character of the "source" alphabet
- "t" is
    the value of the character of the corresponding "target" alphabet

The first character is in position 0, and the first position is 00. In this example the hexadecimal value 40, which is the EBCDIC blank character, corresponds to the value 20, which is the blank character ASCII blank character.

![](/Images/TransferCFT/temp_translation_table.png)

****<span style="color: #800000; text-decoration: none; font-weight: normal;">Related
topics</span>****

- Command syntax
    [CFTXLATE](../../../c_intro_userinterfaces/command_summary#CFTXLATE)
- Parameter list
    [CFTXLATE](../../../c_intro_userinterfaces/about_cftutil/configuring_cft_start_here/cftxlate)
- UI operations
    Defining translation tables
