{
    "title": "nfver",
    "linkTitle": "nfver",
    "weight": "2210"
}<span id="nfver"></span>

### nfver

#### RECV

****NFVER = { 0
&#124; n }****

Version of the transferred file.

Version number of the file sent, in the case of a GDG file.

The values represent the following:

- 0: version 0 of
    the file (default)
- n: version -n of
    the file

****Case 2****: NFVER is used alone
(closed mode with implicit sending from the sender server end).

FNAME = &PART.&IDF (-&NFVER).

The partner sends the version of the ‘&PART.&IDF’ logical file
indicated in the NFVER

parameter.

****GDG****

****Case 1****: NFVER is used with NFNAME
(open mode with implicit sending from the sender server end).

****Example****:

****MVS****

FNAME = &FNAME (-&NFVER).

The partner sends the GDG file with the root and the version number
indicated.

****Case 2****: NFVER is used alone
(closed mode with implicit sending from the sender server end)

****Example****:

****MVS****

FNAME = &TEST.GDG (-&NFVER).

The partner sends version of the ‘&PART.&IDF’ file indicated
in the NFVER parameter.

****Note****: if NFVER is not defined,
the default value is 0.

It is consequently recommended to define one send command (CFTSEND)
for each type of file to be processed (normal or with versions).

[Return to Command index](../../)
