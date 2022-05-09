{
    "title": "UCONF parameters for SecureRelay",
    "linkTitle": "UCONF parameters for Secure Relay",
    "weight": "260"
}While some of the Transfer CFT UCONF parameters for Secure Relay are quite technical, most have default values that should be suitable for common usage.

The MA and RA parameters are described in the following separate tables, and are all prefixed by ****secure_relay****.

<span id="_Toc362510690"></span>

Master agent parameters
-----------------------

Parameters that appear in Master agent configuration file are in bold.


| Parameter | Type | Default | Comment |
| --- | --- | --- | --- |
| secure_relay.enable | Bool | No | General flag to access Transfer CFT through Secure Relay if set to Yes. |
| secure_relay.ma.autostart | Bool | Yes | Allows an automatic start of the embedded Secure Relay Master Agent.  |
| secure_relay.ma.jar_fname | String | $(cft.install.xsr_dir)/xsrMaster.jar | Secure Relay Master Agent jar file.  |
| secure_relay.ma.pid_fname | String | $(cft.runtime.run_dir)/xsrMaster.pid | File containing the Secure Relay Master Agent Process ID.  |
| secure_relay.ma.start_timeout | Int | 30 sec | Amount of time, in seconds, in which Secure Relay can start before a timeout.  |
| secure_relay.ma.start_options | String | -Xmx512m -Xrs | Secure Relay Master Agent start options.  |
| secure_relay.ma.conf_fname | String | $(cft.runtime.run_dir)XsrConf.xml | Secure Relay Master Agent configuration file.  |
| secure_relay.ma. ca_cert_fname | String |   | Secure Relay certificate authority.<br/> This is a mandatory field, however certificates are not delivered with Transfer CFT. |
| secure_relay.ma.cert_fname | String |   | Secure Relay Master Agent user certificate.<br/> This is a mandatory field, however certificates are not delivered with Transfer CFT. |
| secure_relay.ma.cert_password_fname | String | $(cft.runtime.run_dir)/XsrPwd.dat | Secure Relay Master Agent certificate password file.  |
| secure_relay.ma.cert_password | String | test | Secure Relay Master Agent certificate password.  |
| secure_relay.ma.host | String | 127.0.0.1 | Secure Relay Master Agent listening IP address or FQDN.  |
| secure_relay.ma.comm_port | Int | 6801 | Secure Relay Master Agent listening communication port.  |
| secure_relay.ma.log_level | Int | 0 | 0=NONE, 1=SHORT, 2=FULL, 3=DEBUG |
| secure_relay.ma.log_fname | String | $(cft.runtime.log_dir)/xsrMaster.log | Secure Relay Master Agent log file.  |
| secure_relay.ma.admin_outport_range | String | None | Secure Relay Master Agent admin outport range.  |
| secure_relay.ma.comm_outport_range | String | None | Secure Relay Master Agent comm outport range.  |


Router Agent parameters
-----------------------

As you can have several Router Agents working with a Master Agent, the UCONF Router Agent definitions are arrays. Note however that Transfer CFT supports only one Router Agent.

In the Secure Relay parameters table below:

- The letter N is used in parameter names.
- Parameters that appear in Master Agent configuration file are displayed in bold.
- For an array, use the notation format ****secure_relay.ra.N.parameter****, where N is between 0 and number of routers – 1.


| Parameter | Type | Default | Comment |
| --- | --- | --- | --- |
| secure_relay.ra.N.enable | Bool | Yes | Enables the Router agent.  |
| secure_relay.ra.N.dmz | String | DMZ0 | Logical name of the DMZ where the Router Agent is running, with a maximum of 32 characters.  |
| secure_relay.ra.N.host | String | None | Router Agent IP address or FQDN. |
| secure_relay.ra.N.admin_port | Int | 6810 | Router Agent administration port. |
| secure_relay.ra.N.comm_port | Int | 6811 | Router Agent communication port.<br/> This parameter is specific to each {{< TransferCFT/axwayvariablesComponentShortName  >}} using the Router Agent. If more than one {{< TransferCFT/axwayvariablesComponentShortName  >}} uses the same Router Agent, each {{< TransferCFT/axwayvariablesComponentShortName  >}} must have a unique value. |
| secure_relay.ra.N. nb_data_connections | Int | 5 | Number of data connections between the Master Agent and the Router Agent.  |
| secure_relay.ra.N. data_channel_ciphering | Bool | No | Activates data connections ciphering. |
| secure_relay.ra.N. outcall_network_interface | String | None | Address to bind for outgoing calls. |


### Define the Router Agent to use  

The `srdmz` parameter in the CFTPART command allows you to specify a dedicated DMZ for outgoing connections.

```
secure_relay.ra = n (number of Router Agents)
secure_relay.ra.<i>.dmz = DMZ<i> (for each Router Agent, i = 0 to n-1)
secure_relay.ra.<i>.host = @hostSR<i> (for each Router Agent, i = 0 to n-1)
 
CFTPART id=part,nspart=myself,srdmz=DMZ<i> (where i is the Router Agents to be used for the outgoing connection)
```
