{
    "title": "narchivename",
    "linkTitle": "narchivename",
    "weight": "2110"
}### narchivename

### CFTRECV, RECV

**Available only when using the SFTP protocol.**

****[ NARCHIVEFNAME = string { max length 512} ]****

The archived source file name after a transfer completes if [NACTION](../naction)=ARCHIVE. It may contain [symbolic variables](../../symbolic_variables) such as &idtu, &froot, and so on.

If NACTION=ARCHIVE and NARCHIVEFNAME is undefined, the following error occurs: `DIAGI=159, DIAGP=NARCHIVEFNAME NOT DEFINED`

[Return to Command index](../../)
