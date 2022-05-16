---

    title: sslttask
    linkTitle: sslttask
    weight: 3350

---
<span id="sslttask"></span>

### sslttask

#### CFTPARM

****\[SSLTTASK = <u>3</u> | n \]****

The maximum number of simultaneous network sessions guaranteed by an
SSL task (default = 3). Above this number, a new task is created, if necessary.

If the maximum number of SSL tasks is reached (<span style="font-weight: bold;">****sslmtask****</span>
value), additional sessions are evenly distributed between all SSL tasks.

The following values are supported:

- <span style="font-weight: bold;">****1****</span>
    to <span style="font-weight: bold;">****64****</span>
    for all operating systems other than Windows
- <span style="font-weight: bold;">****1****</span>
    to <span style="font-weight: bold;">****1000****</span> for Windows

[Return to Command index](../../)
