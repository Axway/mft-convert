{
    "title": "Transfer scripts and  temporary files",
    "linkTitle": "Transfer scripts and temporary files",
    "weight": "310"
}This topic describes temporary files,
which Transfer CFT creates various temporary
files in the */tmp* directory depending on the script run.

Temporary files
---------------

There are four types of temporary files corresponding to different types
of actions:

- */tmp/cftlo\**
    files are produced during the log switching procedure
- */tmp/cftcn\**
    files are produced when the accounting feature is enabled
- */tmp/cftsu\**
    files are produced when end of transfer procedures are run
- */tmp/cftsu\*.err*
    files correspond to the results of commands in the *cftsu\** files

### Deleting temporary files

Transfer CFT cannot delete temporary files automatically as it does not
know exactly when the end of the user script is reached. Therefore, to avoid saturating the `/tmp` directory, you should end any shell
procedure with the `rm $0` command.

> **Note**
>
> Note: If you omit this command, the /tmp partition rapidly becomes
> full and the end of transfer procedures fails.

This command deletes the procedure that runs it and is applicable to
*cftlo\*, cftfcn*\* and *cftsu*\* files.

Additionally, you should delete the *cftsu\*.err* files associated with *cftsu\**
files. However, to avoid losing any errors that may
be logged in this file, it is best to check that the file is empty before
deleting it.

For example, enter:

```
if test -s $0.err
then
echo $0.err contains data to be consulted
else
rm $0.err
fi
```

### TMPDIR environment variable

You can use the environment variable `TMPDIR=<path> `to define a temporary directory in Unix and POSIX. If you define TMPDIR, the temporary file goes in this directory. If you do not define TMPDIR, {{< TransferCFT/axwayvariablesComponentLongName  >}} uses the default directory (/tmp).

****Example****

```
export TMPDIR=/home/Desktop/tmpdir/
```

> **Note**
>
> Note: You must restart Transfer CFT if you change the TMPDIR value.

Sample script to execute a procedure
------------------------------------

The contents of the `recvm.cmd` file stored in `<installdir>/runtime/conf/` are
shown below. The `recvm.cmd` file is a sample script to execute
after receiving a message.

This procedure must first be declared in the CFTPARM section of your
configuration file in the EXECRM field so that it can be executed.

****Example****

```
EXECRM = '/home/transfer/cft/<installdir>/runtime/conf//recvm.cmd'
The contents of the recv.cmd file are as follows:
echo "MESSAGE RECEIVED" /\* display of the message received \*/
echo "\*\* &msg \*\*" /\* by CFT using the &msg symbolic \*/
/\* variable that contains the \*/
/\* message text \*/
rm $0 /\* deleting the /tmp/cftsu\* \*/
/\* temporary file \*/
if test -s $0.err
then
echo $0.err contains data to be consulted
else
rm $0.err
fi
```
