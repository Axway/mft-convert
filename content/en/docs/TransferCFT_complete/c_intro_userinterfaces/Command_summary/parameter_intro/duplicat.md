{
    "title": "duplicat",
    "linkTitle": "duplicat",
    "weight": "770"
}### duplicat

****CFTSEND, CFTRECV****

****[DUPLICAT = { string 512 } ]****

This customizable parameter is used in detecting duplicate transfers. If you are performing a file transfer in and the file already exists, DUPLICAT recognizes the repeated pattern for a transfer. If this happens, the new transfer is rejected with a K status and DIAG = 432, DIAGP = DUPLICAT.

In *requester* mode, if the transfer is sent using synchronous API, the [DACTION](../daction) parameter is taken into account.

This field may contain a list of symbolic variables separated by a period ".". Possible variables include:

- PART
- IDF
- DIRECT
- MODE
- SAPPL / RAPPL
- IDA
- SUSER / RUSER
- FNAME / NFNAME
- SYSDATE / SYSTIME
- PARM

You cannot use IDT or IDTU as symbolic variables for DUPLICAT. Duplicate verifications are performed before writing to the catalog (when DACTION=ERROR), but the IDT and IDTU are inserted when the transfer is written to the catalog (after the verification), therefore these variables cannot be used as selection criteria in DUPLICAT.

****Example****

`DUPLICAT= &PART.&IDF.&IDA.&SAPPL`

[Return to Command index](../../)
