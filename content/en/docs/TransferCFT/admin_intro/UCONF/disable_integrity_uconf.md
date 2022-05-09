{
    "title": "Disable UCONF integrity control",
    "linkTitle": "Disable UCONF integrity control",
    "weight": "340"
}*Available on Unix, Windows, and HP NonStop*

The integrity control checks for corrupted records in the CFTUCONF database file. If a record is corrupted, it is displayed in the trace `cft.out `file and it is no longer accessible. Deleting this record is the only available action.

Integrity control is activated by default `(-`[](../../../c_intro_userinterfaces/command_summary/parameter_intro/mac)`=yes`). To disable integrity control for the UCONF dictionary, add the `-mac=no` parameter to the `cftruntime `command either when creating the runtime environment or regenerating the uconf file:

Unix syntax:

- `cftruntime <cft-install-dir> <cft-runtime-dir> [-profile&#124;-n <name> --uconf --mac=no`
- `cftruntime <cft-install-dir> <cft-runtime-dir> [-profile&#124;-n <name> --inst --mac=no`

Windows syntax:

- `cftruntime <cft-install-dir> <cft-runtime-dir> [-profile&#124;-n <name> -uconf -mac=no`
- `cftruntime <cft-install-dir> <cft-runtime-dir> [-profile&#124;-n <name> -inst -mac=no`
