---

    title: Assigning  a transfer owner
    linkTitle: Transfer owner assignment - CFTAPPL
    weight: 240

---
This section describes how to configure access management when not using {{< TransferCFT/PrimaryCGorUM  >}}.

For a specified transfer and file or message identifier, Transfer CFT
assigns the USERID identifier defined in the corresponding CFTAPPL command.
The user in question becomes the owner of the transfer in place of the
request sender.

If the security system is active, at least one default CFTAPPL command
must exist for each transfer direction. The identifier used by this command
is the default value defined in the general parameters. [\[CFTPARM\]](../../../../admin_intro/admin_config_commands/cftparm_general_parameters)

Identifiers with the same prefixes can be grouped together in a single
CFTAPPL command.

The relationship between the identifier of the transferred file or message
(IDF or IDM) and the command identifier may concern all or part of the
identifier. In the second case, the length of the comparison is defined
in the general parameters (refer to the CFTPARM LENAPPL parameter later
in this section).

If neither the corresponding CFTAPPL command nor the default command
can be found, the transfer is refused.

For more details, refer to<span class="italic_in_para"> Pre-Transfer Controls</span>
and the Requester/Sender and Server/Sender examples in
the previous topic.

If the site uses switching, a CFTAPPL command with the COMMUT identifier
must exist, irrespective of the LENAPPL value defined in CFTPARM.

<span id="CFTAPPL_syntax"></span>

### CFTAPPL syntax

```
CFTAPPL    
     [MODE       =    
{CREATE | DELETE | REPLACE},]
     ID          =    
identifier,
     USERID      =    
identifier,
     [GROUPID    =    
identifier,]
     [DIRECT     =    
{BOTH | RECV | SEND}]
```

### Parameters

****\[DIRECT     = {BOTH
| RECV | SEND}\]****

Transmission direction.

Possible values are:

- BOTH:
    associates an owner with the transfer, irrespective of the direction
- RECV:
    associates an owner with receive transfers (CFTRECV/RECV)
- SEND:
    associates an owner with send transfers (CFTSEND/SEND)

****\[GROUPID     = {identifier
| &GROUPID}\]****

Identifier of the group that owns the transfer.

When privileges are checked, if the value specified is &GROUPID,
the variable is replaced by the GROUPID that created the transfer.

****ID     = identifier****

Command identifier (eight characters).

If a specific CFTAPPL command is used, the identifier corresponds to
the value of the CFTSEND/CFTRECV ID parameters, or to the SEND/RECV IDF
(IDM) parameters.

If the default CFTAPPL command is used, the identifier is set to the
default value defined in the general parameters (DEFAULT parameter of
the CFTPARM command).

If a generic CFTAPPL command is used, identifiers with the same prefixes
are grouped together in a single command. The length of the identifier
is defined in the general parameters (LENAPPL parameter of the CFTPARM
command).

The ? and/or \* wildcard characters may be used to represent any single
character and any substring respectively.

****MODE     = {REPLACE
| CREATE | DELETE}****

Operation to be executed.

Possible values are:

- REPLACE:
    modifies one or more records, or creates them if they do not exist
- CREATE:
    creates one or more records
- DELETE:
    deletes one or more records

****USERID     = {identifier
| &USERID}****

Identifier (8 characters) of the transfer owner.

When privileges are checked, if the value specified is &USERID,
the variable is replaced with the USERID that created the transfer.

 

This section describes how to configure access management when not using {{< TransferCFT/PrimaryCGorUM  >}}.

The concept of a local application is used by Transfer CFT when submitting
transfer requests and executing the associated transfers. It corresponds
to an object represented by the value of the local file identifier associated
with the CFTSEND or CFTRECV commands and with the logical identifier of
the file to be sent via the SEND or RECV requests.

By assigning an owner to the <span id="local_applicatio"></span>local application
object, you can guarantee a secure transfer on both the requester and
server sides.

<span id="Implementation"></span>

## Implementing

