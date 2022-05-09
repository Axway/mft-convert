{
    "title": "Registration overview",
    "linkTitle": "Registration overview",
    "weight": "160"
}This topic describes the {{< TransferCFT/axwayvariablesComponentShortName  >}} to {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} registration process.

There are several types of exchanges that occur between {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} and {{< TransferCFT/axwayvariablesComponentShortName  >}}. The first exchange is registration, which begins when Copilot initiates a request to connect with {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}}.

The registration is performed on a SSL connection using simple authentication. Further exchanges - the heartbeat, and certificate renewal - are performed on a SSL connection using mutual authentication.

Refer to the {{< TransferCFT/PrimaryCGorUM  >}} {{< TransferCFT/PrimaryCGversion  >}} User Guide for more information on registration processes, such as registration approval.

********Registration exchange overview (the same for either {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}})********

![Copilot submits registration and Central Governance sends certificates by way of response.](/Images/TransferCFT/cft_registration.png)

<span id="Step"></span>

Step overview
-------------

This section describes the general steps that occurs during the registration process, and the impact on the configuration.

Starting Copilot after installation begins the connection and registration process with {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}}.

> **Note**
>
> Note: Transfer CFT requires the Central Governance or Flow Manager shared secret to register. See the Central Governance or Flow Manager documentation for details.

****1. Copilot connects to Central Governance or {{< TransferCFT/suitevariablesFlowManager  >}} and submits its registration.****

- Copilot sends a registration request through a simple authenticated SSL connection and submits its registration. Copilot authenticates the Central Governance or {{< TransferCFT/suitevariablesFlowManager  >}} server using the CA certificate pointing by the uconf:cg.ca_cert_id parameter. The registration request contains:

    -   Information about the {{< TransferCFT/axwayvariablesComponentShortName  >}} instance, including its instance name, host, port and version.
    -   Two Certificate Signing Requests (CSRs) for {{< TransferCFT/PrimaryCGorUM  >}} to process.

    > **Note**
    >
    > Note: If you use an intermediate certificate as a governance CA certificate, you must add the root CA certificate that signs this intermediate certificate in the Transfer CFT PKI database.

****2. {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} sends the SSL certificates to {{< TransferCFT/axwayvariablesComponentShortName  >}}.****

Central Governance or {{< TransferCFT/suitevariablesFlowManager  >}} processes the CSRs and returns two SSL certificates, one dedicated to governance exchanges and the other one dedicated to business exchanges (used for securing file transfers between the registering {{< TransferCFT/axwayvariablesComponentShortName  >}} and all other {{< TransferCFT/axwayvariablesSolutionShortName  >}}s).

Both certificates are stored in the internal PKI base using the following identifiers:

- &lt;uconf:cft.instance_id&gt;_GOV for the governance certificate
- &lt;uconf:cft.instance_id&gt; for the business certificate

****3. Copilot sends the first heartbeat over a mutual authenticated SSL connection.****

****4. The Transfer CFT configuration is updated and returned to Transfer CFT.****

During the registration process {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} receives the current configuration of {{< TransferCFT/axwayvariablesComponentShortName  >}} and changes it accordingly to their own rules.

Registration completes with Transfer CFT appearing in the {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} product list with the status of "Started" or "Stopped".

Configuration updates
---------------------

During the registration process Central Governance or {{< TransferCFT/suitevariablesFlowManager  >}} receives the original Transfer CFT configuration and updates it so that Transfer CFT is configured to:

- Connect to {{< TransferCFT/PrimaryCGorUM  >}} using the Central Governance mutual authentication port
- Use Central Governance for access management
- Use Central Governance for transfer monitoring
- Use its own internal PKI

These changes create two security profiles (CFTSSL) for Transfer CFT, one client and one server, named SSL_DEFAULT.

