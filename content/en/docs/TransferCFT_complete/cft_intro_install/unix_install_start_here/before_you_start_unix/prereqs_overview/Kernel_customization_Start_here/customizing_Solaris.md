{
    "title": "Solaris: Customize the kernel",
    "linkTitle": "Solaris: Customize the kernel",
    "weight": "220"
}This topic describes how to customize Solaris for {{< TransferCFT/axwayvariablesComponentShortName  >}}.

- About resource controls
- Selecting a project

<span id="Solaris_10"></span>

Solaris 10
----------

{{< TransferCFT/axwayvariablesComponentShortName  >}} makes extensive use of System V IPC services and file access services. As of the Solaris 10 release, the corresponding configuration parameters are no longer kernel parameters, but instead can be defined as resource controls.

<span id="About"></span>

### About resource controls

While most of the corresponding parameter default values are suitable for normal {{< TransferCFT/axwayvariablesComponentShortName  >}} functioning, three of these parameter values must have at least the following minimum values:


| Resource control  | {{< TransferCFT/axwayvariablesComponentShortName  >}} minimum value  |
| --- | --- |
| project.max-shm-memory  | 33554432  |
| process.max-msg-messages  | 8192  |
| process.max-file-descriptor  | 1024  |


This means that you must modify the corresponding resource controls to meet the requirements of the project associated with the user account that {{< TransferCFT/axwayvariablesComponentShortName  >}} will run under.

<span id="Selectin"></span>

### Selecting a project

If you plan to run {{< TransferCFT/axwayvariablesComponentShortName  >}} under the "axway" user account, for example, create a project associated with the user "axway" using the following system command: `projadd -p 201 -G axway -U axway -c "Axway" axway`

This command creates the project "axway" and associates the ****uid axway**** and ****gid axway**** with this project.

Alternatively, you may choose to not associate a specific project with {{< TransferCFT/axwayvariablesComponentShortName  >}}. In this case, {{< TransferCFT/axwayvariablesComponentShortName  >}} will run under a project named "default".

For the following command examples, we use "cft_project" as the name of the project chosen for {{< TransferCFT/axwayvariablesComponentShortName  >}}. This can be either a specific project (named "axway", in the example above) or simply the project "default".

#### Change the maximum size of global memory segments

The following command sets this value to 32 MB:

```
projmod -s -K "project.max-shm-memory=(privileed,33554432,deny) cft_project
```

#### Change the maximum number of messages

The following command sets this value to 8192:

```
projmod -s -K "process.max-msg-messages=(privileged,8192, deny) cft_project
```

#### Change the maximum number of open files per process

The following command assigns a value of 1024 to this parameter:

```
projmod -s -K "process.max-file-descriptor(privileged,1024, deny) cft_project
```
