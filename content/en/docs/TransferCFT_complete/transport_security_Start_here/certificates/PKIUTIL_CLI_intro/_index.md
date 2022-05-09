{
    "title": "PKIUTIL  command line utility: Start here",
    "linkTitle": "Command line operations",
    "weight": "190"
}This section describes the PKIUTIL commands that are used to manage the
local certificate database. This topic
introduces the command line PKIUTIL utility CFTPKI.

Overview
--------

The PKIUTIL command topics are as follows:

- [ACT/INACT](using_act_inact)
    Activate/deactivate a certificate
- [PKICER](using_the_pkicer_command)
    Import, delete, or update a certificate in the database
- [LISTPKI](using_the_listpki_command)
    Display the internal datafile contents
- [PKIFILE](using_the_pkifile_command)
    Create/delete the local certificate database
- [PKIENTITY](pkientity) Create lists of certificate authority IDs in the PKI database
- PKIKEY Import, delete, or update a key in the PKI database
- [PKIKEYGEN](pkikeygen) Generate a new key and insert in the PKI base

When activated, PKIUTIL uses the local default certificate database.
For more information, refer to the Transfer CFT Operations
Guide specific to your OS.

The next topic describes how to use the [CFTUTIL
display command](cftutil_utility_display_commands) for transport security uses.

Autocomplete command
--------------------

****UNIX/Windows only****

To simplify the use of `PKIUTIL `commands, you can use the autocomplete feature when working in interactive mode. See [Autocomplete](../../../c_intro_userinterfaces/about_cftutil/autocomplete).

****Related topics****

- [CFTPARM](../../../admin_intro/admin_config_commands/cftparm_general_parameters)
