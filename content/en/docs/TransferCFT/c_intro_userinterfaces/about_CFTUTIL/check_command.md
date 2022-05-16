---

    title: Use the check command
    linkTitle: Use the check command
    weight: 180

---
The CFTUTIL <span class="code">`CHECK `</span>command validates the coherence of parameters, partners, and the Transfer CFT PKI database.

The syntax is:

```
CHECK CONTENT=<u>BRIEF</u>|FULL, FOUT=FileName
```

The <span class="code">`CHECK CONTENT=BRIEF`</span> (default) command verifies that:

- All the referenced objects exist
- Each CFTPART has an associated CFTTCP
- There is at least one CFTPARM
- The used certificates have not expired

Any encountered errors are displayed in the console, and we highly recommend that you fix them before starting Transfer CFT.

The <span class="code">`CHECK CONTENT=FULL, FOUT=FileName`</span> command also checks that:

- All objects are used
- No CRONTAB is empty
- Each CFTPROT is used by at least one CFTPART
- There is just one CFTPARM

Where:

- The FOUT option sends the <span class="code">`check `</span>results to a file instead of displaying in the console.

You can use the <span class="code">`check`</span>command with <span class="code">`cftinit `</span>and <span class="code">`cftupdate `</span>using the following syntax:

- `cftinit -check file`
- `cftupdate -check file`

Here, the <span class="code">`-check`</span> option is equivalent to running <span class="code">`CFTUTIL CHECK `</span>at the end of a successful cftinit or cftupdate .

**** ****
