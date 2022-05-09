{
    "title": "Running  CFTUTIL",
    "linkTitle": "Running CFTUTIL",
    "weight": "220"
}CFTUTIL is a command line mode user interface used to create the Transfer CFT
working environment manually and configure the product. It allows you
to create or delete a parameter, partner, catalog, log and accounting files.

The following operations can only be carried out when Transfer CFT is
shut down.

- Modify or add certain
    parameters
- View the parameter,
    partner, catalog, log and accounting files
- Send commands to
    the monitor

### Start CFTUTIL

CFTUTIL is started up by entering `CFTUTIL `directly from the Transfer
CFT program directory or from any other directory if the PATH environment
variable includes the path pointing to the Transfer CFT programs.

CFTUTIL can also be activated in a shell procedure.

### Line mode input

CFTUTIL accepts *line mode* commands. The *CFT &gt;* prompt
enables you to enter any commands online and validate them by pressing
ENTER**.**

To exit CFTUTIL, enter the */end* command.

****Example****

```
     %
CFTUTIL
CFT/V2/UAIX
Release 3.10
(C) Copyright Axway 2000- 2022
CFT> send
part=headoffice,idf=txt,fname=/home/lisa/report.txt
CFTU94I SEND part =HEADOFFICE,idf=TX _ Correct
CFT> /end
%
```

### Run-time parameters

CFTUTIL can also accept commands passed either individually as parameters
or in a command file:

- Command passed
    as a parameter:  
    The command line is passed as a CFTUTIL parameter using the following
    syntax:

    `CFTUTIL command parameter=value, parameter=value,...`

****Example****

`CFTUTIL listcat part=headoffice, direct=send`

This command displays a table of scheduled (or performed)
transfers to the *headoffice* partner

- File passed as
    a parameter

The following command runs the CFTUTIL utility, which
reads the commands to be executed in the *scen.cft* file and displays
the results on the screen:

`CFTUTIL @scen.cft`
