{
    "title": "Security updates with migration ",
    "linkTitle": "Updating security",
    "weight": "210"
}If your {{< TransferCFT/axwayvariablesComponentLongName  >}} UCONF security setting was` am.type=cft`, you must migrate to am.type=exit in {{< TransferCFT/axwayvariablesComponentLongName  >}} {{< TransferCFT/axwayvariablesReleaseNumber  >}}. This migration process is delivered with Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}}.

Replace habilitation
--------------------

Use the` ..INSTALL library (H81$AMEX)` JCL to activate the new security method. See [Implement security](../../t_start_servers_jobs_zos/post_certificates/install_security_zos/implement_security_zos) for more information on security.

### Configuration file

The configuration file is delivered in the `..UPARM` library (EXAMMVS2). The file describes the basic principles as well as how to map the resources themselves. A best practice is to duplicate this file and limit its access rights to `READ `for all users except the administrator.

### Mapping examples

The resource/action SERVICE: CFTSRV, SHUTDOWN. The existing profile ‘SHUT’ maps to the new resource as follows:

`PC SERVICE: CFTSRV, SHUTDOWN = SHUT, *`

There was no profile for the resource/action SERVICE: CFTSRV, STARTUP so to ensure compatibility, map the new resource as follows:

`PC SERVICE: CFTSRV, STARTUP = STARTUP, * ùùù_insert_pipe_here_ùù_insert_pipe_here_ùùù SHUT, *`

Limitations
-----------

- Issue when creating transfers via the user interface when using resource APPL.\* as the former habilitation.
- **Show my privileges** does not accurately display privileges.
- You can no longer use the FNAME as a criteria to characterize the TRANSFER resource. This can result in regressions on SEND-type transfers. You may need to revise the PERMIT (RACF) definitions for the TRANSFER profile.
- Some issues with MESSAGE resource being confused with the TRANSFER resource.
- You must define a new GENERAL RESOURCE PROFILE UI to manage access to the Transfer CFT user interface, connecting authorized users or groups to this profile.
- You must define a FILE GENERAL RESOURCE PROFILE used with the CREATE/DELETE parameter files with the CFTFILE command.
- The same FILE resource in access (READ) allows to limit the access to the directories via the UI.
- For the copilot.rootdirs definition, the asterisk '\*' character must be unique and in the last position.
