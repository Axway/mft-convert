---

    title: Create inclusion and exclusion filters
    linkTitle: Create inclusion and exclusion filters
    weight: 210

---
<span id="Defining"></span>

This topic described some of the more advanced parameter settings for Transfer CFT folder monitoring.

You can use the file\_include\_filter or file\_exclude\_filter parameters to define file name patterns to include or exclude files from folder monitoring.

The filter\_type parameter value indicates how the comparison of file names against the patterns occurs. The possible filter\_type parameter values are **STRJCMP**, **WILDMAT**. and <span class="bold_in_para">****EREGEX****</span>.

## STRJCMP filter

A STRJCMP pattern-matching filter can contain the asterisk (\*) and/or the question mark (?) characters. The STRJCMP filter characters are interpreted as follows:


| Character  | Description  | Example  |
| --- | --- | --- |
| *  | Indicates any sequence of zero or more characters.  | The filter "*.dat" selects any file name that has the extension ".dat".  |
| ?  | Indicates any single character.  | The filter "T*.???" selects any file name starting with a 'T' and having an extension of exactly three characters.  |


<span id="WILDMAT"></span>

## WILDMAT filter

**Unix/Windows only**

The WILDMAT pattern-matching filter offers more operations than the STRJCMP filter\_type. The WILDMAT filter characters are interpreted as follows, where x and y are used to indicate any character:


| Character  | Description  | Example  |
| --- | --- | --- |
| *  | Indicates any sequence of zero or more characters.  | The filter "*.dat" selects any file name that has the extension ".dat".  |
| ?  | Indicates any single character.  | The filter "T*.???" selects any file name starting with a 'T' and having an extension of exactly three characters.  |
| \x  | Indicates that when x is a special character, x is interpreted as a normal character.  | This is generally used to invalidate the meaning of the * and ? characters.  |
| [x...y]  | Indicates a single character set defined by "x...y".  | The filter [0-9]<br/> indicates any decimal digit. |
| -  | Indicates a range of characters. However, the minus character (or hyphen) has no special meaning if it is either the first or the last character in the set. | The filter [0-9a-zA-Z] indicates any alphanumeric character (in English).  |
| ]  | Has no special meaning if it is the first character in the set.  |   |
| [^x...y]  | Indicates any character other than those specified in the set "x...y".  | The filter [^0-9]<br/> indicates any character that is not a decimal digit.<br/> The filter [^]-] indicates any character other than a closed bracket or minus sign. |
| *.[tT][xX][tT]  | Indicates any string terminated by .TXT regardless of the case.  |   |


## EREGEX filter

EREGEX (extended regular expressions) is the use of special characters and strings to define a search pattern. In Transfer CFT, you can use these search patterns to create filters.

In POSIX-Extended regular expressions, all characters match themselves meaning they match a sub-string anywhere inside the string to be searched. For example *abc*, matches abc123, 123abc, and 123abcxyz. Some symbols are exceptions though; commonly used symbols and example usages are listed in the following table.


| Symbol  | Indicates  | Example  |
| --- | --- | --- |
| .  | Any character except newline (line break)  | *a.c* matches abc  |
| [ ]  | Or  | *[def]* means d or e or f  |
| {}  | Exactly  | *{3}* means exactly three  |
| ()  | Capture group  | *pand(ora|467)* matches pandora OR pand467  |
| *  | 0 or more occurrences of the preceding element  | *ab*c* matches ac, abc, abbc, abbbc, and so on |
| +  | 1 or more occurrences of the preceding element  | *ab+c* matches abc, abbc, abbbc, and so on, but not ac  |
| ?  | Zero or one occurrence of the preceding element  | *plurals?* matches plural  |
| |  | Alternation (matches either the right side or the left) / OR operand  | *ab|cd|ef* matches ab or cd or ef  |
| ^  | Start of a string  | *^a* matches any file that starts with an a  |
| [^ ...]  | Any single character that is **not** in the class  | *[^/]** matches zero or more occurrences of any character that is not a forward-slash, such as http://  |
| $  | End of string  | *.*? the end$* matches this is the end  |


> **Note**
>
> EREGEX refers to POSIX Extended Regular Expression. There are multiple tutorials available online to aid in creating search patterns; for additional information on expression syntax please refer to Regular expressions.

## Use case

The EREGEX examples describe how to create multiple exclusions using the <span class="code">`INCLUDEFILTER `</span>and <span class="code">`EXCLUDEFILTER `</span>parameters.

### EREGEX example 1

In this example, the following files are not sent from the specified folder – that is, the following files are excluded:

- out\*.ffs\_lock
- out\*.jbase\_header
- out\*.ffs\_db

```
CFTFOLDER ID = 'E',
STATE = 'ACTIVE',
METHOD = 'MOVE',
RESUBMITCHANGES= 'YES',
ENABLESUBDIR= 'YES',
FILEIDLEDELAY= '0',
IDF = 'IDFDEF',
PART = 'PARIS',
SCANDIR = 'e',
WORKDIR = 'e_work',
INTERVAL = '60',
FILECOUNT = '0',
FILESIZEMIN = '0',
FILESIZEMAX = '0',
FILTERTYPE = 'EREGEX',
/\* INCLUDEFILTER= '',\*/
EXCLUDEFILTER= '^out.\*\\.((ffs_lock)|(jbase_header)|(ffs_db))',
RENAMEMETHOD= 'NONE',
RENAMESEPARATOR= '.',
USEFSEVENTS = 'YES',
MODE = 'REPLACE'
```

### EREGEX example 2

Create a filter to include all files that begin with the letter "c" and are followed by any number characters except the ca or cb files.

```
CFTFOLDER ID = 'F',
STATE = 'ACTIVE',
METHOD = 'MOVE',
....
FILTERTYPE = 'EREGEX',
INCLUDEFILTER='^c+' #'^c+'
EXCLUDEFILTER='(^ca$|^cb$)'
...
```

### EREGEX example 3

Create a filter that includes all .jpg files that are:

- A word (at least one other word character)
- Followed by four-digit number

For example, the following filter would include IMGP0122.jpg.

```
CFTFOLDER ID = 'J',
STATE = 'ACTIVE',
METHOD = 'MOVE',
....
FILTERTYPE = 'EREGEX',
INCLUDEFILTER='^[0-9a-zA-Z]+[0-9]{4}.jpg'
...
```