Completing the registration process, {{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} gets the current configuration of {{< TransferCFT/axwayvariablesComponentShortName  >}} and changes it.

- {{< TransferCFT/axwayvariablesComponentShortName  >}} is configured to use the {{< TransferCFT/PrimaryCGorUM  >}} {{< TransferCFT/suitevariablesPassPortName  >}} service for access management.
- {{< TransferCFT/axwayvariablesComponentShortName  >}} is configured to use the {{< TransferCFT/PrimaryCGorUM  >}} {{< TransferCFT/PrimarySentinel  >}} for transfer monitoring. For Transfer CFT 3.2.2 and higher, a secured connection is used.

These changes create two security profiles (CFTSSL) on the {{< TransferCFT/axwayvariablesComponentShortName  >}} (except when using a SAF-based PKI {{< TransferCFT/axwayvariablesComponentShortName  >}}). The profiles are named SSL_DEFAULT; one profile is of type client and one is of type server. Their SSL version is TLSV1COMP. The configured cipher list is CIPHLIST= ('53','47') for {{< TransferCFT/axwayvariablesComponentShortName  >}} 3.1.2 and 3.1.3. The values represent the following cipher suites:

The values represent the following cipher suites:

- 47: TLS_RSA_WITH_AES_128_CBC_SHA
- 53: TLS_RSA_WITH_AES_256_CBC_SHA

For Transfer CFT 3.2.2 and higher, the configured cipher list is CIPHLIST= ('49200', '49199', '49192', '49191', '157', '156', '61', '60', '53', '47'). The values represent the following cipher suites:

- 49200: TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- 49199: TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- 49192: TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- 49191: TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- 157: TLS_RSA_WITH_AES_256_GCM_SHA384
- 156: TLS_RSA_WITH_AES_128_GCM_SHA256
- 61: TLS_RSA_WITH_AES_256_CBC_SHA256
- 60: TLS_RSA_WITH_AES_128_CBC_SHA256
- 53: TLS_RSA_WITH_AES_256_CBC_SHA
- 47: TLS_RSA_WITH_AES_128_CBC_SHA

The client and server security profiles must be mutually authenticated. However, by default, a registered {{< TransferCFT/axwayvariablesComponentShortName  >}} does not have a protocol with a security profile. You must edit the {{< TransferCFT/axwayvariablesComponentShortName  >}} configuration to support protocols with mutual authentication.

{{< TransferCFT/PrimaryCGorUM  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} sends the updated configuration to {{< TransferCFT/axwayvariablesComponentShortName  >}}. The following are the {{< TransferCFT/axwayvariablesComponentShortName  >}} parameters updated in this process. However, you can overwrite certain default values by assigning an existing policy at registration. Please see, below for details.


| Parameter  | Value  |
| --- | --- |
| am.passport.cg.organization  | Org<br/> <blockquote> **Note**<br/> Note: You must restart both Transfer CFT and Copilot following a change to this parameter.<br/> </blockquote>  |
| am.passport.domain  | CG |
| am.passport.hostname  | &lt;{{< TransferCFT/suitevariablesFlowManager  >}} or {{< TransferCFT/PrimaryCGorUM  >}} host name &gt;  |
| am.passport.instance_id  | $(cft.instance_group).$(cft.instance_id)  |
| am.passport.port  | 6666  |
| am.passport.use_ssl  | Yes  |
| am.passport.userctrl.check_permissions_on_transfer_execution  | No  |
| am.type  | passport  |
| cft.purge.rt  | 10D  |
| cft.purge.rx  | 10D  |
| cft.purge.st  | 10D  |
| cft.purge.sx  | 10D  |
| cft.server.bandwitdth.cos  | 4  |
| cft.server.bandwitdth.cos.0.max_rate_in  | -1  |
| cft.server.bandwitdth.cos.0.max_rate_out  | -1  |
| cft.server.bandwitdth.cos.1.weight_in  | 80  |
| cft.server.bandwitdth.cos.1.weight_out  | 80  |
| cft.server.bandwitdth.cos.2.weight_in  | 15  |
| cft.server.bandwitdth.cos.2.weight_out  | 15  |
| cft.server.bandwitdth.cos.3.weight_in  | 5  |
| cft.server.bandwitdth.cos.3.weight_out  | 5  |
| cft.server.bandwitdth.enable  | No  |
| cg.mutual_auth_port  | &lt;secured communications port&gt;  |
| copilot.misc.createprocessasuser  | No  |
| pki.type  | cft<br/> <blockquote> **Note**<br/> Note: Except when using a SAF-based PKI Transfer CFT, in which case &quot;system&quot; is the value.<br/> </blockquote>  |
| sentinel.trkipaddr  | &lt;Sentinel – Front-end host &gt;  |
| sentinel.trkipport  | &lt;Sentinel - Font-end port&gt;  |
| sentinel.xfb.enable  | Yes  |
| sentinel.xfb.use_ssl  | Yes  |


### {{< TransferCFT/axwayvariablesComponentShortName  >}} instance id

When you register {{< TransferCFT/axwayvariablesComponentShortName  >}} with {{< TransferCFT/suitevariablesCentralGovernanceName  >}} or {{< TransferCFT/suitevariablesFlowManager  >}}, {{< TransferCFT/suitevariablesCentralGovernanceName  >}} or {{< TransferCFT/suitevariablesFlowManager  >}} sets the CFTPARM PART parameter to the same value as the {{< TransferCFT/suitevariablesTransferCFTName  >}} installation instance ID. Having a different value may impact transfer monitoring.

### Network definitions

****Windows and Linux****

For Transfer CFT to register successfully with Flow Manager, it must have at least one of the following Flow Manager network types defined, but cannot have more than one of each type: TCP, pTCP, and UDT. This means that you can register a Transfer CFT that has a maximum of three CFTNET objects, where the Transfer CFT type=TCP and CLASS is different for each CFTNET.

The CFTNET objects are:

- One pTCP, where ID = &lt;pTCP_ID&gt; and CLASS=&lt;X&gt; and the UCONF parameter acceleration.ptcp = &lt;pTCP_ID&gt;
- One  UDT, where ID = &lt;UDT_ID&gt; and CLASS=&lt;Y&gt; and the UCONF parameter acceleration.udt = &lt;UDT_ID&gt;
- One TCP, where ID=&lt;TCP_ID&gt; and CLASS=&lt;Z&gt;

Flow Manager does not manage additional CFTNET types; these are stored, without modification, on the Transfer CFT as retrieved on registration.

### Communication profiles

If the Transfer CFT that is registering has existing network definitions, for example a server type TCP_SSL definition, Flow Manager creates the appropriate corresponding communication profile on registration.

Flow Manager can create Transfer CFT flows that are configured with the PeSIT E  protocol. When a Transfer CFT registers with Flow Manager, for each CFTPROT where TYPE=PESIT and PROF=ANY,  Flow Manager recognizes communication profiles created with this protocol.
