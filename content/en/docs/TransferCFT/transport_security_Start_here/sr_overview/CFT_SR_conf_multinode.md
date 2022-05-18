---
title: " Secure Relay with a multi- node architecture"
linkTitle: "Secure Relay with a multi- node architecture"
weight: 240
--- You can install {{< TransferCFT/axwayvariablesComponentLongName  >}} in an active/active architecture where you add multiple SecureRelay Router Agents behind a load balancer. The architecture could resemble the diagrams in the [Example architectures](#Examples) section of this page.

This page describes how to configure Transfer CFT in a multi- node architecture to use Secure Relays:

- Install {{< TransferCFT/axwayvariablesComponentLongName >}} in a multi- node, multi- host architecture
- Install 2 or more SecureRelay Router Agents (use the same CA and USER certificate as the Master Agent)
- Enable Secure Relay and configure Java
- Set the listening ports
- Configure additional {{< TransferCFT/axwayvariablesComponentLongName >}} objects
    - Network object CFTNET
    - Protocol object CFTPROT
    - Partner object CFTPART and CFTTCP
- Install and configure a load balancer (to use for incoming connections)
- Example architectures schema

## Prerequisites

Follow the server- side load balancer instructions to install and configure the load balancer.

## Secure Relay Router Agent installation

Follow the installation instructions provided in the [Secure Relay RA Installation Guide](https://docs.axway.com/bundle/SecureRelay_271_InstallationGuide_allOS_en_PDF/resource/SecureRelayRA_InstallationGuide_allOS_en_PDF.pdf). During installation, it is essential that you configure the Router Agent CA and user certificate as follows to enable secure communication between the Master Agent and Router Agent:

- `<CACertificate>CA_for_RA.der</CACertificate>`
- `<UserCertificate>USER_for_RA.p12</UserCertificate>`

You need these values when you configure the Master Agent in the {{< TransferCFT/axwayvariablesComponentLongName  >}} configuration, where the user certificate that you use must be signed by `CA_for_RA`. You should use the same CA and USER certificate as for the Master Agent.

## Configure the Router Agents in {{< TransferCFT/axwayvariablesComponentLongName  >}}

After completing installation, configure the Router Agents in the {{< TransferCFT/axwayvariablesComponentLongName  >}} configuration.

1. Set the value for the number of Router Agents using the `secure_relay.ra` parameter. {{< TransferCFT/axwayvariablesComponentLongName >}} generates a set of `secure_relay.ra.n.*` parameters, where the number, *n*, corresponds to the number of Router Agents you defined in this parameter.
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

## Configure the Master Agent in {{< TransferCFT/axwayvariablesComponentLongName  >}}

Configure the following UCONF parameters to enable the Master Agent communication with the Router Agent:

- `secure_relay.ma.ca_cert_fname = <must be the same as the CA_for_RA.der value>`
- `secure_relay.ma.cert_fname = <must be a P12 user certificate that is signed by the CA_for_RA.der>`
- `secure_relay.ma.cert_password = <user certificate password>`

## Enable Secure Relay and configure the Java

In {{< TransferCFT/axwayvariablesComponentLongName  >}} from the CFTUTIL prompt, perform the following commands:

1. Enable Secure Relay:  
    ```
    UCONFSET id=secure_relay.enable ,value=yes
    ```
1. Set the full path to Java executable, for example:  
1. ```
    UCONFSET id=cft.jre.java_binary_path ,value=/bin/java
    ```

## Set the listening ports

If you need to set listening ports, for example if you are using a firewall, proceed as follows:

1. To define the {{< TransferCFT/axwayvariablesComponentLongName >}} internal listening points for inter- node communication, select a port- range using the UCONF `cft.multi_node.listen_port_range` parameter. For example:  
    ```
    UCONFSET id=cft.multi_node.listen_port_range,value='33000- 33100'
    ```
1. Define the protocol listening ports for Secure Relay, which correspond to the SAP values in the CFTPROT object. For each node this value  increments by one port number. Therefore, when configuring the CFTPROT object ensure that there is no  listening port overlap.

****Example****

For example, if you set the SAP=1761 when you have 4 nodes, Secure Relay opens the ports 1761, 1762, 1763 and 1764.

```
CFTPROT ID=PROT0,SAP=1761,...
CFTPROT ID=PROT1,SAP=1764,...
```

In the scenario above if 4 nodes are configured in {{< TransferCFT/axwayvariablesComponentLongName  >}} multi- node, then Secure relay will open the listening ports 1761, 1762,1763, 1764 causing an issue for the second defined protocol, PROT1, as it has a SAP=1764.

Additionally, for each node the comm_port also increments. That is, the `secure_relay.ra.N.comm_port` parameter, where N is an index.

- N = 0: first RA
- N =1: second RA
- N = 2: third RA

However, when you define the `secure_relay.ra.N.admin_port  `value, where N is an index, the value does not increment according to the number of nodes.

****Example****

If a multi- node {{< TransferCFT/axwayvariablesComponentLongName  >}} has 4 nodes and the setting `secure_relay.ra.N.comm_port= 6811`, {{< TransferCFT/axwayvariablesComponentLongName  >}} opens the ports 6811, 6812, 6813, and 6814.

## Define the environment for SecureRelay

### Create a CFTNET object

1. Create a CFTNET object where:
    - TYPE = TCP
    - PROTOCOL = SR
1. Define the RECALLHOST, HOST, and SSLTERM parameters.
    - RECALLHOST (mandatory): The host address on which the Master Agent calls Transfer CFT when Secure Relay receives an incoming call. If Transfer CFT and the Master Agent run of the same host, use the loopback network interface (for example, 127.0.0.1) instead of the public network interface.
    - HOST: Designates the network interface that is used on the Router Agent side. We recommend setting this to INADDR_ANY.
    - SSLTERM: Set this Boolean to YES to enable SSL termination.
1. If there are existing CFTNET object(s), the class parameter must be different.

****Example****

As no connection dispatcher is used with Secure Relay, the SAP configured in the CFTPROT for SecureRelay is also incremented by the node number.

```
CFTNET ID = NETSR,
TYPE = TCP,
CALL = INOUT,
CALL = 'INOUT',
MAXCNX = '1000',
ORIGIN = 'CFTUTIL',
CLASS = '2',
HOST = 'INADDR_ANY',
SRCPORTS = ( '5000- 65535'),
PORT = '0',
PROTOCOL = 'SR',
RECALLHOST = '127.0.0.1',
SSLTERM = 'NO',
MODE = 'REPLACE'
```

### Create a CFTPROT object

This section describes the CFTPROT object, and how various parameters are related to enabling secure data transmission using Secure Relay.

- CFTPROT is related to the CFTNET object through the NET parameter.
- The SAP parameter is the listening port that is used on the RA side (using the CFTNET HOST parameter as the network interface).  
    This value automatically increments by one per node, where the range is &lt;number of nodes>- 1. Therefore, be certain that you do not use the ports in this range for another protocol.

****Example****

This example uses a CFTNET object called NETXSR, and PROTXSR is bound to port 1861 for node 0, and port 1862 for node 1.

```
CFTPROT ID = 'PROTXSR'
NET = 'NETXSR',
SAP = '1861', <Transfer CFT increments this for each node, be sure to check for port conflicts>
SRIN = 'BOTH',
SROUT = 'BOTH',
TYPE = 'PESIT',
....
```

### Create CFTPART and CFTTCP objects

When a partner object refers to a CFTPROT object and a CFTNET object that use Secure Relay, it uses Secure Relay for both incoming and outgoing connections.

So to complete the configuration, create a CFTPART and a CFTTCP. In this way, the CFTPART refers to the CFTPROT object, and that in turn refers to a CFTNET, which points to Secure Relay.

****Example****

This is an example of the CFTPART and CFTTCP object configuration.

```
CFTPART id = PARIS,
 prot = PROTXSR,
 sap = <remote_partner_sap>,
 nspart = NPARIS,
  nrpart = NPHOENIX,
  mode = replace

CFTTCP id = PARIS,
 class = 2, /\* this must match and be the same class as the one used in the CFTNET (SecureRelay)\*/
 host = <remote_partner_host_address>,
 mode = replace
```

### Indicate a specific SecureRelay to use

If you would like to use a specific SecureRelay with a given partner, set the following parameter in the CFTPART:

```
SRDMZ = <UCONF secure_relay.ra.*n*.dmz value, where n is the number that corresponds to the SecureRelay to use>
```

## Configure the load balancer

1. Set the listening port, 1555 for our example, for the load balancer (LB).
1. Set the back- end server address and port for each host (hostF and hostG).

In our example, we have load balancing with 2 Router Agents, 2 hosts, and 2 {{< TransferCFT/axwayvariablesComponentLongName  >}} nodes. You can see the port numbers increment by one for the second node (as many entries as number of RA x number of nodes).

listen cft LB:1555

<span id="Examples"></span>

## Example architectures

The following diagrams are examples of high availability with multiple Router Agents.

![](/Images/TransferCFT/sec_relay_multi_RA.png)

![](/Images/TransferCFT/sr_add_node.png)
