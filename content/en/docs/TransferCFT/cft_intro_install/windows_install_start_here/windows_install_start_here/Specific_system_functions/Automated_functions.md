---

    title: Automated functions
    linkTitle: Automated functions
    weight: 280

---
This topic describes how to use a batch procedure in Transfer CFT Windows.

- [Updating
    batch procedures launched by Transfer CFT](#Updating%20batch%20procedures%20launched%20by%20Transfer%20CFT)
- [Using
    symbolic variables in batch files started by Transfer CFT](#Using_symbolic_variables_in_the_batch_files_started_by_CFT)

<span id="About_automated_CFT_functions"></span>

### Batch procedures started by Transfer CFT

After a certain number of Transfer CFT events, such as transmissions,
receptions, SWITCH commands, for example, Transfer CFT can automatically
start batch procedures. These are defined in the parameters.

When these batch files are started, Transfer CFT generates a temporary
batch file name into which it copies the content of the initial batch
file, having resolved the symbolic variables which may have been invoked.
Once executed, by default, Transfer CFT deletes these temporary batch
files.

### Non-deletion of temporary batch files after execution

****Environment variable****

<span id="CFTNODEL"></span>CFTNODEL

The CFTNODEL environment variable is able to stop Transfer
CFT from definitively deleting the temporary file, it is usually
deleted after it has been submitted and executed.

<span id="Updating batch procedures launched by Transfer CFT"></span>

## Updating batch procedures launched by Transfer CFT

When automatic procedures are started, Transfer CFT performs
the following operations:

- Reads the batch
    file concerned
- Specifies and creates
    a unique name for the temporary file

The unique name for the temporary file is specified by specifying a
prefix in the form of CFTnnnnn, where nnnnn is a number between 0 and
99999. Each time a prefix is generated, the number nnnnn is incremented
by 1. The following processes occur:

- Content of the
    batch file in which the symbolic variables have been resolved is written
    into this temporary file
- Temporary file
    executes
- Temporary file
    is deleted

If you have trouble using the batch procedures:

1. Start the batch file to be
    implemented manually and watch the effect this produces.
1. To observe the effect
    of the substitutions by Transfer CFT of the symbolic variables, use the
    CFTNODEL environment variable.

<span id="Using symbolic variables in batch files started by Transfer CFT"></span><span id="Using_symbolic_variables_in_the_batch_files_started_by_CFT"></span>

## Using symbolic variables in batch files started by Transfer CFT

Do not set an operating system variable
directly into a batch file started by Transfer CFT.

If you do not take special precautions, there is a risk that a temporary
file will be generated to Transfer CFT, where it provokes an illegal operation
when submitted to the operating system. Such submission can be automatic
or manual, it is still an illegal operation.

You can still use environment variables, provided that they are set
in a different batch file to that started by Transfer CFT. The batch file
started by Transfer CFT calls the batch file containing the environment
variable settings.

****Example****

`/* BATCH started by CFT */call pos_envCFTUTIL SEND PART = &part, IDF = %IDF%...`

`/* BATCH pos_env called by the batch file started by CFT   */SET IDF=TEST...`
