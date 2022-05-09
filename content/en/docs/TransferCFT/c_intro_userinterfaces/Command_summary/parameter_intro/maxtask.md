{
    "title": "maxtask",
    "linkTitle": "maxtask",
    "weight": "1940"
}<span id="maxtask"></span>

### maxtask

#### CFTPARM

**MAXTASK = { <span class="underline">8</span>
&#124; n}    **

Enter the number of authorized file access tasks (default = 8). This refers to the number of authorized file access tasks, for example CFTTFIL. The used value may be recomputed, and be greater than the defined value, depending on the fixed number of files a task can handle on the system.

The following table indicates the maximum number supported for each
system.

> **Note**
>
> Note: When MAXTASK is set to one, a high TRANTASK value is useless.


| OS  | Maximum number supported  |
| --- | --- |
| UNIX  | 64 |
| Windows  | 64 |
| z/OS (MVS) | 400 |
| IBM i | 64 |
| OpenVMS  | 64 |


The following CFTI18I  message displays in the CFTLOG when Transfer CFT is started so that you can view the actual MAXTRANS, MAXTASK, TRANTASK values.

****Example****

```
CFTI18I+MAXTRANS=128, MAXTASK=16, TRANTASK=3
```

[Return to Command index](../../)
