{
    "title": "PI to parameter mapping",
    "linkTitle": "PeSIT parameter summary",
    "weight": "160"
}The following table shows the correspondence between
standardized PI codes and Transfer CFT defined values.


| PI  | Description  |  Transfer CFT value  |
| --- | --- | --- |
| 1  | Use of a CRC (deprecated) | PAD  |
| 2  | Diagnostic  | (T)  |
| 3  | Requester identifier | NSPART(length &lt; 24)  |
|   | Sending application’s name  | SAPPL  |
|   | Sending user’s name  | SUSER  |
| 4  | Server identifier  | NRPART<br /> (length &lt; 24)  |
|   | Receiver application name  | RAPPL  |
|   | Receiver user name  | RUSER  |
| 5  | Access control  | NSPASSW<br /> NRPASSW  |
|   | New password  | ( / )  |
| 6  | Version number  | 2  |
| 7  | Interval between synchronization points  | SPACING<br /> RPACING  |
|   | Acknowledgement window  | SCHKW<br /> RCHKW  |
| 11  | File type  | (T)  |
| 12  | File name  | NIDF (length &lt; 76)<br /> (possibility of generic selection)  |
|   | Message name  | IDM  |
| 13  | Transfer identifier  | (T)  |
| 14  | Requested attributes  | 0xE0 (all)  |
| 15  | Restarted transfer  | 0 (no), 1 (yes)  |
| 16  | Data code  | NCODE  |
| 17  | Transfer priority  | PRI  |
| 18  | Restart point  | (T)  |
| 19  | End-of-transfer code  | (T)  |
| 20  | Synchron. point no.  | (T)  |
| 21  | Compression  | NCOMP<br /> RCOMP SCOMP  |
| 22  | Access type  | SROUT (read, write, mixed)  |
| 23  | Resynchronization  | RESYNC  |
| 25  | Data entity maximum size  | SRUSIZE<br /> RRUSIZE  |
| 27  | Number of data bytes  | (T)  |
| 28  | Number of articles  | (T)  |
| 29  | Diagnostic complement  | ( / )  |
| 31  | Article format  | NRECFM  |
| 32  | Article length  | NLRECL  |
| 33  | File organization  | NORG  |
| 37  | File label  | NFNAME (length &lt; 512)  |
| 38  | Key length  | NKEYLEN  |
| 39  | Key offset  | NKEYPOS  |
| 41  | Space reservation unit  | 0 (kilo-bytes)  |
| 42  | Space reservation<br /> maximum value  | NSPACE  |
| 51  | Creation date  | FDATE  |
|   | Creation time  | FTIME  |
| 52  | Date and time of last extraction  | PI 51  |
| 61  | Client identifier | NSPART of initial sender  |
| 62  | Bank identifier | NRPART of final recipient  |
| 91  | Message  | New parameter  |
| 99  | Free message  |   |


In this table:

- T: defined by Transfer
    CFT during transfer
- (/): not used by
    Transfer CFT
- (-): PI not supported
    by this version of the protocol
