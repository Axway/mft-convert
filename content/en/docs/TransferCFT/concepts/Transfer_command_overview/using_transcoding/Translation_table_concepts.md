{
    "title": "Define translation tables",
    "linkTitle": "Using CFTXLATE tables",
    "weight": "360"
}This topic describes the Transfer CFT translation table concept and how to use the corresponding CFTXLATE object to manage machine language translation.

You can use a new translation table when sending or receiving a file, and ONLINE to the data sent as the transmission
proceeds, or OFF LINE to any file using the COPYFILE command.

- Create a new translation table
    -   Delivered tools
    -   Reduced alphabets
- [Define a translation and execute a transfer](#Define)
- [Limitations](#Limitati)
- [CFTXLATE command syntax](../../../../c_intro_userinterfaces/command_summary#CFTXLATE)
- [Code pages and translation tables](#Code2)

Create a new translation table
------------------------------

Begin by creating a translation table file, and modify as needed. You can use the [OS specific delivered tool](#Use2) to help with this task. Or use the following procedure:

Use the `CFTXLATE `command with the `TABLE `parameter. See [Translation tables](#Translat) to understand how to set the TABLE parameter.

**Example**

You can use the following `CFTXLATE `command to generate an ASCII CP437 to EBCDIC CP1047 translation table.

```
CFTXLATE
ID=CP437TOCP1047,FCODE=ASCII,NCODE=EBCDIC,DIRECT=BOTH,TABLE
=00010203372D2E2F1605250B0C0D0E0F-
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
- UNIX/Windows - Use the Axway delivered [xvi utility Use this utility to update a conversion table.]() located in the `home/bin` folder.

    xvi syntax

    xvi [-d &#124; -a &#124; -e &#124; -l &lt;file&gt; ] &lt;table&gt;

    **Standard use**

    xvi &lt;table&gt;: updates a valid existing &lt;table&gt; (256 characters)

    **Advanced use**

    -d: displays a valid existing table in ASCII

    -a: creates a &lt;table&gt; to convert from ASCII to EBCDIC (if CFTXLATE command table exists, it is overwritten)

    -e: creates a &lt;table&gt; to convert from EBCDIC to ASCII (if CFTXLATE command table exists, it is overwritten)

    -l: creates a &lt;table&gt; from an ASCII &lt;file&gt;, generally the file from option -d (if the table exists, it is overwritten)

The correct base table means that a table behaves exactly the same as the current default mapping from EBCDIC to ASCII.

<span id="Reduced"></span>

### Reduced alphabet

You can also manually create a translation table for a reduced alphabet.
The invalid characters are associated with the character &lt;DEL&gt; of
the target alphabet.

<span id="Define"></span>

Define a translation and execute a transfer
-------------------------------------------

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
> Note: If FCODE or NCODE are BINARY no translation
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

### Example of how to create a CFTXLATE

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
    is the default identifier of the {{< TransferCFT/axwayvariablesComponentShortName  >}}  
    (CFTPARM DEFAULT = identifier)

And if all the following conditions are met:

- transfer direction
    corresponding to the DIRECT parameter
- data code specified
    for this file in the FCODE parameter
- network data
    code in the NCODE parameter

<span id="Limitati"></span>

Limitations and usage restrictions
----------------------------------

- {{< TransferCFT/PrimaryCGorUM  >}} **interoperability** Central Governance allows you to use the default translation tables, but not to create new translation tables via the Central Governance user interface. To create and use additional tables, in Transfer CFT  manually create translation tables as described in this section, and refer to them using the XLATE parameter in a SEND or RECV command.
- In the event of file store and forward by partner, there is no translation
    on the intermediate site.
- You cannot override the default translation table when using COPYFILE.

<span id="Code2"></span>

Code pages and translation tables
---------------------------------

The following tables provide basic mapping printable hexadecimal characters for translation as a reference.

<span id="Code"></span>

### Code pages

#### Printable hexadecimal EBCDIC (CP1047)


|   | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | A | B | C | D | E | F |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 4 |   |   | ƒ | „ | … |   | ? | † | ‡ | ¤ | › | . | &lt; | ( | + | &#124; |
| 5 | &amp; | ‚ | ˆ | ‰ | Š | ¡ | Œ | ‹ | ? | á | ! | $ | * | ) | ; | ^ |
| 6 | - | / | ? | Ž | ? | ? | ? | ? | € | ¥ | ? | &quot;,&quot; | % | _ | &gt; | ? |
| 7 | ? | ? | ? | ? | ? | ? | ? | ? | ? | ` | : | # | @ | ' | = | &quot;&quot;&quot;&quot; |
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
| 2 |   | ! | &quot; | # | $ | % | &amp; | ' | ( | ) | * | + | , | - | . | / |
| 3 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | : | ; | &lt; | = | &gt; | ? |
| 4 | @ | A | B | C | D | E | F | G | H | I | J | K | L | M | N | O |
| 5 | P | Q | R | S | T | U | V | W | X | Y | Z | [ | \ | ] | ^ | _ |
| 6 | ` | a | b | c | d | e | f | g | h | i | j | k | l | m | n | o |
| 7 | p | q | r | s | t | u | v | w | x | y | z | { | &#124; | } | ~ |   |
| 8 | Ç | ü | é | â | ä | à | å | ç | ê | ë | è | ï | î | ì | Ä | Å |
| 9 | É | æ | Æ | ô | ö | ò | û | ù | ÿ | Ö | Ü | ¢ | £ | ¥ | P | ƒ |
| A | á | í | ó | ú | ñ | Ñ | ª | º | ¿ | ¬ | ¬ | ½ | ¼ | ¡ | « | » |
| B | ¦ | ¦ | ¦ | ¦ | ¦ | ¦ | ¦ | + | + | ¦ | ¦ | + | + | + | + | + |
| C | + | - | - | + | - | + | ¦ | ¦ | + | + | - | - | ¦ | - | + | - |
| D | - | - | - | + | + | + | + | + | + | + | + | ¦ | _ | ¦ | ¦ | ¯ |
| E | a | ß | G | p | S | s | µ | t | F | T | O | d | 8 | f | e | n |
| F | = | ± | = | = | ( | ) | ÷ | ˜ | ° | · | · | v | n | ² | ¦ |   |


<span id="Translat"></span>

Translation tables
------------------

This section demonstrates how the translation table works. In a translation table, the "s" character takes the value "t":

- "s" is
    the value of the character of the "source" alphabet
- "t" is
    the value of the character of the corresponding "target" alphabet

The first character is in position 0, and the first position is 00. In this example the hexadecimal value 40, which is the EBCDIC blank character, corresponds to the value 20, which is the blank character ASCII blank character.

![](/Images/TransferCFT/temp_translation_table.png)

****Related
topics****

- Command syntax
    [CFTXLATE](../../../../c_intro_userinterfaces/command_summary#CFTXLATE)
- Parameter list
    [CFTXLATE](../../../../c_intro_userinterfaces/web_copilot_ui/flow_def_intro/cftxlate)
- UI operations
    Defining translation tables
