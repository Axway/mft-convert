{
    "title": "Use the check command",
    "linkTitle": "Use the check command",
    "weight": "170"
}The CFTUTIL `CHECK `command validates the coherence of parameters, partners, and the Transfer CFT PKI database.

The syntax is:

```
CHECK CONTENT=<span class="underline">BRIEF</span>&#124;FULL, FOUT=FileName
```

The `CHECK CONTENT=BRIEF` (default) command verifies that:

- All the referenced objects exist
- Each CFTPART has an associated CFTTCP
- There is at least one CFTPARM
- The used certificates have not expired

Any encountered errors are displayed in the console, and we highly recommend that you fix them before starting Transfer CFT.

The `CHECK CONTENT=FULL, FOUT=FileName` command also checks that:

- All objects are used
- No CRONTAB is empty
- Each CFTPROT is used by at least one CFTPART
- There is just one CFTPARM

Where:

- The FOUT option sends the `check `results to a file instead of displaying in the console.

You can use the `check`command with `cftinit `and `cftupdate `using the following syntax:

- `cftinit -check file`
- `cftupdate -check file`

Here, the `-check` option is equivalent to running `CFTUTIL CHECK `at the end of a successful cftinit or cftupdate .

**** ****
