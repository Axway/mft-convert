{
    "title": "bufsize",
    "linkTitle": "bufsize",
    "weight": "330"
}<span id="bufsize"></span>

### bufsize

#### CFTPARM

**[BUFSIZE = n]    **{512..see
below}

The size of the {{< TransferCFT/axwayvariablesComponentShortName  >}} internal buffer used to exchange data between Transfer CFT
tasks, expressed in characters (bytes).

Using a high BUFSIZE value improves
performance but uses more memory.

Default values:

- UNIX, Windows: 4096
- z/OS: 36608
- IBM i: 32740
- OpenVMS: 8100

Range of values:

- UNIX, Windows, z/OS, IBM i: 512
    to 65535
- OpenVMS: 512 to 8180

If the BUFSIZE is lower than the RRUSIZE or SRUSIZE parameter values, Transfer CFT uses the RRUSIZE or SRUSIZE value rendering BUFSIZE redundant.

[Return to Command index](../../)
