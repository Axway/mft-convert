{
    "title": "Install with security",
    "linkTitle": "External access management  (RACF)",
    "weight": "280"
}To protect its objects, Transfer CFT uses the SAF interface (System Authorization Facility) on z/OS platforms to address authorization requests to an ESM, such as RACF. Transfer CFT does not perform any checks itself, and accepts the ESM decision. Before implementing specific protection, you must protect any Transfer CFT objects that the operating system recognizes from a file, an operator command, or program protection mechanisms.

When installing Transfer CFT, all procedures that update RACF must be performed by users with RACF rights. Other procedures are performed by the Transfer CFT administrator. For information on the Transfer CFT administrator definition, see the H83SAFDA member details.

Transfer CFT installation procedures
------------------------------------

Transfer CFT software can be installed with or without the security system. In all cases, you must follow the installation procedure indicated in the Transfer CFT z/OS Installation and Operations Guide. The security system is intended to be installed on systems already running Transfer CFT. It is installed in several steps; each represents control levels with increasing severity.

- Level 0: Transfer CFT without security

<!-- -->

- Level 1: Transfer CFT with security controlling configuration files and transfers

Level 2: corresponds to level 1 with an additional program-based control of accesses to CFT files (PADS) (USERCTRL=YES)

### Access management under {{< TransferCFT/PrimaryCGorUM  >}}

When running under {{< TransferCFT/PrimaryCGorUM  >}}, the access management is automatically set to am.type=passport. If you want to use external access management, RACF, you must manually modify this value after registering with {{< TransferCFT/PrimaryCGorUM  >}} using the command:

```
CFTUTIL UCONFSET id=am.type, value=internal
```

### Installation prerequisites

#### Creating RACF classes

The security system uses a general resource class, safcftcl. It is created using the H81SACDT (or H81SAFCD and H82SAFRT) job, which is adapted from the samples provided in the SYS1.SAMPLIB RACTABLE member.

- H81SACDT / H81SAFCD: Creates a RACF class (to adapt to requirements) H81SACDT is based on the RACF dynamic class definition. H81SAFCD is the former linked class table.

<!-- -->

- H82SAFRT: Creates a router table (to adapt to requirements), and is only required with H81SAFCD.

<!-- -->

- MVS IPL: Applies RACF classes, needed only with the H81SAFCD JOB.

#### Creating RACF groups and users

- H83SAFDA: Creates groups and users

Once the H83SAFDA job is adapted, use this job to create groups and the Transfer CFT administrator (admcft) responsible for installing the security system. This user must be able to create and manage general resource profiles for the Transfer CFT class (safcftcl), and perform all installation-related procedures (file creation rights).

#### Modify the existing configuration

- A00CUSTO: Customizes installation files  
      
    After adapting the security system values in the A03PARM member, you must execute the A00CUSTO job again.

```
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
\*   New parameters for RACF (or SAF enabled) control of Transfer CFT.
\*   These definitions are not used in the base installation.
\*   You must read the “Transfer CFT Installing z/OS Security”
\*   before changing the following values:
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
  grpcft   'grpcft'               CFT administrator SAF group
  grpmon   'grpmon'               CFT monitor SAF group
  grpaprm  'grpaprm'              all parameter access SAF group
  grpfprm  'grpfprm'              PARM and PART access SAF group
  grpdesk  'grpdesk'              CFT help desk SAF group
  grptrf   'grptrf'               CFT transfer SAF group
  userdef  'userdef'              default CFTRECV userid
  safcftcl 'safcftcl'             SAF class for CFT profiles    
```

Security system installation procedure
--------------------------------------

When the general resource class (safcftcl) is applied by RACF, the authorized user can begin the installation to implement the minimum (Transfer CFT and RACF) resources required to run the security system. You must perform the installation procedures in the order shown in the following table.

#### Security system installation procedures


| Job | Installation procedure |
| --- | --- |
| H84SAFDF | Creates RACF general resource CFT profiles |
| H85SAFPR | Executes RACF PERMIT commands |
| H88PARM | Edits the parameter file (adds CFTAPPL) |
| H89SAFAS | Creates CFT file protection profiles with PADS (upgrades from security level 1 to level 2) |
| H89SAFAU | Sample allowing certain users to modify parameters and/or perform transfers |


#### Delete security definitions


| Job | Description |
| --- | --- |
| H89SAFDD | Used to delete the RACF definitions created by the H84SAFDF job. |
| H89SAFDS | Used to delete the RACF definitions created by the H89SAFAS job. |

