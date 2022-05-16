---

    title: retryn
    linkTitle: retryn
    weight: 2890

---
<span id="retryn"></span>

### retryn

#### CFTTCP

****\[RETRYN = {<span style="text-decoration: underline;">4</span>
| n } \]****

Use this field to specify the number of reconnection attempts to make
with a time interval of <span style="font-weight: bold;">****retryw****</span>
between attempts.

<span style="font-weight: bold;">****4****</span>: default value

- any
    other value from <span style="font-weight: bold;">****0****</span> to <span style="font-weight: bold;">****32767****</span>

When <span style="font-weight: bold;">****retryn**** </span>attempts have been
made without success, {{< TransferCFT/axwayvariablesComponentShortName  >}} divides <span style="font-weight: bold;">****retryn****</span>
by two and multiplies <span style="font-weight: bold;">****retryw****</span> by
two and then begins the sequence again up to the total number of times
specified <span style="font-weight: bold;">****retrym****</span>.

**Example intervals**

If retryw = 1, retryn = 4, retrym =12

In this example the following occurs:

- Four
    retries at one-minute intervals
- Two
    retries at two-minute intervals
- One retry at four-minute intervals
- Five
    retries at eight-minute intervals

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
