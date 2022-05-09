{
    "title": "retrym",
    "linkTitle": "retrym",
    "weight": "2900"
}<span id="retrym"></span>

### retrym

#### CFTTCP

****[RETRYM = {12
&#124; 0...32767 } ]****

Use this field to specify the maximum number of reconnection attempts.

- 12 (default value)
- any
    other value from 0 to 32767

If you enter 0 and if the initial connection fails, no further reconnection
attempts are made.

**Retry concepts**

When specifying multiple hosts in CFTTCP and PROTs/SAPs in CFTPART, Transfer CFT first retries the host and then the PROT/SAP.

For example, if RETRYN is set to 1 and there are two defined hosts and two PROTs/SAPs as shown here:

```
CFTTCP ...
HOST = ( 'IP1',
'IP2'),
CFTPART ... PROT = ( 'PROT1',
'PROT2),
           SAP = ( 'SAP1',
'SAP2'),
```

The retry order is as follows:

- IP1 SAP1 PROT1 (first host, first protocol)
- IP1 SAP1 PROT1 (retry)
- IP2 SAP1 PROT1 (second host, first protocol)
- IP2 SAP1 PROT1 (retry)
- IP1 SAP2 PROT2 (first host, second protocol)
- IP1 SAP2 PROT2 (retry)
- IP2 SAP2 PROT2 (second host, second protocol)
- IP2 SAP2 PROT2 (retry)

 

[Return to Command index](../../)
