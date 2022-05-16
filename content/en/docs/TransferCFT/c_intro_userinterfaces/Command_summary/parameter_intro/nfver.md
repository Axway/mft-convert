---

    title: nfver
    linkTitle: nfver
    weight: 2190

---
<span id="nfver"></span>

### nfver

#### RECV

****NFVER = { <span style="text-decoration: underline;">0</span>
| n }****

Version of the transferred file.

Version number of the file sent, in the case of a GDG file.

The values represent the following:

- 0: version 0 of
    the file (default)
- n: version -n of
    the file

<span style="font-weight: bold;">****Case 2****</span>: NFVER is used alone
(closed mode with implicit sending from the sender server end).

FNAME = &PART.&IDF (-&NFVER).

The partner sends the version of the ‘&PART.&IDF’ logical file
indicated in the NFVER

parameter.

<span style="font-weight: bold;">****GDG****</span>

<span style="font-weight: bold;">****Case 1****</span>: NFVER is used with NFNAME
(open mode with implicit sending from the sender server end).

<span style="font-weight: bold;">****Example****</span>:

****MVS****

FNAME = &FNAME (-&NFVER).

The partner sends the GDG file with the root and the version number
indicated.

<span style="font-weight: bold;">****Case 2****</span>: NFVER is used alone
(closed mode with implicit sending from the sender server end)

<span style="font-weight: bold;">****Example****</span>:

****MVS****

FNAME = &TEST.GDG (-&NFVER).

The partner sends version of the ‘&PART.&IDF’ file indicated
in the NFVER parameter.

<span style="font-weight: bold;">****Note****</span>: if NFVER is not defined,
the default value is 0.

It is consequently recommended to define one send command (CFTSEND)
for each type of file to be processed (normal or with versions).

[Return to Command index](../../)