The CFTAPPL command is used to create the local application.

This command:

- Defines a user
    as being the owner of a transfer or group of transfers: broadcast list,
    generic requests
- Defines a user
    as being the owner of the transferred files
- Specifies the transfer
    direction

See also <a href="#" class="selected">Transfer
owner CFTAPPL</a>.

<span id="Pre_transfer_controls"></span>

### Pre-transfer controls

Transfer CFT scans the configuration to locate a CFTAPPL command, the
identifier of which is the same as the IDF assigned to the SEND or RECV
request.  
If such an identifier is located, the user specified in the CFTAPPL command
becomes the transfer owner.

Otherwise, Transfer CFT searches for a CFTAPPL command, the identifier
of which is equal to the value of the DEFAULT parameter in CFTPARM.  
If such an identifier exists, the user specified in the CFTAPPL command
becomes the transfer owner.

Otherwise, given that identifiers with the same prefixes can be grouped
together in the same CFTAPPL command, Transfer CFT performs a generic
search on the CFTAPPL identifiers.

The length of the comparison is then specified in the LENAPPL parameter
of the CFTPARM command.

In all other cases, the transfer is rejected.

<span id="Requester_sender_configuration"></span>

### Requester/sender configuration

#### Using a specific CFTAPPL command

USER1 creates a SEND request for an IDF corresponding to a CFTAPPL command
with the USER2 userid:

- USER1 becomes the
    owner of the SEND command
- USER2 becomes the
    owner of the transfer

On the server side, a CFTAPPL command corresponding to the same IDF
contains the USER3 userid:

- USER3 becomes the
    owner of the transfer
- the user who submitted
    the RECV command becomes the owner of the command

#### Requester/Sender with CFTAPPL

![](/Images/TransferCFT/requester_sender_CFTAPPL.gif)

#### Using the default CFTAPPL command

USER1 creates a SEND request for an IDF that does not correspond to
a CFTAPPL command. There is a default CFTAPPL command:

- USER1 becomes the
    owner of the SEND command
- USER2 becomes the
    owner of the transfer

On the server side, a CFTAPPL command corresponding to the same IDF
contains the USER3 userid:

- USER3 becomes the
    owner of the transfer
- The user who submitted
    the RECV command becomes the owner of the command

Requester/Sender with the Default CFTAPPL

![](/Images/TransferCFT/sender_rec_CFTAPPL_default.gif)

<span id="Server_sender_configuration"></span>

### Server/sender configuration

#### Using a Specific CFTAPPL Command

USER1 creates a RECV request for an IDF corresponding to a CFTAPPL command
with the USER2 userid:

- USER1 becomes the
    owner of the RECV command
- USER2 becomes the
    owner of the transfer

On the server side, a CFTAPPL command corresponding to the same IDF
contains the USER3 userid:

- USER3 becomes the
    owner of the transfer
- the user who submitted
    the SEND command becomes the owner of that command

****Requester/sender with CFTAPPL****

![](/Images/TransferCFT/req_sender_with_CFTAPPL.gif)

#### Using the Default CFTAPPL Command

USER1 creates a RECV request with an IDF corresponding to a CFTAPPL
command with the USER2 userid:

- USER1 becomes the
    owner of the RECV command
- USER2 becomes the
    owner of the transfer

On the server side, an implicit CFTSEND command corresponding to the
same IDF contains the USER3 userid. USER3 is the owner of the transfer.

****Server/sender with the default CFTAPPL
SEND****

![](/Images/TransferCFT/server_sender_CFTAPPL.gif)

The same processing takes place when the IMPL=YES option is set.

<span id="Rights_for_users_and_files"></span>

## Rights for users and files

Transfer CFT files must be protected by the operating system and can
only be accessed via Transfer CFT components: batch and interactive utilities,
APIs.

To ensure security system portability, there are never any default privileges.
Any user who has not been declared is granted no rights.

Special case for the external security mode: the transfer owner is also
the user processing the file to be sent.
