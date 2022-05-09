{
    "title": "UNIX-specific unified configuration options",
    "linkTitle": "UNIX specific uconf settings ",
    "weight": "290"
}This topic describes the unified configuration values to define for the following Unix options in {{< TransferCFT/axwayvariablesComponentShortName  >}}:

- [Command line history: enable readline](#Command)
- [System user: define CFTSU](#System)
- [Separating filenames from the file extension](#Separat)
- [General Unix-specific parameters](#UNIX)

<span id="Command"></span>

Command line history (readline)
-------------------------------


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.unix.readline.enable  | YES &#124; NO  | Enable readline history for CFTUTIL and PKIUTIL.  |
| cft.unix.readline.history_fname  | $(HOME)/.cft_history  | Readline history file.  |
| cft.unix.readline.history_size  | 1000  | Readline history size.  |


<span id="System"></span>

System user
-----------


| Parameter  | Description  |
| --- | --- |
| cft.unix.cftsu.afunix  | Defines the address family file for inter-process communications.  |
| cft.unix.cftsu.isservice  | Defines the use of CFTSU as a service.  |
| cft.unix.cftsu.fname  | Specify the absolute path name to the CFTSU to execute when the user control is enabled, to enable upgrading without being the system administrator. See Unix: [Using system users](../t_adding_system_user_unix). |


<span id="Separat"></span>

Separating filename and extension
---------------------------------


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.unix.parse_file_name_suffix  | No  | Use this setting for a {{< TransferCFT/axwayvariablesComponentShortName  >}} running in Unix to separate the extension from the name of the file, as is done in Windows.<br/> Possible values:<br/> • Yes: Enables Unix to mimic Windows functioning so that the name of the file and the type of extension are presented separately<br/> • No: Disables the feature to have normal Unix file functioning with no extension type displayed |


See [Suffix management](../suffix_management).

<span id="UNIX"></span>

UNIX parameters
---------------


| ID  | Default  | Former value  |
| --- | --- | --- |
| cft.unix.start_timeout  | (int) |  30 |
| cft.unix.stop_timeout | (int) | 30 |
| cft.unix.passwd_fname |  (fname) | $(cft.runtime_dir)/conf/passwd |
| cft.unix.passwd_temp  | (fname) | $(cft.runtime_dir)/conf/passwdXXXXXX |
| cft.unix.group_fname | (fname) | $(cft.runtime_dir)/conf/group |
| cft.unix.group_temp |  (fname) |  $(cft.runtime_dir)/conf/groupXXXXXX |
| cft.unix.pid_fname |  (fname) | $(cft.runtime_dir)/run/cft.pid |
| cft.unix.ipc_fname  | (fname) | $(cft.runtime_dir)/run/cft.ipc |
| copilot.unix.unix_socket_fname  | (fname) | $(cft.runtime_dir)/run/S_COPSMNGFW |
| copilot.unix.pid_fname |  (fname) | $(cft.runtime_dir)/run/copilot.pid CFTCOPILOTPID |

