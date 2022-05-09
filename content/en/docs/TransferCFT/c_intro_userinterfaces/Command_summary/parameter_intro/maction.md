{
    "title": "maction",
    "linkTitle": "maction",
    "weight": "1880"
}<span id="maction"></span>

### maction

#### CFTRECV, RECV

****[ MACTION = { '  '
&#124; replace } ]****

**Applicable only on Windows and z/OS environments. On Unix environments, files are systematically replaced.**

When receiving a group of files, action for the files as they are deconcatenated. Used only in homogeneous mode.

- “   ”:
    If the files already exist in the directory, the copy of these
    files is ignored
- REPLACE:
    -   Unix: Always replace files
    -   Windows and z/OS: Only replace files if they are newer than the existing file
    -   IBM i and OpenVMS: Not available

[Return to Command index](../../)
