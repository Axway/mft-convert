{
    "title": "origin",
    "linkTitle": "origin",
    "weight": "2510"
}### origin

#### All CFTXXX commands

****ORIGIN = &lt;C&gt; ('CFTUTIL','C','DESIGNER','D','COPILOT','O')****

This parameter indicates the origin of an object. You can use this to determine if an object was created or modified using {{< TransferCFT/PrimaryCGorUM  >}} (DESIGNER), CFTUTIL, or Copilot.

To view the ORIGIN parameter, for example:

```
CFTUTIL cftext id=test, content=brief
CFTSEND ID = 'TEST',
IMPL = 'NO',
STATE = 'DISP',
CYCLE = '0',
...
ORIGIN = 'CFTUTIL',
MAXDURATION = '0',
...
MODE = 'REPLACE'
```
