{
    "title": "Defining Transfer CFT user descriptions",
    "linkTitle": "Define Transfer CFT users",
    "weight": "310"
}This section describes how to define the Transfer CFT user description in z/OS. The user groups are:

- Administrator group (GRPCFT)

<!-- -->

- Transfer group(grpmon)

<!-- -->

- Configuration (update) group (GRPAPRM)

<!-- -->

- Configuration Group(grpfprm)

<!-- -->

- Help desk group (GRPDESK)

<!-- -->

- File transfer group (GRPTRF)

About user descriptions
-----------------------

In order to modify Transfer CFT files for configuration purposes, transfers, and so on, you must be authorized to access the following:

- The file (in read or write mode)

<!-- -->

- Transfer CFT objects protected by safcftcl class profiles

If one or other of the requirements is not met, the operation fails. To simplify authorization implementation in RACF, groups have been defined to allow files and safcftcl class profiles to be accessed. These groups are:

- The grpcft group has file and safcftcl class profile access rights, to enable administrators to manage all resources.

<!-- -->

- The grpmon group has file and safcftcl class profile access rights, to enable the monitor to perform all transfers.

<!-- -->

- The grpaprm group has safcftcl class profile access rights, to enable users belonging to the group to edit all Transfer CFT parameters.

<!-- -->

- The grpfprm group has file access rights, to enable users belonging to the group to access the configuration files.

<!-- -->

- The grpdesk group has file and safcftcl class profile access rights, to enable operators to manage the monitor.

<!-- -->

- The grptrf group has file access rights, to enable users belonging to the group to submit transfer requests.
