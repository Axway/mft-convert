---

    title: trkpart
    linkTitle: trkpart
    weight: 3600

---
<span id="trkpart"></span>

### trkpart

#### CFTPARM

**\[TRK = { <span style="text-decoration: underline;">UNDEFINED</span>
| ALL | NO | SUMMARY }\]**

The default value that specifies how much detail {{< TransferCFT/axwayvariablesComponentShortName  >}} should
provide Sentinel concerning transfers with a partner who does
not have a defined [trk](../trk) value.

Select one of the following options:

- <span style="font-weight: bold;">****NO****</span>: the monitor never sends Tracked
    Instances to Sentinel. Default value.
- <span style="font-weight: bold;">****ALL****</span>: for each step of each transfer
    process, the monitor sends a Tracked Instance to Sentinel.
- <span style="font-weight: bold;">****SUMMARY****</span>: for both the initial step and
    the final step of each transfer process, the monitor sends a Tracked Instance
    to Sentinel.

 

[Return to Command index](../../)
