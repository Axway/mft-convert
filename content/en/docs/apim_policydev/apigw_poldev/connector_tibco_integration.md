{
"title": "Integrate with TIBCO Rendezvous",
"linkTitle": "Integrate with TIBCO",
"weight": 60,
"date": "2019-10-17",
"description": "Configure API Gateway to produce or consume messages for TIBCO Rendezvous."
}

API Gateway ships with in-built support for TIBCO Rendezvous. TIBCO Rendezvous is a low latency messaging product for real-time high throughput data distribution applications. A message can be sent from the TIBCO daemon running on the local machine to a single TIBCO daemon running on a separate host machine or it can be broadcast to several daemons running on multiple machines. Each message has a subject associated with it, which acts as the *destination* of the message.

A listener, which is itself a TIBCO daemon, can declare an interest in a subject on a specific daemon. Whenever a message is delivered to this subject on the daemon the message is delivered to the listening daemon.

The API Gateway can act as a listener on a specific subject at a TIBCO daemon, in which case it said to be acting as a *consumer* of TIBCO messages. Similarly, it can also send messages to a TIBCO daemon, effectively acting as a *producer*
of messages. In both cases, the local TIBCO daemon must be configured to talk to the TIBCO daemons running on the remote machines.

## TIBCO Rendezvous integration

The API Gateway can act as a producer and a consumer of TIBCO Rendezvous messages. In both cases, a Rendezvous daemon must be configured. This is responsible for communicating with other Rendezvous programs on the network.

### Produce TIBCO Rendezvous Messages

Perform the following steps to produce messages and send them to another Rendezvous program:

1. Configure a Rendezvous daemon. For more information, see [Configure a TIBCO Rendezvous daemon](#configure-a-tibco-rendezvous-daemon).
2. Use a **TIBCO Rendezvous** filter to route messages to TIBCO Rendezvous.

### Consume TIBCO Rendezvous Messages

A TIBCO Rendezvous listener can be configured at the API Gateway instance level to consume Rendezvous messages. Perform the following steps to consume Rendezvous messages:

1. Configure a Rendezvous daemon. For more information, see [Configure a TIBCO Rendezvous daemon](#configure-a-tibco-rendezvous-daemon).
2. Configure a Rendezvous listener. For more information, see [Configure a TIBCO Rendezvous listener](#configure-a-tibco-rendezvous-listener).

## Configure a TIBCO Rendezvous daemon

This section describes how to configure a TIBCO Rendezvous daemon. For a more detailed description of how to configure the fields on this dialog, refer to your TIBCO Rendezvous documentation.

You can configure TIBCO Rendezvous daemons under the **Environment Configuration** > **External Connections**
tree node in the Policy Studio. Right-click the **TIBCO Rendezvous Daemons**
node, and select **Add a TIBCO Rendezvous Daemon**. Configure the following fields on the **TIBCO Daemon Settings**
dialog:

**Name**:
Enter a friendly name for this TIBCO Rendezvous daemon. When configured, this name is available for selection when configuring a **TIBCO Rendezvous Listener**
and a **TIBCO Rendezvous**
filter.

**Service**:
Communication between TIBCO Rendezvous daemons takes place using Pragmatic General Multicast (PGM) or Universal Datagram Protocol (UDP) services. The specified *service parameter*
configures the local TIBCO Rendezvous daemon to use this type of service when sending or broadcasting messages to other TIBCO Rendezvous daemons who are also using this service.

You can specify the service in the following ways:

* **By Service Name**:
    If your network administrator has added an entry for TIBCO Rendezvous in a network database such as NIS (for example, `rendezvous 7500/udp`), you can enter the name of the service (for example, `rendezvous`) in this field.
* **By Port Number**:
    Alternatively, you can enter the port number on which the TIBCO Rendezvous daemon is listening (for example, `7500`).
* **Default Option**:
    If you leave this field blank, a default service name of `rendezvous`
    is assumed. For this reason, administrators should add an entry in the network database with this name (for example, `rendezvous 7500/udp`. This enables you to leave this field blank so that this default service is used.

**Network**:
If the machine on which the TIBCO Rendezvous daemon is running has more than one network interface, you can specify what interface to use for all communications with other daemons. Each TIBCO Rendezvous daemon can only communicate on a single network, meaning that separate daemons must be configured for each network you want the daemon to communicate on.

For simplicity, you can leave this field blank, in which case the primary network interface is used for communication with other daemons. For more information on how to configure different networks and multicast groups, see the TIBCO Rendezvous documentation.

**Daemon**:
The value entered here tells the API Gateway where it can find the TIBCO Rendezvous daemon, which is responsible for communicating with all other daemons on the network. This daemon can be local or remote.

For local daemons you need only specify the port number that the daemon is running on (for example `6500`). Alternatively, you can leave this field blank to connect to the daemon on the default port.

To connect to a remote daemon, you must specify both the host and port number of the daemon in this field (for example `daemon_host:6500`).

## Configure a TIBCO Rendezvous listener

You can configure a TIBCO Rendezvous Listener at the API Gateway instance level in the Policy Studio. To add a listener, perform the following steps:

1. In the Policy Studio tree, expand the **Environment Configuration** > **Listeners** node.
2. Right-click the API Gateway instance, and select **TIBCO** > **Rendezvous Listener** > **Add**.
3. Configure the following fields on the **TIBCO Rendezvous Listener** dialog:
    * **TIBCO Settings tab**: Enter the name of the subject that you want this consumer to listen for in the **Rendezvous Subject** field. Only messages addressed with this subject are consumed by the listener.

        Click the button next to the **TIBCO Rendezvous Daemon to use** field, and select a previously configured TIBCO Rendezvous Daemon to communicate with other TIBCO programs. To add a TIBCO Rendezvous Daemon, right-click the **TIBCO Rendezvous Daemons** tree node, and select **Add a TIBCO Rendezvous Daemon**. For more details, see see [Configure a TIBCO Rendezvous daemon](#configure-a-tibco-rendezvous-daemon).
    * **Policy to Use**: When messages with the specified subject have been consumed they must be passed into a policy where they can be processed accordingly. Select the policy that you want to use to process consumed messages from the tree.
