{
    "title": "Transfer scripts and temporary files",
    "linkTitle": "Transfer scripts and temporary files",
    "weight": "270"
}This topic describes how to use scripts  in Transfer CFT Windows.

- [Updating
    batch procedures launched by Transfer CFT](#Updating%20batch%20procedures%20launched%20by%20Transfer%20CFT)
- [Using
    symbolic variables in batch files started by Transfer CFT](#Using_symbolic_variables_in_the_batch_files_started_by_CFT)

<span id="About_automated_CFT_functions"></span>

> **Note**
>
> Note: You should not use an operating system variable
> directly in a script started by Transfer CFT. Doing so could result in Transfer CFT generating a temporary
> file that provokes an illegal operation.

Scripts started by Transfer CFT
-------------------------------

Some Transfer CFT events, such as transfers, CRONJOBs, or log switching, automatically
start scripts that are defined in the exec parameters. When these scripts are started, Transfer CFT generates a temporary
file name in which it copies the content of the initial script, after resolving any used symbolic variables.
Once executed, by these temporary
files are deleted by default.

### How to save temporary files after executing a script

****CFTNODEL environment variable<span id="CFTNODEL"></span>****

The temporary file is usually
deleted after the script is submitted and executed. However, you can use the CFTNODEL environment variable to stop Transfer
CFT from deleting this temporary file. By default the temporary file is located in the `<install_dir>/runtime/run` folder. To keep a temporary file after the script's execution, set CFTNODEL to any value (YES in the example) and restart {{< TransferCFT/axwayvariablesComponentLongName  >}} session.

```
C:<install_dir>\\runtime>set CFTNODEL=YES
```
<span id="Updating batch procedures launched by Transfer CFT"></span>

Updating scripts launched by Transfer CFT
-----------------------------------------

When automatic scripts are started, Transfer CFT performs
the following operations:

- Reads the script
- Specifies and creates
    a unique name for the temporary file

The unique name for the temporary file is specified by a
prefix in the form of CFTxxxxxxxx_nnnnn, where xxxxx represents the CFTMAIN PID and nnnnn is a number between 0 and
99999. Each time a prefix is generated, the number nnnnn is incremented
by 1.

The following processes occur:

- Content of the
    script in which the symbolic variables have been resolved is written
    into this temporary file
- Temporary file
    executes
- Temporary file
    is deleted

Troubleshooting
---------------

If you have trouble using a script:

1. Manually start the script and watch the results.
1. To observe the effect
    of the symbolic variables substitutions by Transfer CFT; you can use the
    CFTNODEL environment variable.
