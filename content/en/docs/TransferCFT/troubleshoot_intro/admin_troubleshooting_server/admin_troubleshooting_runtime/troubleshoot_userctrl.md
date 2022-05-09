{
    "title": "Troubleshoot user control (USERCTRL)",
    "linkTitle": "Troubleshoot user control (USERCTRL)",
    "weight": "420"
}****Homogeneous transfers fail on UNIX systems****

```
CFTF03E local file deselect error 00000008
CFTF30W SFM_COPY : invalid file name OR other error IDTU= IDF=
CFTF30W+IDT=
CFTF30W+U=expl F=<temporary filename> <IDTU=
```

If a USERID is set for a CFTRECV during a homogeneous transfer, the transfer fails if the user does not have rights on the log folder.

****Fix****

Grant the user the right to execute $CFTDIRLOG to access the directory.
