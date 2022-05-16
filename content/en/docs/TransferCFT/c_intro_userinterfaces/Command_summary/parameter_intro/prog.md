---

    title: prog
    linkTitle: prog
    weight: 2720

---
<span id="prog"></span>

### prog

#### CFTEXIT

**\[<span id="PROG1"></span>PROG = {<u>CFTEXIT</u>
| *string* <span style="font-weight: bold;">****(1 …512)****</span>}\]**

Enter the name of the executable module that corresponds to the EXIT
task to activate. This module comprises the interface provided with the
transfer CFT product and linked with the user program.

To facilitate identification of the associated modules, the following
module names are recommended:

- <span style="font-weight: bold;">****CFTEXA****</span>
    for a directory type EXIT
- <span style="font-weight: bold;">****CFTEXE****</span>
    for an end-of-transfer type EXIT
- <span style="font-weight: bold;">****CFTEXF****</span>
    for a file type EXIT

If you define more than one EXIT, you can add two characters to the
name to assign a sequential number (for example: CFTEXA01).

 

[Return to Command index](../../)
