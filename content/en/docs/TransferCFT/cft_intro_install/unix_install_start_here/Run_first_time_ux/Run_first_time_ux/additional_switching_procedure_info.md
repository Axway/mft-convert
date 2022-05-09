{
    "title": "Switching  procedure",
    "linkTitle": "Switching procedure",
    "weight": "260"
}-   [About
    the switching procedure](#About_the_switching_procedure)
- [Switching
    log files](#Switching_log_procedure)
- [Switching
    accounting files](#Switching_accounting_files)

<span id="About_the_switching_procedure"></span>

About the switching procedure
-----------------------------

Transfer CFT maintains activity traces in primary and alternate files:

- Transfer events
    are stored in two log files, to which the CFTLOG and CFTLOGA environment
    variables point respectively
- Accounting data
    concerning successful transfers is stored in two accounting files, to
    which the CFTACNT and CFTACNTA environment variables point respectively

The switching principle is described in [Housekeeping for logs](../../../../../admin_intro/admin_monitoring_intro/housekeeping_logs).

Switching between the primary and alternate files is configured by the
operator when Transfer CFT is configured. It requires the following definitions:

- The time at which
    switching is to take place each day  
    This switching time is entered using the SWITCH command
- The switching procedure,
    using the EXEC command; this procedure, written in the shell, controls
    the switching operation

The SWITCH and EXEC commands must be added as follows:

- To switch log files,
    the following declaration is used in the CFTLOG section:

```
CFTLOG
ID = log0,
FNAME = '_CFTLOG', /\* Log file      \*/
AFNAME = '_CFTLOGA', /\* Alternate log file      \*/
SWITCH = 2359, /\* Switching time      \*/
EXEC = 'switch.cmd' /\* Switching procedure      \*/
```

- To switch accounting
    files, the following declaration is used in the CFTACNT section:

```
CFTACCNT
ID = acct0,
FNAME = '_CFTACNT',
/\* Accounting file      \*/
AFNAME = '_CFTACNTA',
/\* Alternate accounting file \*/
SWITCH = 2359,
/\* Switching time      \*/
Exec = 'switch.cmd',/\*
Switching procedure     \*/
```

> **Note**
>
> Note: After the switching procedure has completed, the old files used must
> be purged so that they can be reused by Transfer CFT for the next switch.

**If this is not done, Transfer CFT will
*freeze* the next time it is started.**

<span id="Switching_log_procedure"></span>

Switch log procedure
--------------------

In the log file, Transfer CFT begins working on the file that CFTLOG
points to. After the first switch, Transfer CFT uses the file that CFTLOGA
points to. At the next switch, it returns to the file that CFTLOG
points and so on, alternating the files to which CFTLOG and CFTLOGA
point.

With this method, the current and previous, from the day before, log
files are maintained.

Although this is an adequate solution for straightforward operations,
you may wish to have a slightly longer archiving period.

The example below describes a simplified procedure that maintains a
history over four days rather than two. This switching procedure, <span id="switch_cmd"></span>*switch.cmd*, is located
in the *&lt;installdir&gt;/runtime/conf/* directory and is used in the *cft-tcp.conf*
sample configuration.

#### Example: switching log files

This procedure is an example. It does not, for example, take into account
the various error conditions. Its contents are as follows:

```
\#!/bin/sh
\#
\# Sample LOG file switching procedure
\#
filename=`cft2unix &flog`
mv ${filename} ${filename}_sav
CFTUTIL CFTFILE type=log,fname=$filename
rm $0
```

The effects of each line in the procedure are as follows:

- \#!/bin/sh

Use of the BOURNE shell is systematically forced;
even if it is not essential for this example, it is a good safety measure

- filename=`cft2unix
    &FLOG`
- The Transfer
    CFT FLOG symbolic variable is used to retrieve the name of the log file
    to which the CFTLOG environment variable points (Transfer CFT symbolic
    variables are described in the *Transfer CFT Concepts guide*)
- The cft2unix
    utility is provided in the &lt;installdir&gt;/bin directory. It receives the physical
    name of a file if &flog contains a Transfer CFT logical name. Otherwise,
    it returns the name passed as a parameter  
      

Example: the *cft2unix log* command returns *log* whereas
*cft2unix_CFTLOG* returns the value of the CFTLOG environment
variable.

The name of the log file is then stored in the *filename* variable
(*cft_log* for example)

- mv ${filename}
    ${filename}_sav
- The log file
    to which *filename* points is copied to a new file and given the
    *_sav* extension (*cft_log* becomes *cft_log_sav* for example)

<!-- -->

- CFTUTIL CFTFILE
    TYPE=LOG, FNAME=$filename

<!-- -->

- The initial
    log file is recreated. Do not forget that the log file concerned must
    be empty, so that Transfer CFT can use it for switching

<!-- -->

- rm $0

The temporary file is deleted (*see the section Transfer
CFT and Temporary Files*).

<span id="Switching_accounting_files"></span>

Switching accounting files
--------------------------

A simplified version of the previous procedure for switching the accounting
file with the same backup properties is described below. This switching
procedure, *switchacnt.cmd*, is located in the *&lt;installdir&gt;/runtime/conf/* directory
and is used in the *cft-tcp.conf* sample
configuration provided.

#### Example: switching accounting files

```
\#!/bin/sh
\#
\# Sample accounting file switching procedure
\#
filename=`cft2unix &FACCNT`
mv ${filename} ${filename}_sav
CFTUTIL CFTFILE TYPE=ACCNT, FNAME=$filename
rm $0
 
```

This procedure is an example intended to illustrate the concept. It
does not take into account possible error conditions.
