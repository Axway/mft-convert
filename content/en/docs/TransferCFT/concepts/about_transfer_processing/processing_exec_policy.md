---

    title: Processing execution policy 
    linkTitle: Processing execution policy 
    weight: 190

---
This topic describes the processing execution policy as follows:

- [Broadcast request: processing execution policy](#Broadcas)
- [Group of files request: processing execution policy](#Group)
- [Both broadcast and group of files request: processing execution policy](#Broadcas2)

<span id="Broadcas"></span>

## Broadcast request

The EXECPRE, EXEC and EXECA parameters of the CFTDEST object define the execution policy for pre-processing, post-processing and acknowledgement processing procedures, respectively, for a broadcast request.

- EXECPRE: execution policy for a PREEXEC script
- EXEC: execution policy for EXEC and EXECE scripts
- EXECA: execution policy for ACKEXEC script to execute at acknowledgment phase

These parameters can have the values:

- PART: the procedure is executed on all requests, the generic request and its children
- DEST: the procedure is executed on the generic request
- CHILDREN: the procedure is executed on children

<span id="Group"></span>

## Group of files request

The EXECSUBPRE, EXECSUB and EXECSUBA parameters of the CFTSEND/CFTRECV object define, respectively, the execution policy for pre-processing, post-processing and acknowledgement processing procedures for a group of files request.

- EXECSUBPRE: execution policy for PREEXEC script
- EXECSUB: execution policy for EXEC and EXECE scripts
- EXECSUBA: execution policy for ACKEXEC script to execute at acknowledgment phase

These parameters can have the values:

- FILE: the procedure is executed on all requests, the generic request and its children
- LIST: the procedure is executed on the generic request
- SUBF: the procedure is executed on children

<span id="Broadcas2"></span>

## Broadcast and group of files request

When mixing both features, a broadcast and group of files, you can specifically define on which request you want to execute the processing procedures.

The result of mixing a broadcast with a group of files creates 3 levels of requests:

- L1: generic request of broadcast
- L2: child of generic request of broadcast (L2) and generic request of group of files
- L3: child of generic request of group of files (L3)

The processing level that is executed when you mix EXEXPRE/EXEC/EXECA and EXESUBPRE/EXECSUB/EXECSUBA parameters is shown in the following table.

QQQ\_QQQ\_CHECK check row vs col ...


| <span >CFTDEST (1) vs<br /> CFTSEND/SEND/CFTRECV/RECV (2)</span>  | PART  | DEST  | CHILDREN  |
| --- | --- | --- | --- |
| FILE  | L1 + L2 + L3  | L1  | L2 + L3  |
| LIST  | L1 + L2  | L1  | L2  |
| SUBF  | L1 + L3  | L1  | L3  |


\(2\) Columns: EXECPRE/EXEC/EXECA for a CFTDEST broadcast request

\(2\) Rows: EXECSUBPRE/EXECSUB/EXECSUBA
for CFTSEND/SEND/CFTRECV/RECV
Group of files request

## Improve the file layer service reliability

Setting the uconf c<span class="code">`ft.server.transfer.raise_error_when_exec_not_found `</span>parameter to <span class="code">`Yes `</span>(default) raises an error if the defined procedure script is not found, which improves the file layer service reliability.

This parameter is applicable to both post-processing (EXEC and EXECE) and ack-processing (ACKEXEC) procedures.
