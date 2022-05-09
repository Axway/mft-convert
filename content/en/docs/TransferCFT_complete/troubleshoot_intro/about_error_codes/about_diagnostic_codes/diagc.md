{
    "title": "DIAGC - Complementary diagnostic codes",
    "linkTitle": "DIAGC- Complementary diagnostic codes",
    "weight": "320"
}DIAGC are a readable source of additional information concerning a transfer intended to supplement DIAGI and DIAGP, which can be useful in troubleshooting instances. The DIAGC may display as part of a log message or, depending on the issue, it may also display in the catalog, though it does not systematically display for all transfer errors.

****Example****

In this example, there was an SFTP transfer with an authentication problem (incorrect password). Checking the DIAGC provides connection details.

In the catalog (content=brief) :

```
Partner DTSAPP File Transfer Records Diags Appli. Appstate.
Id. Id. Transmit Total CFT Protocol Id.
-------- ------ -------- -------- ---------- ---------- --- -------- -------- ---------
LOCAL SFH TH BIN A1918244 0 0 933 00000001
In the log
00 22/01/19 18:24:43 CFTT95E Incorrect user or password <IDTU=A0000001 PART=LOCAL IDF=BIN IDT=A1918244 DIAGI=933>
00 22/01/19 18:24:43 CFTT82E Transfer aborted <IDTU=A0000001 PART=LOCAL IDF=BIN IDT=A1918244 DIAGI=933
00 22/01/19 18:24:43 CFTT82E+ DIAGP=00000001 DIAGC=The user is not allowed to connect to the server for reason: Access denied for 'password'. Authentication
00 22/01/19 18:24:43 CFTT82E+that can continue: publickey,password>
```

In the catalog (content=debug):

```
Local diagnosis DIAGI = 933
Protocol diagnosis DIAGP = 00000001
Complementary diagnosis DIAGC =
\*The user is not allowed to connect to the server for reason: Access denied for '
\*password'. Authentication that can continue: publickey,password
```
