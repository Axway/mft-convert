{
    "title": "retryw",
    "linkTitle": "retryw",
    "weight": "2920"
}<span id="retryw"></span>

### retryw

#### CFTTCP

****[RETRYW = { <span class="underline">1</span> &#124; n }]****

The time interval (in minutes) between reconnection attempts.

- ****1****
    : default value
- any other value
    from ****0**** to ****32767****

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
