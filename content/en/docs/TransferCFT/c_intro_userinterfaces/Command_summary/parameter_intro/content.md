---

    title: content
    linkTitle: content
    weight: 510

---
<span id="content"></span>

### content

<span id="content_CFTLOG"></span>

#### CFTLOG

**\[CONTENT = {<u>FULL</u> | BRIEF }\]**

The messages written in the active
LOG file are filtered.

The possible values are:

- FULL/ACTIVE: All messages
    are printed out including STAT
- BRIEF: The following
    messages do not appear in the LOG (these are messages of the "system
    information" category - see the CFTLOG [OPERMSG](../opermsg) parameter):  
    CFTR12I &cmd Treated  
    CFTC07I PART=&part IDF=&idf IDT=&idt State=&state Deleted
- Requester mode:  
    CFTT53I PART=&part IDF=&idf IDT=&idt Requester file selected  
    CFTT55I PART=&part IDF=&idf IDT=&idt Requester file opened  
    CFTT51I PART=&part IDF=&idf IDT=&idt Requester session
    opened  
    CFTT56I PART=&part IDF=&idf IDT=&idt Requester file closed  
    CFTT54I PART=&part IDF=&idf IDT=&idt Requester file deselect  
    CFTT52I PART=&part IDF=&idf IDT=&idt Requester session
    close

<!-- -->

- Server mode:  
    CFTT51I PART=&part IDF=&idf IDT=&idt Server session opened  
    CFTT53I PART=&part IDF=&idf IDT=&idt Server file created  
    CFTT55I PART=&part IDF=&idf IDT=&idt Server file opened  
    CFTT56I PART=&part IDF=&idf IDT=&idt Server file closed  
    CFTT54I PART=&part IDF=&idf IDT=&idt Server file deselect

All error or warning messages are printed out.

For a transfer without incident, the LOG file will contain the following
two messages:

- Requester mode:  
    CFTT57I PART=&part IDF=&idf IDT=&idt Requester transfer
    started  
    CFTT58I PART=&part IDF=&idf IDT=&idt Requester transfer
    ended
- Server mode:  
    CFTT57I PART=&part IDF=&idf IDT=&idt Server transfer started  
    CFTT58I PART=&part IDF=&idf IDT=&idt Server transfer ended

<span id="content_LISTCAT"></span>

#### LISTCAT

****\[CONTENT = { <u>BRIEF</u>
| FULL | DEBUG | COMMUT | EXTEND | | BLKNUM} \]****

Used to obtain part or all of the information of a catalog entry.

The possible values are:

- <span style="font-weight: bold;">****BRIEF****</span>: Displays the most basic, essential information
    concerning the selected transfers with one line per transfer
- <span style="font-weight: bold;">****EXTEND****</span>: Displays information concerning
    security, exits and end-of-transfer procedures, as well as the BRIEF type
    information with one line per transfer
- **COMMUT**: Displays a sort of BRIEF output, but contains some network-oriented details
- <span style="font-weight: bold;">****FULL****</span>: Displays complete information concerning
    each transfer
- <span style="font-weight: bold;">****DEBUG****</span>: Displays the most complete output with additional information beyond the FULL content
- **BLKNUM**: Displays the same information as BRIEF, but the **Appli id** and **Appstate** columns are replaced by the **blknum** column

#### LISTCOM

**\[CONTENT = {<span style="text-decoration: underline;"> ACTIVE</span> |
FULL }\]**

Used to obtain part or all of the
information of a catalog entry.

- ACTIVE: Displays only communication records that are not empty
- FULL: Displays all communication records

<span class="bold_in_para">****Example****</span>

```
LISTCOM CONTENT =FULL
 
CFTU00I LISTCOM _ Correct (content=full)
CFTU00I SEND _ Correct (part=paris,idf=txt,fname=pche.dv.mvtx4)
CFTU20I Communication file row number used: 00001137 on 20191212 Time 09331119
CFTU00I RETURN _ Correct (CODE=0)
 
1 record(s) selected
0 record(s) cleared
1500 record(s) in Com file
1499 record(s) free (99%)
```

#### LISTPARM, LISTPART

******\[CONTENT =
{<u>FULL</u> | BRIEF}\]******

Used to obtain part or all of the
information.

#### CFTEXT

****\[CONTENT =
{<u>FULL</u> | BRIEF}\]****

Level of content included in output:

- BRIEF = Empty or default value parameters are omitted
- FULL = All parameters are included

#### MQUERY (OBJECT=CACHE or SYSTEM)

******\[CONTENT =
{ <span style="text-decoration: underline;">BRIEF</span>
| FULL | STAT } \]******

Used to obtain part or all of the
information.

#### MQUERY (OBJECT=PROBE or STATS)

******\[CONTENT =
{ <span style="text-decoration: underline;">XMLBRIEF</span>
| XMLFULL | RAW } \]******

Used to obtain part or all of the
information.

#### DISPLAY

******\[CONTENT =
listcat\]******

#### LISTUCONF, UCONFGET

******\[CONTENT= { BRIEF | FULL | EXTRACT | DEBUG | PROPS }\]******

Indicates the amount of information to display. Here, the options are listed in order of increasing details that are displayed.

- PROPS: Delivers a simple property-like output
- BRIEF: Displays basic information
    concerning the UCONF value (the set value and if this is a user or default value)
- EXTRACT: Outputs information in format that you can directly enter in CFTUTIL
- FULL: Displays values, descriptions, and most other parameter details
- DEBUG: Most exhaustive level of information (including some internal flags/information)

[Return to Command index](../../)
