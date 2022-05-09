{
    "title": "sslwresp",
    "linkTitle": "sslwresp",
    "weight": "3380"
}<span id="sslwresp"></span>

### sslwresp

#### CFTPARM

****[SSLWRESP =   {1...32767
} ]****

The timeout granted to an SSL task to start and return
its initialization status, which can be any
value from ****1**** to ****32767**** seconds.

Default values per operating system:

- 60: Windows, OpenVMS
- 100: UNIX
- 600: z/OS (MVS)
- 1000: OS/400 (IBM i)

If no status is returned within this time limit, {{< TransferCFT/axwayvariablesComponentShortName  >}} considers
that the task could not be initialized and defers the transfer in requester
mode, or closes the network connection in server mode.

 

[Return to Command index](../../)
