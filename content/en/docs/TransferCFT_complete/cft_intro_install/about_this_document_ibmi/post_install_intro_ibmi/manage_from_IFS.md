{
    "title": "Manage Transfer CFT using IFS",
    "linkTitle": "Manage Transfer CFT using IFS",
    "weight": "200"
}This section explains how you can use Transfer CFT on the IFS environment. To do this you must run the following commands:

1. Log on the iSeries using the Transfer CFT account.
1. Execute the `QSH `command.
1. Change the directory:
    ```
    cd /home/cft/TransfertCFT/runtime
    ```
1. Load the profile:
    ```
    . ./profile
    ```
1. You can then use standard Transfer CFT programs, such as:

- CFTSTART: Start Transfer CFT
- CFTSTOP: Stop Transfer CFT
- COPSTART: Start UI Server
- COPSTOP: Stop UI Server
- CFTUTIL
- PKIUTIL
- Etc.
