---
title: "Configuring PassPort AM "
linkTitle: "Configuring PassPort AM"
weight: 190
--- This section describes how to configure access management when not using {{< TransferCFT/PrimaryCGorUM  >}}.

<span id="Procedure PassPort parameters"></span>

## Procedure

To configure the PassPort AM connection, set the UCONF parameters described
in this section. From
the ****Administration**** screen in the
graphical user interface, access the *Unified Configuration* window. Double- click in the **Unified
Configuration** window to begin editing parameters.

1. Define the connection to the PassPort AM server using the UCONF parameters in the following tables. You must define the parameters in the order listed.

****PassPort AM connector parameters****

| Parameter  | Description  |
| --- | --- |
| <div > am.passport.hostname </div>  | <div > PassPort AM server hostname/IP address. </div>  |
| <div > am.passport.port </div>  | <div > PassPort AM server port. </div>  |
| am.passport.srchost  | The PassPort AM local network interface for outgoing connections. |
| <div > am.passport.instance_id </div>  | Transfer CFT instance ID for PassPort AM <br/> • You must determine your Transfer CFT's PassPort instance name. If it does not match the instance name of **default**, you must add an instance with the correct name.<br/> • The passport.instance_id corresponds to the instance (An instance name is a unique identifier of the installed instance of the component. Check that PassPort and your Transfer CFT have the SAME instance name) of the CSD that is available in PassPort.<br/> • In PassPort, you can view this by selecting **Access** &gt; **Components** &gt; **Transfer CFT** and checking the screen display (default value: default (when you import a component, PassPort assigns it the instance name **default** )). |
| <div > am.passport.login </div>  | <div > Transfer CFT login for PassPort AM. This user must exist in PassPort and have an Administrator role. This user represents an application user with which Transfer CFT makes requests. </div>  |
| <div > am.passport.password </div>  | <div > Transfer CFT Instance ID Password for PassPort AM, see above. </div>  |
| <div > am.passport.superuser </div>  | Enables users to perform any type of action without PassPort AM permission checks.<br/> You *must* set up at least one superuser. Doing so enables you to deactivate or change the PassPort AM connector configuration if the server is not responding.<br/> If the user's name for a session contains a space in the name, you must insert the backslash **\** character where the space occurs.<br/> ********Example********<br/> If you are defining the users "firstname lastname" and "johndoe" as superusers, you would set the am.passport.superuser parameter value to "firstname**\** lastname johndoe".<br/> So for this example, the command is:<br/> **<code>CFTUTIL uconfset id = am.passport.superuser, value = "'firstname\ lastname johndoe'"</code>** |
| am.passport.use_ssl  | Enables SSL with PassPort AM.<br/> The server port is *not* the same as the default port when using SSL. |
| am.passport.ca_cert  | Certification Authority (CA) public certificate to authenticate the PassPort AM server.  |
| am.passport.csd_file  | Transfer CFT Component Security descriptor file for PassPort AM. The default value is $(cft.install_dir)/extras/PassPort/csd_Transfer_CFT.xml.  |

1. Set the access management type parameter to PassPort: am.type = passport

> **Note**
>
> The am.type is the last parameter to set when activating PassPort AM and the first to unset when deactivating it.

1. Restart the Transfer CFT and Copilot servers.

## Example PassPort AM configuration with SSL 

1. Configure the Access Management type:

`CFTUTIL UCONFSET ID=am.type, VALUE=none`

1. Configure the connection using your CFTPARM PART name as the instance_id value.

`CFTUTIL UCONFSET ID=am.passport.hostname, VALUE=pam.company.com`

`CFTUTIL UCONFSET ID=am.passport.port, VALUE=6666CFTUTIL UCONFSET ID=am.passport.use_ssl, VALUE=YES`

`CFTUTIL UCONFSET ID=am.passport.ca_cert, VALUE=conf/pki/<your_passport_CA>`

`CFTUTIL UCONFSET ID=am.passport.instance_id, VALUE=CFT23412CFTUTIL UCONFSET ID=am.passport.login, VALUE=PASSPORT_ADMIN_USER`

`CFTUTIL UCONFSET ID=am.passport.password, VALUE=PASSPORT_ADMIN_PASSWORD`

1. Activate the PassPort Access Management:

` CFTUTIL UCONFSET ID=am.type, VALUE=passport`

## Optional PassPort AM

| Parameter  | Definition  |
| --- | --- |
| am.passport.userctrl.check_permissions_on_transfer_execution  | <span id="Check"></span>Check the permissions for the execute action on the transfer resource when the {{< TransferCFT/axwayvariablesComponentShortName  >}} user control is enabled ([USERCTRL](../../../c_intro_userinterfaces/command_summary/parameter_intro/userctrl)=YES). To disable the permission check, set the following parameter to No. The default is Yes. |
| am.passport.domain  | PassPort AM domain.  |
| am.passport.max_connections  | Maximum number of connections with PassPort.  |
| am.passport.pipe_priority  | Pipelining priority mode.  |
| am.passport.pipeline_size  | Maximum number of requests in the pipe for one PassPort.  |
| am.passport.resource_prefix  | Only EXPERTS may use the resource prefix.  |

## References

A complete set of PassPort documentation is available at [support.axway.com](http://support.axway.com/).

For more information about starting the Transfer CFT UI (Copilot), refer to Starting and Stopping the Copilot UI.

****Related topics****

[About PassPort AM](../)

[PassPort AM CSD](../passport_am_csd)

[Defining user rights Windows](../../../cft_intro_install/windows_install_start_here/windows_install_start_here/running_cft_for_the_first_time_windows/user_rights_and_interface_win)

[Defining user rights Unix](../../../cft_intro_install/unix_install_start_here/run_first_time_ux/run_first_time_ux/user_rights_and_interface_unix)
