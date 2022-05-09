{
    "title": "Use  batch mode",
    "linkTitle": "Using batch mode",
    "weight": "150"
}CFTUTIL is a command-line mode user interface, which serves to create
the Transfer CFT working environment and set up Transfer CFT application
parameters. You use it to:

- Create and delete parameter, partner, catalog, log and account files

<!-- -->

- Add or modify parameters
- Display parameter,
    partner, catalog, log and account files
- Send commands
    to Transfer CFT

In batch mode, CFTUTIL reads the Transfer CFT
commands from a text file. The commands are either placed in the default
communication medium, referred to Transfer CFT media, or in the
medium defined by the CONFIG TYPE = COM MEDIACOM = ... command.

<span id="Batch_execution"></span>

Batch execution
---------------

In batch mode with input flow rerouting, CFTUTIL reads the Transfer
CFT commands in the file being submitted or in any text file. Output is
made on the standard batch output. The operating mode is analogous in
all respects to interactive operation.

Batch examples are given in the
Transfer CFT *Operating Guide* that corresponds to your OS.

The available batch mode commands are:

- SEND
- RECV
- HALT
- KEEP
- START
- RESUME
- DELETE
- END
- SWITCH
- SHUT
- ACT
- CLEARCMD
- DELETE
- END
- HALT
- INACT
- KEEP
- KSTATE
- MQUERY
- PURGE
- RECV
- RESUME
- SEND  
- SHUT  
- START   
- SUBMIT  
- SWITCH
- TURN
- WLOG
