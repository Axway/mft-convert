{
    "title": "trkpart",
    "linkTitle": "trkpart",
    "weight": "3620"
}<span id="trkpart"></span>

### trkpart

#### CFTPARM

**[TRK = { UNDEFINED
&#124; ALL &#124; NO &#124; SUMMARY }]**

The default value that specifies how much detail {{< TransferCFT/axwayvariablesComponentShortName  >}} should
provide Sentinel concerning transfers with a partner who does
not have a defined [trk](../trk) value.

Select one of the following options:

- ****NO****: the monitor never sends Tracked
    Instances to Sentinel. Default value.
- ****ALL****: for each step of each transfer
    process, the monitor sends a Tracked Instance to Sentinel.
- ****SUMMARY****: for both the initial step and
    the final step of each transfer process, the monitor sends a Tracked Instance
    to Sentinel.

 

[Return to Command index](../../)
