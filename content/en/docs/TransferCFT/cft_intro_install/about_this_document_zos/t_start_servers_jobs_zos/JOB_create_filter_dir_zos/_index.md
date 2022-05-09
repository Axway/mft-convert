{
    "title": "Copilot options for log and catalog filters",
    "linkTitle": "Set up optional features",
    "weight": "220"
}You can optionally set up log and catalog filters in Copilot using the JCL described in this section.

The JCL **COPFILTR** lets you create a directory under the USS to store the Copilot catalog and log filters. This server directory has read, write, and delete rights for Transfer CFT Copilot users, but is independent of the Copilot component directory.

Creating a filter directory
---------------------------

You use the following JCL and statements to create a filter directory.

### Defining the directory

You will need to enter the directory name twice for the following:

- STP001 - To create the directory
- STP002 - To initialize the variable copilot.general.persistencedir

**Example**

You can use the following example as a reference for creating your directory.  In this example, we used **` /home/COPILOT/runtime/persist`  as the new directory.**

```
//\*
// SET P='/home/COPILOT/runtime/persist'
//\*
//STP001 EXEC PGM=BPXBATCH,
// PARM='SH mkdir -p -m 774 &P'
//STDIN DD DUMMY
//STDOUT DD SYSOUT=\*
//STDERR DD SYSOUT=\*
//\*
//\* Set variable: copilot.general.persistencedir
//\*
//STP002 EXEC PCFTUTIL,PARM='/1=&P'
//CFTIN DD \*
UCONFSET ID=copilot.general.persistencedir,
VALUE='%_ARGV1%'
/\*
```
