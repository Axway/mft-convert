{
    "title": "Managing audit logs",
    "linkTitle": "Manage audit logs - verify avec ST / GTW",
    "weight": "250"
}{{< TransferCFT/headerfootervariableshfoverview  >}}
---------------------------------------------------------

{{< TransferCFT/axwayvariablesComponentLongName  >}} features a compulsory audit log that tracks user authentication, administration, configuration, and denied transfer actions, which is saved in a text file. This feature can be useful in helping to diagnose Transfer CFT production problems or for security purposes  and can be scanned by supervisory tools in production environments. Additionally, it is included when you run the `cft_support` command.

Value descriptions
------------------

Each string in the audit log file provides context for an action, the “when, status ,who, where, and what”.

****WHEN****

This is timestamp that uses the ISO 8601 format time zone YYYY-MM-DDTHH:MM:SS.mmm+/-hh:mm. Where: YYYY: year, MM: month, DD: Day, HH: hour: MM: minute: SS: second, mmm: millisecond: Timezone offset from UTC in the format ±[hh]:[mm] for hours and minutes.

Example timestamp: 2022-01-13T15:55:31.585+0100.

****STATUS****

This is the execution status of the action. The possible values are:
&lt;/p&gt;

- Done: Action executed without error
- Failed: Action failed
- Denied: Action not allowed for the user

****WHO****

This is the user performing the action, which is a single name on most operating systems. On Windows, the format is Domain\\User.

****WHERE****

This has the format Tools@Workstation, where:

Workstation is where the action was performed, either the computer name or its IP address.

Tool indicates which tool performed the action:

UTIL: CFTUTIL

PKIUTIL: PKIUTIL

UI: The CFT Web User Interface

REST: A Rest API call from Swagger, curl or another application

WS: A Web Service call (Soap) from an application

FM: Flow Manager (should we say here {{< TransferCFT/PrimaryCGorUM  >}} also?)

cft: cft commands mainly for multi-node

- cftstart: Start Transfer CFT server
- cftstop: Stop Transfer CFT server
- copstart: Start Copilot server
- copstop: Stop Copilot server

> **Note**
>
> Note: The format on z/OS platforms is Tools:jobname.jobid@Workstation.

****WHAT****

This is the action and action details.

The possible values are:
&lt;/p&gt;

- Authentication: Login or logout
- Configuration: Modification in a Transfer CFT object or a Transfer CFT uconf parameter
- Administration: Start/stop Transfer CFT and Copilot, initialization, and multi-node configuration
- Transfer: Logged only if the Send or Receive transfer is denied

Action details
--------------

The possible actions are:

- Login, Logout
- Start CFT, CFT is starting, CFT started, CFT failed to start (including the force and fast options)
- Stop CFT, CFT is stopping, CFT stopped, CFT failed to stop (including the force and fast options)
- Restart CFT, CFT is restarting
- CFT copilot is starting, CFT copilot started, CFT copilot failed to start
- CFT copilot stop
- UCONFSET ID='uconf_parameter', VALUE='old value' -&gt; 'new value'
- UCONFUNSET ID='uconf_parameter', OLD_VALUE='old value'
- CREATE object param1='value1,param2=value2, ...
- DELETE object param1='value1,param2=value2, ...
- UPDATE cft_object param1='old value1'&gt;'new value1',param2='old value2'&gt;'new value2', ...
- UPDATE pki_object param1='value1,param2=value2, ...
- Command parameters for CFTFILE, PKIFILE, cftinit, cftupdate, cft (for multi-node), SEND , RECV

> **Note**
>
> Note: When using the UPDATE command, only the updated and key parameters are listed.

Audit file retention and format
-------------------------------

The audit log files are saved in the `audit `folder as one file per day. The file name format is:

- Windows, UNIX, HP NonStop, OpenVMS: audit_YYYYMMDD
- z/OS, IBM i: AYYMMDD

### Customize the file location

You can use the UCONF cft.runtime.audit_dir parameter to customize this file.

On z/OS, you can configure the audit file as a native file or a USS file using the cft.runtime.audit_dir parameter. If that parameter followed by a slash (/), the audit is saved as a USS file. For example, `uconfset id=cft.runtime.audit_dir, value='/home/user/USERNAME/AUDIT/'`

### Purging audit files

The files are purged as defined in the uconf `cft.audit.retention` parameter (default value is 30 days). Set the parameter to zero (0) to keep only the current day's file.

Limitations
-----------

- When using in conjunction with Flow Manager:
    -   The WHERE parameter contains the name of the Flow Manager server and not the workstation where the browser action was launched.
    -   The user is the {{< TransferCFT/axwayvariablesComponentLongName  >}} user.
