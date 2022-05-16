---

    title: timep
    linkTitle: timep
    weight: 3510

---
<span id="timep"></span>

### timep

#### CFTCAT

**TIMEP = {<u>23595999</u> | HHMMSSCC}**

User-defined daily purge time.

The user can program an automatic, cyclic catalog purge.  
The default purge time is midnight.

> **Note**
>
> You can completely deactivate purging by definingTIMEP = 00000000. Use this option with caution as no automatic purging
> is performed (at a selected time or at midnight).

#### PURGE

**\[TIMEP = {<u>23595999</u> | HHMMSSCC}**

User-defined purge time.

> **Note**
>
> You can deactivate the next purge function by setting TIMEP = 00000000. This operation should be used with care,
> because of the risks of catalog overloading -with a loss of performance,
> and risk of overflow. If the next purge is part of a cycle, see the CFTCAT TIMEP parameter,
> the whole cycle is deleted, and not just the next occurrence of this cycle.

 

[Return to Command index](../../)
