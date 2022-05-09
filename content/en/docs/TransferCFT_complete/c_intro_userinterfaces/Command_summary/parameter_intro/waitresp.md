{
    "title": "waitresp",
    "linkTitle": "waitresp",
    "weight": "3760"
}<span id="waitresp"></span>

### waitresp

#### CFTPARM

****[WAITRESP = {1...32767} ]****

The timeout for internal communication between Transfer CFT
tasks, which can be any
value from ****1**** to ****32767**** seconds.

Default values per operating system:

- 60: Windows, OpenVMS
- 100: UNIX
- 600: z/OS (MVS)
- 1000: OS/400 (IBM i)

This parameter is used for a synchronous exchange of requests between
two monitor tasks during the initialization phase. After ****waitresp****
seconds without reply, the timeout is interrupted. A CFTS09 message is
written in the log and the {{< TransferCFT/axwayvariablesComponentShortName  >}} initialization stops.

 

[Return to Command index](../../)
