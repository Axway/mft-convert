{
    "title": "naction",
    "linkTitle": "naction",
    "weight": "2080"
}### naction

#### CFTRECV, RECV

****[NACTION = { <span class="underline">NONE</span> &#124; DELETE &#124; ARCHIVE }****

**Available only when using the SFTP protocol.**

Action to perform on the remote source file after a receive transfer is completed:

- **NONE** (default): No action occurs on the file after the transfer.
- **DELETE**: Delete the remote source file after the transfer.
- **ARCHIVE**: Move the remote source file to the file name specified in the [NARCHIVEFNAME](../narchivename) parameter after the transfer.
    -   If the transfer fails, the file is not moved.
    -   If the target file already exists, it is overwritten if the remote server supports SFTP version 4.

If an error occurs while deleting or archiving, the receive transfer is still successful but a warning displays in the log.

[Return to Command index](../../)
