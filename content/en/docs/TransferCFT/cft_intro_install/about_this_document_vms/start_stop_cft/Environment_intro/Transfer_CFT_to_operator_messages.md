{
    "title": "Transfer CFT to operator messages ",
    "linkTitle": "Transfer CFT to operator messages",
    "weight": "270"
}The CFTLOG command NOTIFY parameter is used to designate the operator where LOG messages are sent. These messages are processed differently, according to the syntax used for the NOTIFY parameter. Two possibilities are available, user name or hexadecimal value.

- The NOTIFY parameter value is a user name. The messages intended for the operator are sent to all of the terminals on which the user is logged. Each message is displayed on the last line of the terminal. A new message overwrites the previous one. To stop receiving these messages on a particular terminal, enter the command: `$ set terminal/nobroadcast`

To restart this message delivery, use the command:` $ set terminal/broadcast`

- The NOTIFY parameter value has the OPnnnnnn format, where nnnnnn represents a string of six hexadecimal digits. The messages are sent to an operator terminal. This string determines the type of terminal to receive the messages. It corresponds to the OPC$B_MS_TARGET field of the SYS$SNDOPR system service request block. For more information, refer to the **OpenVMS System Services Reference Manual**.

To receive messages, enter the REPLY/ENABLE=XXX command, where XXX represents the selected operator class.

**Example**

```
$ reply/enable=oper1
```

**Hexadecimal values for the operator classes**


| Symbolic Name  | Hexadecimal Mask  | Decimal Value  |
| --- | --- | --- |
| OPC$M_NM_CENTRL | 1 | 1 |
| OPC$M_NM_PRINT | 2 | 2 |
| OPC$M_NM_TAPES | 4 | 4 |
| OPC$M_NM_DISKS | 8 | 8 |
| OPC$M_NM_DEVICE | 10 | 16 |
| OPC$M_NM_CARDS | 20 | 32 |
| OPC$M_NM_NTWORK | 40 | 64 |
| OPC$M_NM_CLUSTER | 80 | 128 |
| OPC$M_NM_SECURITY | 100 | 256 |
| OPC$M_NM_OPER1 | 1 000 | 4 096 |
| OPC$M_NM_OPER2 | 2 000 | 8 192 |
| OPC$M_NM_OPER3 | 4 000 | 16 384 |
| OPC$M_NM_OPER4 | 8 000 | 32 768 |
| OPC$M_NM_OPER5 | 10 000 | 65 536 |
| OPC$M_NM_OPER6 | 20 000 | 131 072 |
| OPC$M_NM_OPER7 | 40 000 | 262 144 |
| OPC$M_NM_OPER8 | 80 000 | 524 288 |
| OPC$M_NM_OPER9 | 100 000 | 1 048 576 |
| OPC$M_NM_OPER10 | 200 000 | 2 097 152 |
| OPC$M_NM_OPER11 | 400 000 | 4 194 304 |
| OPC$M_NM_OPER12 | 800 000 | 8 388 608 |

