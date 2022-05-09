{
    "title": "Troubleshoot the catalog",
    "linkTitle": "Troubleshoot the catalog",
    "weight": "410"
}The catalog is full and {{< TransferCFT/axwayvariablesComponentLongName  >}} has stopped
---------------------------------------------------------------------------------------------

****Symptom****

Transfer CFT stops and you cannot restart it. To remedy this issue, you can execute the `cftcatal `tool.

****Remedy****

You can use the `cftcatal `utility to increase the size of the Transfer CFT catalog file without losing information. In a multi-node environment, this action resizes all nodes.

****Example on Windows or Unix****

Execute the `profile `from the runtime directory and then enter the `cftcatal` command. Modify the number of records when prompted, for example:

```
cftcatal
CFT Catalog file extension
Current size of catalog file : 1000
Number of records in catalog : 1000
Number of records for catalog file (0=cancel,minimum=10) : 2000
Extracting current catalog records ......
New catalog extended to 2000 records and cft can be restarted.
```
