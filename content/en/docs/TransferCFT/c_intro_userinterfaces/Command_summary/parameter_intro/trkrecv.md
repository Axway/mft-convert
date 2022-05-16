---

    title: trkrecv
    linkTitle: trkrecv
    weight: 3610

---
<span id="trkrecv"></span>

### trkrecv

#### CFTPARM

**\[TRKRECV = {** **<span style="text-decoration: underline;">UNDEFINED</span>
|** **ALL | NO | SUMMARY }\]**

Specifications concerning transfers via CFTRECV for which the [trk](../trk)
value is not defined.

Select one of the following options:

- <span style="font-weight: bold;">****NO****</span>
    (default value): the monitor never sends Tracked Instances to Sentinel.
- <span style="font-weight: bold;">****ALL****</span>:
    for each step of each transfer process, the monitor sends a Tracked Instance
    to Sentinel.
- <span style="font-weight: bold;">****SUMMARY****</span>:
    for both the initial step and the final step of each transfer process,
    the monitor sends a Tracked Instance to Sentinel.

 

[Return to Command index](../../)
