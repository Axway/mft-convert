{
    "title": "Customize the VIPA and execute commands",
    "linkTitle": "Customize the VIPA and execute commands",
    "weight": "190"
}This sections describes how to customize the LPAR (hosts) for your multi-node setup. It is based on an example implementation of an Active/Active High Availability multi-host configuration use case. For a mono host, multi-node installation, you only customize one LPAR (host).

After you complete the customization and configuration checks, execute the program commands.

Procedure overview
------------------

To customize the hosts in your setup and submit the customization, perform the tasks listed in the table.

"How to" instructions for configuring LPAR options and submitting the VARY OBEYFILE commands are provided in the sections following this table.


| Step  | Task  |
| --- | --- |
| 1  | If you have not done so, customize [MNINIT]().  |
| 2  | Customize the UPARM(TC*) members.<br/> The example (below) demonstrates how to customize LPAR1 and LPAR2. If you have additional machines in your configuration, repeat this step for each host machine. For a single host installation, you only customize UPARM(TCPSHAP1). |
| 3  | Perform a Transfer CFT configuration check:<br/> • Verify that the CFTNET object host parameter corresponds with what you defined for the VIPA.<br/> • Verify that the CFTNET command SRCPORT parameter is set to 1. |
| 4  | Submit the MNINIT. The Transfer CFT is now configured for multi-node.  |
|   | Steps 5 and 6, which use the VARY OBEYFILE commands, require administrator rights.  |
| 5  | Execute the VARY OBEYFILE command for each UPARM(TC*) member.  |
| 6  | For a z/OS restart, the administrator must do all of the VIPA OBEYFILE commands performed in Step 5 on the z/OS machine.  |


Customize the UPARM(TC\*) members
---------------------------------

The UPARM(TC\*) members customization example is based on a setup two hosts, LPAR1 and LPAR2. For a mono host, multi-node installation, you only need to customize the UPARM(TCPSHAP1).

For each host in your multi-node configuration, perform the following customization and execute. You can execute after each option, or after customizing all options.

### Customize the LPAR1

Define the shareport method.

..UPARM(TCPSHAP1) SHAREPORT / SHAREPORTWLM

Example VIPA command to execute:

```
v tcpip,tcpip,obeyfile,dsn=... UPARM(TCPSHAP1)
```

Customize the dynamic XCF address (automatic function to declare LPAR).

..UPARM(TCPDYMX1) DYNAMICXCF

Example VIPA command to execute:

```
v tcpip,tcpip,obeyfile,dsn=...UPARM(TCPDYMX1)
```

### Customize the LPAR2

Define the shareport method.

..UPARM(TCPSHAP1) SHAREPORT / SHAREPORTWLM

Example VIPA command to execute:

```
v tcpip,tcpip,obeyfile,dsn=...V30X.UPARM(TCPSHAP1)
```

Customize the dynamic XCF address (automatic function to declare LPAR).

..UPARM(TCPDYMX2) DYNAMICXCF (one for each host)

Example VIPA command to execute:

```
v tcpip,tcpip,obeyfile,dsn=...V30X.UPARM(TCPDYMX2)
```

Customize the cluster
---------------------

Define the cluster address after configuring the DYNAMICXCF on all hosts (that is, the customization of LPAR1 and LPAR2 in the previous section). This host address is the unique host address as seen externally, and should correspond with the one declared in the local CFTNET.

Remember that the VIPA commands must be executed by someone with administrator rights.

Customize the cluster as follows:

..UPARM(TCVIPDEF) VIPADEFINE (global for all hosts VIPADEFINE mask @DVIPA)

Example VIPA command to execute:

```
v tcpip,tcpip,obeyfile,dsn=...UPARM(TCVIPDEF)
```

This means that in the example configuration, the cluster will work with the machines (LPAR1, LPAR2,...) using this one single dynamic address.

### About Copilot

For each listening port configuration (for the SYSPLEX distributor incoming calls), you can have one dvipa address for Copilot, where there is one Copilot for each host.

> **Note**
>
> Note: There is no PORT statement for Copilot. An external Copilot passes via the SYSPLEX distributor directly to the host Copilot. There is one Copilot per host, regardless of the number of Transfer CFT nodes on the host.

Two command samples contain the VIPAdistribute Statement, which concerns the SYSPLEX distributor. There is also a command also for Copilot.

### About REST API

When using REST API (`copilot.restapi.enable=yes`) in a multi-node environment, you must also define the `copilot.restapi.serverport` value in the VIPA configuration. See, for example, the delivered UPARM(TCVIPRN5) and UPARM(TCVIPWG5) files.

Define a distribution method
----------------------------

The following examples, `WeightedActive `and `RounRobin`, represent two of the possible load distribution methods that you can use in VIPA. Select a load distribution method, and execute the corresponding VARY OBEYFILE  command once you have finished customizing this option.

**Weighted active example**

`              - DISTMethod WEIGHTEDActive`

..UPARM(TCVIPWG0) Port pesit

Example VIPA command to execute:

```
v tcpip,tcpip,obeyfile,dsn=...UPARM(TCVIPWG0)
```

..UPARM(TCVIPWG1) Port pesit ssl

```
v tcpip,tcpip,obeyfile,dsn=...UPARM(TCVIPWG1)
```

..UPARM(TCVIPWG2) Port Copilot

```
v tcpip,tcpip,obeyfile,dsn=….UPARM(TCVIPWG2)
```

..UPARM(TCVIPWG3) Port Synchronous Communication

```
v tcpip,tcpip,obeyfile,dsn=….UPARM(TCVIPWG3)
```

..UPARM(TCVIPWG4) Port Copilot SSL

```
v tcpip,tcpip,obeyfile,dsn=….UPARM(TCVIPWG4)
```

..UPARM(TCVIPWG5) Port Copilot REST API server port

```
v tcpip,tcpip,obeyfile,dsn=….UPARM(TCVIPWG5)
```

..UPARM(TCVIPWG6) Port SFTP

```
v tcpip,tcpip,obeyfile,dsn=...UPARM(TCVIPWG6)
```

**Round robin example**

`              - DISTMethod ROUNDROBIN`

..UPARM(TCVIPRN0) Port pesit

Example VIPA command to execute:

```
v tcpip,tcpip,obeyfile,dsn=...UPARM(TCVIPRN0)
```

..UPARM(TCVIPRN1) Port pesit ssl

```
v tcpip,tcpip,obeyfile,dsn=...UPARM(TCVIPRN1)
```

..UPARM(TCVIPRN2) Port Copilot

```
v tcpip,tcpip,obeyfile,dsn=...UPARM(TCVIPRN2)
```

..UPARM(TCVIPRN3) Port Synchronous Communication

```
v tcpip,tcpip,obeyfile,dsn=….UPARM(TCVIPRN3)
```

..UPARM(TCVIPRN4) Port Copilot SSL

```
v tcpip,tcpip,obeyfile,dsn=….UPARM(TCVIPRN4)
```

..UPARM(TCVIPRN5) Port Copilot REST API server port

```
v tcpip,tcpip,obeyfile,dsn=….UPARM(TCVIPRN5)
```

..UPARM(TCVIPRN6) Port SFTP

```
v tcpip,tcpip,obeyfile,dsn=...UPARM(TCVIPRN6)
```

You have now customized the VIPA and submitted the programs.
