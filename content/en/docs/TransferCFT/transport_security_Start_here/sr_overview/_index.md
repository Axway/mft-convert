{
    "title": "About Secure Relay",
    "linkTitle": "Implement DMZ using Secure Relay",
    "weight": "210"
}This section describes the {{< TransferCFT/axwayvariablesCompanyName  >}} Secure Relay integration with Transfer CFT. Secure Relay provides Transfer CFT with two main data security features. First, it enables a firewall-friendly access to Transfer CFT via the Secure Relay Master Agent (MA) and the Secure Relay Router Agent (RA). The second feature is SSL termination. This allows for secure SSL access to Transfer CFT without Transfer CFT needing to perform SSL or PKI access procedures. You can use Secure Relay with Transfer CFT over any supported Transfer CFT protocol, for example PeSIT, OFTP, SFTP, and so on.

The following additional topics describe how to configure Transfer CFT to use Secure Relay for exchanges.

- [Secure Relay in a standalone architecture](cft_sr_configuration)
- [Secure Relay in a multi-node architecture](cft_sr_conf_multinode)
- [Configuring SSL termination for exchanges](sr_ssl)
- [UCONF parameters for Secure Relay](sr_parameters)
- [Troubleshoot Secure Relay](sr_troubleshooting)

> **Note**
>
> Note: Secure Relay was formerly called XSR, and some documentation may still use this reference.

Overview
--------

When you set a Transfer CFT network resource to use Secure Relay, all of the connections that use this network resource transmit through Secure Relay for both incoming and outgoing connections.

The following diagram illustrates how Transfer CFT and Secure Relay interact with each other, as well as with the network.

********Network to DMZ overview********

![View of link between Transfer CFT and the Master Agent in the private network, with the Router Agent in the DMZ](/Images/TransferCFT/sr_new4.png)

****Legend****

Data connections are a pool of multiplexed connections between the RA and the MA. There are 5 connections by default, which can be either clear text or SSL-ciphered connections, depending on the configuration.

All connections between the Master Agent (MA) and Router Agents (RA) are initiated by the MA, and are allowed by firewall rules; this refers to all connections coming from the intranet toward the DMZ. When a connection comes from outside (the Internet), the RA alerts the MA using the hot channel (HC or administration channel), and the MA creates a new data connection to handle the incoming data.

When using Secure Relay, all service access points that are normally set in Transfer CFT (in the CFTTCPS.exe process) are exported on the external router side to the DMZ (the network interface accessible from outside).

Secure Relay is not aware of data sent to, or received from, Transfer CFT and remote partners. This means that data are not checked or modified, and Secure Relay is oblivious of the underlying protocol. The only possible transformation done by Secure Relay is the encapsulation of clear text data into SSL packets.

Secure data in high availability with multiple Router Agents
------------------------------------------------------------

You can install {{< TransferCFT/axwayvariablesComponentLongName  >}} in an active/active architecture where you add multiple SecureRelay Router Agents behind a load balancer. The architecture could resemble the following diagram (SSL is not used in this example). See [Secure Relay with a multi-node architecture](cft_sr_conf_multinode) for details on setting up multiple Router Agents.

![](/Images/TransferCFT/sec_relay_multi_RA.png)

![](/Images/TransferCFT/sr_add_node.png)

Prerequisites
-------------

- The {{< TransferCFT/axwayvariablesComponentShortName  >}} license key must include the Secure Relay option.
- Ensure that you have Java JRE 8 installed (Java 1.8.0 u272 or higher).
- Check that the Master Agent and Router Agent are using the same version of Java.
- Prior to setting up Secure Relay to {{< TransferCFT/axwayvariablesComponentShortName  >}} interoperability, you should already have installed Secure Relay 2.7.4. Refer to the Secure Relay documentation available at {{< TransferCFT/axwayvariablesCompanyName  >}} Support at [https://support.axway.com](https://support.axway.com/) and on the Axway [documentation portal](https://docs.axway.com/).

Certificates
------------

If you change the Master Agent certificates, please remember to delete or rename the file referenced by the UCONF `secure_relay.ma.cert_password_fname parameter` (XsrPwd.dat, by default) before starting {{< TransferCFT/axwayvariablesComponentLongName  >}}.

Version compatibility
---------------------

Transfer CFT 3.6 and higher delivers an embedded Secure Relay MA 2.7.4. You must then additionally install the Secure Relay RA 2.7.4.

Limitations
-----------

- SecureRelay is not operational with the {{< TransferCFT/axwayvariablesComponentShortName  >}} acceleration option.
- Secure Relay only supports TLSV1COMP as the SSL version when using Router Agent SSL termination.
- When using Secure Relay with Transfer CFT for SFTP exchanges, SSH termination is not supported.
- Transfer CFT cannot perform exchanges using Router Agent SSL termination if the Secure Relay FIPS mode is enabled. However, you can perform end-to-end SSL exchanges with the FIPS mode enabled as described in [Configure exchanges that use SSL](sr_ssl) &gt; *How to enable Secure Relay FIPS mode*.
