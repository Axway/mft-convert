{
    "title": "HP-UX: Customize the kernel",
    "linkTitle": "HP-UX: Customize the kernel",
    "weight": "200"
}This topic provides information on customizing the HP-UX kernel, and how to:

- Modify
    the message queue depth
- Modify
    the number of files used by a process

<span id="Modify_the_message_queue_depth"></span>

Modify the message queue depth
------------------------------

By default, HP-UX only allows a maximum of 256 unread messages to transit
in a message queue. To customize this information, increase the value
of the *msgtql* kernel parameter to 8192.

To customize the message queue:

1. Log in as the root and enter
    the sam command.
1. Select the following sub-menus,
    respectively:
    -   Kernel
        Configuration
    -   Configurable
        Parameters
1. Set the pointer to the *msgtql*
    parameter.
1. Select the *Modify
    Configurable Parameter* option in the *Actions*
    menu.
1. Enter the new value of this
    parameter.
1. Click on *OK*
    to confirm the change.
1. Select the *Add
    Exported File System* option in the *Actions*
    menu.
1. Select the *Exit*
    option in the *File*
    menu.
1. Select the *Create
    a new kernel now* option and click on *OK*
    to confirm your selection.

<span id="Modify_the_number_of_files_used_by_a_process"></span>

Modify the number of files used by a process
--------------------------------------------

By default, HP-UX allows a process to open only 64 files. To customize
this information, increase the value of the maxfiles
kernel parameter to 1024. To do so:

1. Log in as the *root* and
    enter the sam
    command.
1. Select the following sub-menus:
    -   Kernel
        Configuration
    -   Configurable
        Parameters
1. Set the pointer to the *maxfiles*
    parameter.
1. Select the *Modify
    Configurable Parameter* option in the ****Actions****
    menu.
1. Enter the new value for this
    parameter.
1. Click on *OK*
    to confirm the change.
1. Select the *Add
    Exported File System* option in the *Actions*
    menu.
1. Select the *Exit*
    option in the *File*
    menu.
1. Select the *Create
    a new kernel now* option and click on *OK*
    to confirm your selection.
