{
    "title": "sslttask",
    "linkTitle": "sslttask",
    "weight": "3370"
}<span id="sslttask"></span>

### sslttask

#### CFTPARM

****[SSLTTASK = <span class="underline">3</span> &#124; n ]****

The maximum number of simultaneous network sessions guaranteed by an
SSL task (default = 3). Above this number, a new task is created, if necessary.

If the maximum number of SSL tasks is reached (****sslmtask****
value), additional sessions are evenly distributed between all SSL tasks.

The following values are supported:

- ****1****
    to ****64****
    for all operating systems other than Windows
- ****1****
    to ****1000**** for Windows

[Return to Command index](../../)
