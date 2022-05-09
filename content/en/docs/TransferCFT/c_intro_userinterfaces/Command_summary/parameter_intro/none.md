{
    "title": "_NONE_",
    "linkTitle": "_NONE_",
    "weight": "2280"
}A keyword to use with the procedure parameters EXEC, EXECE, and ACKEXEC. You can use the _NONE_ keyword in send and receive commands to disable the EXECSF, EXECRF, EXECSE, EXECRE and EXECSFA procedures that are defined in the general Transfer CFT environment parameter settings (CFTPARM).

Where:

`EXEC = '_NONE_'`

- Disables the EXECSF procedure for a CFTSEND or SEND file.
- Disables the EXECSM procedure for a SEND message or reply.
- Disables the EXECRF procedure for a CFTRECV or RECV file.

`EXECE = '_NONE_'`

- Disables the EXECSE for a CFTSEND or SEND file.
- Disables the EXECSE for a SEND message or reply.
- Disables the EXECRE for a CFTRECV or RECV file.

`AKCEXEC = '_NONE_'`

- Disables the EXECSFA procedure for a CFTSEND or SEND file.

See also, the SEND and RECV commands.
