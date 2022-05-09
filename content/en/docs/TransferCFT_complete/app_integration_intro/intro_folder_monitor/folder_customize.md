{
    "title": "Create inclusion and exclusion filters",
    "linkTitle": "Create inclusion and exclusion filters",
    "weight": "190"
}<span id="Defining"></span>

This topic described some of the more advanced parameter settings for Transfer CFT folder monitoring.

You can use the file_include_filter or file_exclude_filter parameters to define file name patterns to include or exclude files from folder monitoring.

The filter_type parameter value indicates how the comparison of file names against the patterns occurs. The possible filter_type parameter values are **STRJCMP**, **WILDMAT**. and ****EREGEX****.

{{% TransferCFT/snippets/strjcmp_filter%}}
<span id="WILDMAT"></span>

WILDMAT filter
--------------

**Unix/Windows only**

The WILDMAT pattern-matching filter offers more operations than the STRJCMP filter_type. The WILDMAT filter characters are interpreted as follows, where x and y are used to indicate any character:


| Character  | Description  | Example  |
| --- | --- | --- |
| *  | Indicates any sequence of zero or more characters.  | The filter &quot;*.dat&quot; selects any file name that has the extension &quot;.dat&quot;.  |
| ?  | Indicates any single character.  | The filter &quot;T*.???&quot; selects any file name starting with a 'T' and having an extension of exactly three characters.  |
| \x  | Indicates that when x is a special character, x is interpreted as a normal character.  | This is generally used to invalidate the meaning of the * and ? characters.  |
| [x...y]  | Indicates a single character set defined by &quot;x...y&quot;.  | The filter [0-9]<br/> indicates any decimal digit. |
| -  | Indicates a range of characters. However, the minus character (or hyphen) has no special meaning if it is either the first or the last character in the set. | The filter [0-9a-zA-Z] indicates any alphanumeric character (in English).  |
| ]  | Has no special meaning if it is the first character in the set.  |   |
| [^x...y]  | Indicates any character other than those specified in the set &quot;x...y&quot;.  | The filter [^0-9]<br/> indicates any character that is not a decimal digit.<br/> The filter [^]-] indicates any character other than a closed bracket or minus sign. |
| *.[tT][xX][tT]  | Indicates any string terminated by .TXT regardless of the case.  |   |


{{% TransferCFT/snippets/eregex_filter%}}

Use case
--------

The EREGEX examples describe how to create multiple exclusions using the `INCLUDEFILTER `and `EXCLUDEFILTER `parameters.

### EREGEX example 1

In this example, the following files are not sent from the specified folder – that is, the following files are excluded:

- out\*.ffs_lock
- out\*.jbase_header
- out\*.ffs_db

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
EXCLUDEFILTER= '^out.\*\\.((ffs_lock)&#124;(jbase_header)&#124;(ffs_db))',
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
INCLUDEFILTER='^c+' \#'^c+'
EXCLUDEFILTER='(^ca$&#124;^cb$)'
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
