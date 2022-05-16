---

    title: waitresp
    linkTitle: waitresp
    weight: 3740

---
<span id="waitresp"></span>

### waitresp

#### CFTPARM

****\[WAITRESP = {1...32767} \]****

The timeout for internal communication between Transfer CFT
tasks, which can be any
value from <span style="font-weight: bold;">****1****</span> to <span style="font-weight: bold;">****32767****</span> seconds.

Default values per operating system:

- 60: Windows
- 100: UNIX
- 600: z/OS (MVS)
- 1000: OS/400 (IBM i)

This parameter is used for a synchronous exchange of requests between
two monitor tasks during the initialization phase. After <span style="font-weight: bold;">****waitresp****</span>
seconds without reply, the timeout is interrupted. A CFTS09 message is
written in the log and the {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization stops.

 

[Return to Command index](../../)
