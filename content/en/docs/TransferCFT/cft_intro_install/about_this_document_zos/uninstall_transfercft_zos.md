{
    "title": "Uninstall Transfer CFT ",
    "linkTitle": "Uninstall",
    "weight": "190"
}This section describes how to uninstall Transfer CFT in a z/OS environment.

If you uninstall a Transfer CFT, you will lose the entire Transfer CFT
configuration. To avoid this, you must save your environment prior removing the Transfer CFT.

### Procedure

Before uninstalling, you must stop the servers you want to uninstall (including Copilot).

Run the UNINSTAL JCL, which:

1. Removes all files from the INSTANCE, including multi-node, except for the JCL library (target.INSTALL).
1. Removes sub-USS directories, Copilot, and Secure Relay.
1. Removes the distribution environment.

> **Note**
>
> Note: You must manually modify the JCL, by default the distribution environment is not removed (see lines 000018, 000081, and 000082 in the example).

Example

```
000014 //\* 2 phases in this JCL:
000015 //\* --------------------
000016 //\* Phase 1 - Delete Transfer CFT instance env.
000017 //\* Phase 2 - Delete Transfer CFT distribution env.
000018 //
000019 //
000020 //\* Phase 1 - Delete Transfer CFT instance env.
000021 //\* ------------------------------------------
000022 //\* INSTALL library is not deleted.
000023 //IN01 EXEC PREXX,PARM='%RXDEL &CFTENV..USER.OBJ 0'
000024 //IN02 EXEC PREXX,PARM='%RXDEL &CFTENV..SAMPLE 0'
…[continued]…
000085 //
000086 //
000087 //\* --------------------------------------------------
000088 //\* Phase 2 - Delete Transfer CFT distribution env.
000089 //\* --------------------------------------------------
000090 //DI03 EXEC PREXX,PARM='%RXDEL &DISTLIB..CNTL 0'
000091 //DI04 EXEC PREXX,PARM='%RXDEL &DISTLIB..DOC 0'
000092 //DI05 EXEC PREXX,PARM='%RXDEL &DISTLIB..SAMPLE 0'
000093 //DI06 EXEC PREXX,PARM='%RXDEL &DISTLIB..LOG 0'
000094 //DI07 EXEC PREXX,PARM='%RXDEL &DISTLIB..MAC 0'
…[continued]…
```
