---

    title: Sending a negative acknowledgement 
    linkTitle: Sending a NACK
    weight: 230

---
This topic describes the SEND NACK command, which sends a negative acknowledgement.

Via negative acknowledgments, the
final partner signals to the initial sender of the file that application
errors were detected.

If the initial sender does not support this function, as for example in earlier versions of Transfer CFT, the final partner does not transmit the
negative acknowledgement and the Transfer CFT log file displays:

```
CFTT93W PART=XFB1 IDS=00008 Negative ack not supported by server
```

******Example******

```
cftutil send
type=nack,
part=&part,
idm=nack,
msg=recu,
idt=&idt
```

******Syntax******

SEND TYPE = NACK

`IDM   = identifier `

`IDT   = transid  `

`MSG   = string   `

`PART   = identifier `

`[ APPCYCID   = string ]`

`[ APPOBJID   = string ]`

`[ CYCDATE   = date ]`

`[ CYCTIME   = time ]`

`[ EXEC = filename ]`

`[ IDA = identifier ]`

`[ IPART   = string ]`

`[ MAXDATE   =  { 99991231   | date } ]`

`[ MAXTIME   = { 23595999   | time } ]`

`[ MINDATE   = { current   system date | date } ]`

`[ MINTIME   = { 0   | time } ]`

`[ PRI   = pri ]`

`[ PROT = identifier ]`

`[ RAPPL   = string ]`

`[ RUSER   = string ]`

`[ SAPPL   = string ]`

`[ SUSER   = string ]`

`[ TCYCLE   = { DAY   | MIN | MONTH } ]`

`[ TRK   = { UNDEFINED   | ALL | SUMMARY | NO } ]`
