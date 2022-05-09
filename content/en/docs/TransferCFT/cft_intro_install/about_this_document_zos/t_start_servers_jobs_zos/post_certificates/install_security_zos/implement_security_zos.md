{
    "title": "Implement  security",
    "linkTitle": "Implement  security",
    "weight": "300"
}Once the installation procedures are customized, they are designed to generate a minimum security environment for an existing instance of Transfer CFT, while allowing users to keep their original access rights. This environment can then be expanded according to requirements, but the procedure must be performed by the Transfer CFT administrator (cftadm).

This section describes how to customize and implement the Transfer CFT z/OS security by:

- Creating Profiles for Transfer CFT Objects (H84SAFDF)
- Authorizing Access to Transfer CFT Objects (H85SAFPR)
- Creating the DEFAULT CFTAPPL (H88PARM)
- Protecting Transfer CFT Files (H89SAFAS)
- Authorizing Transfer CFT Users (H89SAFAU)

Customize and implement Transfer CFT z/OS security
--------------------------------------------------

### Create profiles for Transfer CFT objects (H84SAFDF)

The Transfer CFT administrator should already be declared in RACF with sufficient rights to define general resources and assign access rights to the various Transfer CFT users.

### Authorize access to Transfer CFT objects (H85SAFPR)

This JOB issues all of the required RACF PERMIT commands.

### Create the DEFAULT CFTAPPL (H88PARM)

This job is used to edit the configuration and include a CFTAPPL command for the DEFAULT identifier. This command enables all transfers to be performed via the local application (APPL). See the CFTPARM command DEFAULT parameter for more information.

If the CFTPARM command does not contain the DEFAULT parameter, users must replace the CFTIN file with the CFTAPPL commands corresponding to the CFTSEND and/or CFTRECV commands.

The CFTRECV ID=DEFAULT command is used to define users other than the monitor user, so that files can be received under their control. You are strongly advised against performing transfers with the monitor user. An owner is assigned to the transfers by setting the USERID= and GROUPID= parameters in the CFTRECV or CFTAPPL DIRECT=RECV commands.

### Protect Transfer CFT files (H89SAFAS)

This job is used to protect Transfer CFT files. By default user groups do not have access to files. Each group created by the H83SAFDA job is granted file access rights by executing the H89SAFAS job. Any user wishing to perform configuration or transfer operations must be associated with one of these groups.

Some groups only access files in read or write mode under Transfer CFT program control (CFTUTIL, CFTCOPL, and so on). If other user programs are to be added to the list, a PADS must be executed on them.

If required, update the CFTENV member:

`000073 //* SAF parameters`

`000074 //      SET SAFCFTCL='xxxxxxx'`

### Authorize Transfer CFT users (H89SAFAU)

Use this job to provide a model for the commands to be executed to allow:

- A user to update the Transfer CFT configuration (example: CFTSEND command)

<!-- -->

- A user to submit transfer requests  (example: SEND command)

<!-- -->

- A transfer owner to send or receive a file (example: APPL object)

<!-- -->

- Users to be associated with a group so that they can be granted Transfer CFT file access rights
