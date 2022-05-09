{
    "title": "prog",
    "linkTitle": "prog",
    "weight": "2740"
}<span id="prog"></span>

### prog

#### CFTEXIT

**[<span id="PROG1"></span>PROG = {<span class="underline">CFTEXIT</span>
&#124; *string* ****(1 …512)****}]**

Enter the name of the executable module that corresponds to the EXIT
task to activate. This module comprises the interface provided with the
transfer CFT product and linked with the user program.

To facilitate identification of the associated modules, the following
module names are recommended:

- ****CFTEXA****
    for a directory type EXIT
- ****CFTEXE****
    for an end-of-transfer type EXIT
- ****CFTEXF****
    for a file type EXIT

If you define more than one EXIT, you can add two characters to the
name to assign a sequential number (for example: CFTEXA01).

 

[Return to Command index](../../)
