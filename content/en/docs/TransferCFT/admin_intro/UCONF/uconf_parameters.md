---

    title: UCONF: General unified configuration parameters
    linkTitle: General configuration parameters
    weight: 280

---
Unified configuration, or UCONF, settings and default values are listed in tables
and grouped into the following categories:

- [Identifier parameters](#Identifi)
- [Instance
    parameters](#Instance)
- [Packaging parameters](#Packagin)
- [Work environment
    parameters](#Work)
- [Common parameters](#Common)
- [Trace parameters](#Trace)
- [{{< TransferCFT/axwayvariablesComponentShortName >}}
    probe configuration](#Transfer2)
- [Add a character set: transcoding](#Add)
- [Accounting records](#Statisti)
- [Compatibility](#Compatib)
- [Deactivate the default IDF](#Deactivate_idf)

<span id="Identifi"></span>

## Identifier parameters


| UCONF  | Default  |
| --- | --- |
| cft.uconf.default_fname  | $(cft.install_dir)/distrib/conf/cftuconf-common.dat  |
| cft.uconf.instance_fname  | $(cft.runtime_dir)/data/cftuconf.dat  |
| cft.uconf.runtime_fname  | $(cft.runtime_dir)/data/cftuconf-run.dat  |


<span id="Instance"></span>

## Instance parameters


| ID  | Default  | Former value  |
| --- | --- | --- |
| cft.runtime_dir | (dir) |  $(CFTDIRRUNTIME) |
| cft.install_dir | (dir) |  $(CFTDIRINSTALL) |
| cft.working_dir | (dir) | $(cft.runtime_dir) |
| cft.uconf.common_fname | (fname) |  $(cft.install_dir)/distrib/conf/cftuconf-common.ini |
| cft.uconf.system_fname | (fname) | $(cft.install_dir)/distrib/conf/cftuconf-system.ini |
| cft.run.msg |   | New |
| cft.run.pid | 38016 | New |
| cft.run.progress | 100 | New |
| cft.run.state | RUNNING | New |
| copilot.run.client_socket | 740 | New |
| copilot.run.manager_pid  | 44920 | New |
| copilot.run.notification_port  | 2953 | New |


<span id="Packagin"></span>

## Packaging parameters

The following table lists the UCONF identifiers, default values, and former Windows and UNIX file values.
&lt;/p>


| ID  | Default  | Windows  | UNIX  |
| --- | --- | --- | --- |
| cft.runtime_dir  | $(CFTDIRRUNTIME)  |   |   |
| cft.install_dir  | $(CFTDIRINSTALL)  |   |   |
| cft.synchrony_dir  |   |   |   |
| cft.cftcat.fname | $(cft.runtime_dir)/data/cftcata  | $CFTCATA(cft.ini)  | _CFTCATA  |
| cft.cftcat.default_size  | 10000  |   |   |
| cft.cftcom.default_size  | 1000  |   |   |
| cft.cftparm.fname  | $(cft.runtime_dir)/data/cftparm  | $CFTPARM  | _CFTPARM  |
| cft.cftparm.partfname  | $(cft.runtime_dir)/data/cftpart  | $CFTPART  | _CFTPART  |
| cft.cftparm.pkifname  | $(cft.runtime_dir)/conf/pki/pkibase  | $CFTPKU  | _CFTPKU  |
| cft.cftparm.habfname  | $(cft.runtime_dir)/sec.ini  | $CFTHINI  | _CFTHINI  |
| cft.cftparm.secparm  | $(cft.runtime_dir)/data/secparm  | $CFTHPARM  | _CFTHPARM  |
| cft.cftparm.keyfname  | $(cft.runtime_dir)/conf/cft.key  |   |   |
| cft.cftcom.fname  | $(cft.runtime_dir)/data/cftcom  | $CFTCOM  | _CFTCOM  |
| cft.cftlog.fname  | $(cft.runtime_dir)/log/cftlog  | $CFTLOG  | _CFTLOG  |
| cft.cftlog.afname &gt;  | $(cft.runtime_dir)/log/cftalog  | $CFTALOG  | _CFTALOG  |
| cft.cftaccnt.fname  | $(cft.runtime_dir)/log/cftaccnt  | $CFTACCNT  | _CFTACNT  |
| cft.cftaccnt.afname  | $(cft.runtime_dir)/accnt/cftaccnt  | $CFTAACCN  | _CFTACNTA  |


<span id="Work"></span>

## Work environment parameters


| ID  | Default  | Description  |
| --- | --- | --- |
| cft.working_dir  | $(cft.runtime_dir)  | Sets the {{< TransferCFT/axwayvariablesComponentShortName  >}} work-environment  |
| cft.idparm  | IDPARM0  | Sets the IDPARM to use in Copilot (GUI) and {{< TransferCFT/axwayvariablesComponentShortName  >}} (optional)  |


<span id="Common"></span>

## Common parameters

Values for ID where the type is Common.


| ID  | Default  | Former value  |
| --- | --- | --- |
| cft.uconf.shared_fname | (fname) | $(cft.runtime_dir)/data/cftuconf-shared.ini  |
| cft.uconf.shared_fname_bak | (fname) | $(cft.uconf.shared_fname).bak |
| cft.uconf.instance_fname | (fname) | $(cft.runtime_dir)/data/cftuconf.ini |
| cft.uconf.instance_fname_bak | (fname) | $(cft.uconf.instance_fname)_bak |
| cft.uconf.runtime_fname  | (fname)  |   |
| cft.uconf.lock_fname | (fname) | $(cft.runtime_dir)/run/uconf.lck |
| cft.uconf.lock_delay_ms | (int) | 100 |
| cft.uconf.lock_retry |  (int) | 20 |
| cft.idparm  | (identifier) | IDPARM0 New |
| cft.cftcat.fname | (fname) | $(cft.runtime_dir)/data/cftcata Win:$CFTCATA(cft.ini) / Unix: _CFTCATA |
| cft.cftcat.default_size  | (int) | 10000 |
| cft.cftparm.fname | (fname) | $(cft.runtime_dir)/data/cftparm Win:$CFTPARM(cft.ini) / Unix: _CFTPARM |
| cft.cftparm.partfname | (fname) | $(cft.runtime_dir)/data/cftpart Win:$CFTPART(cft.ini) / Unix: _CFTPART |
| cft.cftcom.fname | fname | $(cft.runtime_dir)/data/cftcom Win:$CFTCOM(cft.ini) / Unix: _CFTCOM |
| cft.cftcom.default_size | (int) | 1000 |
| cft.cftlog.fname  | fname |  $(cft.runtime_dir)/log/cftlog Win:$CFTLOG(cft.ini) / Unix: _CFTLOG |
| cft.cftlog.afname | fname | $(cft.runtime_dir)/log/cftalog Win:$CFTALOG(cft.ini) / Unix: _CFTLOG |
| cft.cftaccnt.fname  |  fname | $(cft.runtime_dir)/log/cftaccnt Win:$CFTACCNT(cft.ini) / Unix: _CFTACNT |
| cft.cftaccnt.afname  | fname | $(cft.runtime_dir)/accnt/cftaccnt Win:$CFTAACCN(cft.ini) / Unix: _CFTACNTA |
| cft.server.maxtrans |  (int) |   |
| cft.server.delayed_update |  (int) | 1 |


<span id="Trace"></span>

## Trace parameters


| ID  | Default  | Former value  |
| --- | --- | --- |
| trace.xtrace.default_fname  | (fname) | $(cft.runtime_dir)/traces/cft.trc New |
| trace.xtrace.default_level | (fname) | New parameter |
| trace.net.level | (int) | New parameter |
| trace.file.level | (int) | New parameter |


<span id="Transfer2"></span>

## {{< TransferCFT/axwayvariablesComponentShortName  >}} probe configuration

Use the following parameters to define {{< TransferCFT/axwayvariablesComponentShortName  >}} probes.


| ID  | Description  |
| --- | --- |
| cft.probes.history_size | Number of samples kept in the memory for each time class |
| cft.probes.time_classes | Sorted list of time classes in milliseconds |


<span id="Add"></span>

## Add a character set: transcoding


| ID  | Description  |
| --- | --- |
| cft.cft_charsets | Add a character set id to the existing list of charsets.<br/> The default charsets are:<br/> • CFT_UTF-8<br/> • CFT_UTF-16<br/> • CFT_UTF-16LE<br/> • CFT_UTF-16BE<br/> • CFT_UTF-32<br/> • CFT_UTF-32LE<br/> • CFT_UTF-32BE<br/> • CFT_UCS-2<br/> • CFT_UCS-2LE<br/> • CFT_UCS-2BE<br/> • CFT_BIG5<br/> • CFT_CP850<br/> • CFT_ISO8859-1<br/> • CFT_ISO8859-15<br/> • CFT_EBCDIC-FR |
| cft.charsets.value.CUSTOM_CHARSET_ID.iconv_map  | Customize the charset that you created. |


## Start log and catalog parameters


| Parameter  | Default  | Description  |
| --- | --- | --- |
| copilot.startup.catalog  | Yes  | Display/hide catalog at start up.  |
| copilot.startup.catalog.filter  | Errors  | Filter to use for the catalog display on start up.  |
| copilot.startup.log  | Yes  | Display/hide log at start up.  |
| copilot.startup.log.filter  | None  | Filter to use for the log display on start up.  |
| cft.cftlog.switch_on_stop  | No  | Dictates if the switch log occurs at {{< TransferCFT/axwayvariablesComponentShortName  >}} server shutdown  |


## Automatically expand the catalog


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.cftcat.auto_expand_percent  | 0  | This value indicates the factor increase, as a percentage, that the catalog will automatically expand.<br/> The value 0 disables the automatic expansion feature.<br/> <blockquote> **Note**<br/> Tip We recommend that you set this to a relatively high value, at least 50. When repeatedly expanded, the catalog's internal structure may become fragmented and, consequently, catalog access less efficient.<br/> </blockquote>  |
| cft.cftcat.auto_expand_max_size  | 1M  | The maximum number of records for the automatic catalog expansion option. |


See also [Automatic catalog expansion](../../admin_monitoring_intro/auto_expand_catalog).

## Parallel transfers


| Parameter  | Description  |
| --- | --- |
| uconf:cft.server.maxtrans  | Modifies the number of parallel transfers. See <a href="../../../c_intro_userinterfaces/command_summary/parameter_intro/trantask">trantask</a>.  |


## Retrieve subdirectories


| Parameter  | Description  |
| --- | --- |
| uconf:cft.dirdepth=Yes  | Enables retrieving subdirectories.  |


## Synchronous connections


| Parameter  | Description  |
| --- | --- |
| uconf: cft.server.cftcoms.max_connection  | Defines the number of connections for CFTCOMS.  |


## Purge

### Startup configuration

PURGE when starting {{< TransferCFT/axwayvariablesComponentShortName  >}} is now configurable, with the following options:


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.purge.enable_on_start  | Yes  | Condition if the purge must be run on {{< TransferCFT/axwayvariablesComponentShortName  >}} startup.  |
| cft.purge.background_on_start  | Yes  | Condition if the purge must be run on {{< TransferCFT/axwayvariablesComponentShortName  >}} startup.  |
| cft.purge.quantity  | 10  | Number of transfers to delete at once step (only for background).  |
| cft.purge.periodicity  | 0  | Amount of time between each automatic purge.  |


### Dynamically purge catalog

These parameters modify the amount of time to keep transfers in catalog before purging, without directly modifying the CFTCAT object. For each parameter, enter an integer for the amount of time, optionally followed by the letter D, H, or M to indicate day, hour, or minute respectively. See the example section [Purging the catalog](../../admin_commands_intro/purge_catalog) for details. If you enter the default value, the value defined in CFTCAT configuration is used.


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.purge.rx  | -1  | requester state eXecuted  |
| cft.purge.sx  | -1  | server state eXecuted  |
| cft.purge.rt  | -1  | requester state Terminated  |
| cft.purge.st  | -1  | server state Terminated  |
| cft.purge.rh  | -1  | requester sate Hold  |
| cft.purge.sh  | -1  | server state Hold  |


See also, [Purging the catalog](../../admin_commands_intro/purge_catalog), Transfer states and [LISTCAT.](../../../c_intro_userinterfaces/about_cftutil/monitoring_cftutil_intro/brief_catalog_listing)

## Customizable network sessions


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.server.max_session  | 0  | Use this setting to overwrite the default maximum number of network sessions.<br/> The default value 0 doubles the maxtrans value. |


<span id="Transfer"></span>

## Transfer requests

This parameter lets you use the SEND or RECV command without requiring an [IDF](../../../c_intro_userinterfaces/command_summary/parameter_intro/idf). This means that if you do not define a transfer file identifier, a default value (CFTPART IDF) is used. Exceptions:

- For a SEND command the IDFDEF is used if  available, before searching for the CFTPART value.
- For a RECV command with an asterisk "\*" the sender provides the IDF.


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.default_idf.enable  | Yes  | Possible values are:<br/> • Yes: Indicates that a default file identifier is used when the transfer request IDF is not defined in a CFTRECV or CFTSEND command.<br/> • No: Disables the default IDF functionality of no required IDF.If you execute a command without an IDF or using an IDF that does not exist, or using the default IDF defined in CFTPARM (DEFAULT), the commands fails. |


<span id="Statisti"></span>

## Accounting records


| Parameter  | Default  | Description  |
| --- | --- | --- |
| cft.accnt.enable_extended_byte_fields  | No  | For each completed transfer, {{< TransferCFT/axwayvariablesComponentShortName  >}} can record the number of characters in the file (FBYTE) and the number of characters sent over the line (NBYTE).<br/> Possible values are:<br/> • No: The FBYTE and NBYTE fields are filled.<br/> • Yes: The FBYTE_EXTENDED and NBYTE_EXTENDED fields are filled (length=15), and FBYTE and NBYTE are empty (either 0 or blank depending on the CFTACCNT LANG parameter setting). |


<span id="Compatib"></span>

## Compatibility


| Parameter  | Default value  | Description  |
| --- | --- | --- |
| Uconf:cft.listcat_compat  | No  | Defines the LISTCAT display:<br/> • Yes = Display using the former product format, which does not include the new columns. The format in LISTCAT is DTSA.<br/> • No= Display using the product version 3.0 and higher catalog format. The format in LISTCAT is DTSASPP. |
| Uconf:cft.state_compat  | No  | Defines the transfer states:<br/> • Yes= The phase state is fully compatible with the states in versions prior to 3.0.<br/> • No = The state reflects the phase used in Transfer CFT 3.0 and higher. This uses phase instead of the former states, except during the Transfer phase, when the former state is the same as the phase step.<br/> <span >****Note****</span>: Uconf:cft.state_compat also impacts the <a href="../../../concepts/phase_and_phasestep/ack_phase">acknowledgement</a> behavior if ackstate is set to ignore. |


<span id="Deactivate_idf"></span>

## Deactivate the default IDF

You can deactivate the default use of the IDF value for the CFTSEND and CFTRECV commands.


| Parameter  | Default  | Type  | Description  |
| --- | --- | --- | --- |
| cft.default_idf.enable  | Yes  | Bool<br/> Yes / No | Enable the default IDF to use when the transfer request IDF is not explicitly defined by a CFTRECV or CFTSEND command.<br/> DIAGI=434 |

