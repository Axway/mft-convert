{
    "title": "SecureRelay with a standalone architecture",
    "linkTitle": "Secure Relay with a standalone architecture",
    "weight": "230"
}This page describes how to configure Transfer CFT in a standalone architecture for use with Secure Relay.

Step overview:

- Install 1 or more Secure Relay Router Agents
- Configure Secure Relay
- Enable Secure Relay and configure Java
- Configure additional Transfer CFT objects
    -   Network object CFTNET
    -   Protocol object CFTPROT
    -   Partner object CFTPART and CFTTCP

Secure Relay Router Agent installation
--------------------------------------

Follow the installation instructions provided in the [Secure Relay RA Installation Guide](https://docs.axway.com/bundle/SecureRelay_271_InstallationGuide_allOS_en_PDF/resource/SecureRelayRA_InstallationGuide_allOS_en_PDF.pdf). During installation, it is essential that you configure the Router Agent CA and user certificate as follows to enable secure communication between the Master Agent and Router Agent:

- `<CACertificate>CA_for_RA.der</CACertificate>`
- `<UserCertificate>USER_for_RA.p12</UserCertificate>`

You need these values when you configure the Master Agent in the {{< TransferCFT/axwayvariablesComponentLongName  >}} configuration, where the user certificate that you use must be signed by `CA_for_RA`. You should use the same CA and USER certificate as for the Master Agent.

Configure the Router Agents in {{< TransferCFT/axwayvariablesComponentLongName  >}}
----------------------------------------------------------------------------------------

After completing installation, configure the Router Agents in the {{< TransferCFT/axwayvariablesComponentLongName  >}} configuration.

1. Set the value for the number of Router Agents using the `secure_relay.ra` parameter. {{< TransferCFT/axwayvariablesComponentLongName  >}} generates a set of `secure_relay.ra.n.*` parameters, where the number, *n*, corresponds to the number of Router Agents you defined in this parameter.
1. You can use the default values for most fields, but you must customize the` secure_relay.ra.0.dmz` parameter. This value must be unique; for example, you can increment the DMZ0 value by one for each Router Agent so that the  second Router Agent has the value` secure_relay.ra.0.dmz = DMZ1`.
1. Configure the host address for each Secure Relay host using `secure_relay.ra.0.host`.

****Example of two Router Agent definitions****

```
secure_relay.ra = 2
 
secure_relay.ra.0.enable = yes
secure_relay.ra.0.dmz = DMZ0
secure_relay.ra.0.host = @hostF
secure_relay.ra.0.admin_port = 6810
secure_relay.ra.0.comm_port = 6811
secure_relay.ra.0.nb_data_connections = 5
secure_relay.ra.0.data_channel_ciphering = No
secure_relay.ra.0.outcall_network_interface =
 
secure_relay.ra.1.enable = Yes
secure_relay.ra.1.dmz = DMZ1
secure_relay.ra.1.host = @hostG
secure_relay.ra.1.admin_port = 6810
secure_relay.ra.1.comm_port = 6811
secure_relay.ra.1.nb_data_connections = 5
secure_relay.ra.1.data_channel_ciphering = No
secure_relay.ra.1.outcall_network_interface =
```

Configure the Master Agent in {{< TransferCFT/axwayvariablesComponentLongName  >}}
---------------------------------------------------------------------------------------

Configure the following UCONF parameters to enable the Master Agent communication with the Router Agent:

- `secure_relay.ma.ca_cert_fname = <must be the same as the CA_for_RA.der value>`
- `secure_relay.ma.cert_fname = <must be a P12 user certificate that is signed by the CA_for_RA.der>`
- `secure_relay.ma.cert_password = <user certificate password>`

Enable Secure Relay and configure the Java
------------------------------------------

In {{< TransferCFT/axwayvariablesComponentLongName  >}} from the CFTUTIL prompt, perform the following commands:

1. Enable Secure Relay:  
    ```
    UCONFSET id=secure_relay.enable ,value=yes
    ```
1. Set the full path to Java executable, for example:  
1. ```
    UCONFSET id=cft.jre.java_binary_path ,value=/bin/java
    ```

Configure {{< TransferCFT/axwayvariablesComponentLongName  >}} objects
---------------------------------------------------------------------------

### Createa CFTNET object

1. Create a CFTNET object where:
    -   TYPE= TCP
    -   protocol value = SR
1. Define the mandatory parameters RECALLHOST and SSLTERM.
    -   RECALLHOST: The host address on which the Master Agent calls Transfer CFT when Secure Relay receives an incoming call. If Transfer CFT and the Master Agent run of the same host, use the loopback network interface (for example, 127.0.0.1) instead of the public network interface. When using Secure Relay, the HOST parameter designates the network interface that is used on the Router Agent side.
    -   SSLTERM: Set this Boolean to YES to enable SSL termination.
1. If there is existing CFTNET object(s), the class parameter must be different.

****Example****

```
CFTNET ID = NETSR,
PROTOCOL = SR,
TYPE = TCP,
CALL = INOUT,
CLASS = 1,
HOST = <e.g. INADDR_ANY, network_interface_used_by_Router_Agent>,
RECALLHOST = 127.0.0.1, /\*network_interface_used_by_Transfer_CFT\*/
```

Create a CFTPROT object
-----------------------

This section describes the CFTPROT object, and how various parameters are related to enabling secure data transmission using Secure Relay.

- CFTPROT is related to the CFTNET object through the NET parameter.
- The SAP parameter is the listening port that is used on the RA side (using the CFTNET HOST parameter as the network interface).

****Example****

This example uses a CFTNET object called NETSR.

```
CFTPROT id = PESITANY,
net = NETSR,
sap = 1761,
Prot=PESIT,
prof = ANY
```

Create CFTPART and CFTTCP objects
---------------------------------

When a partner object refers to a CFTPROT object and a CFTNET object that use Secure Relay, it uses Secure Relay for both incoming and outgoing connections.

So to complete the configuration, create a CFTPART and a CFTTCP. In this way, the CFTPART refers to the CFTPROT object, and that in turn refers to a CFTNET, which points to Secure Relay.

****Example****

This is an example of the CFTPART and CFTTCP object configuration, using PESITANY.

```
CFTPART id = PARIS,
 prot = PESITANY,
 sap = <remote_partner_sap>,
 nspart = NPHOENIX,
  nrpart = NPARIS,
  mode = replace
 
CFTTCP id = PARIS,
 class = 1, /\* same class as the one used in the CFTNET \*/
 host = <remote_partner_host_address>,
 mode = replace
```

### Indicate a specific SecureRelay to use

If you would like to use a specific SecureRelay with a given partner, set the following parameter in the CFTPART:

```
SRDMZ = <UCONF secure_relay.ra.*n*.dmz value, where n is the number that corresponds to the SecureRelay to use>
```
