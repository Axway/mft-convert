{
    "title": "nblksize",
    "linkTitle": "nblksize",
    "weight": "2100"
}<span id="nblksize"></span>

### nblksize

#### CFTSEND, SEND

**[NBLKSIZE = {<span class="underline">value of FBLKSIZE</span>
&#124; n}]**

File block size in protocol terms.

May be used to transfer a file to a partner whose system manages the
block size concept (the list of these systems is indicated in the FBLKSIZE
parameter).

****z/OS (MVS) only - PeSIT, PeSIT CFT/CFT****

Use this field to define the block size of the file. Make sure that
the partner supports the block size concept.

Enter one of the following values:

- the
    value of fblksize (see the z/OS (MVS)-specific
    tab)
- a value from 0 to 65535

[Return to Command index](../../)
