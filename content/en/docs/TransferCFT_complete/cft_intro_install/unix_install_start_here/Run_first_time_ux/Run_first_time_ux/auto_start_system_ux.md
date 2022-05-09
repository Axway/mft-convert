{
    "title": "Automatic Transfer CFT start on system start up",
    "linkTitle": "Automatic start on operating system start up",
    "weight": "200"
}This section describes automatic Transfer CFT start procedures.
However, the method used to run Transfer CFT when the system starts can vary
according to the operating system. For more information on the various parameters' syntax, refer to your operating system documentation.

Because the time it takes to shut Transfer CFT down depends on the current
workload, an automatic Transfer CFT shut down may cause the system shut down
procedure to be temporarily suspended.

The examples in this section assume the following:

- Transfer CFT is
    installed in the `cft `user account
- Transfer CFT is
    installed as standard in the `Axway/Transfer_CFT` directory for this account
- In a multi-node environment, the {{< TransferCFT/headerfootervariableshflongproductname  >}} runtime is located in `/mnt/axway/cft/runtime`
- The `home `directory
    for this user is `/home/cft`
- Transfer CFT is
    correctly installed, configured, and manually tested before attempting any automatic
    activation procedure

> **Note**
>
> Note: The following sections refer to using a standard editor, for example, vi.

Using Systemd (Linux)
---------------------

`Systemd `is a system and service manager for Linux operating systems.

### Define the Transfer CFT Copilot service (non multi-node)

Use a standard editor to create the `cftcopilot.service` file in the `/etc/systemd/system` directory for a standalone installation as follows:

```
[Unit]
Description=Axway Transfer CFT Copilot
After=syslog.target network.target
 
[Service]
Type=forking
WorkingDirectory=/home/cft/Axway/Transfer_CFT/runtime
ExecStart=/bin/sh -c '. ./profile && copstart'
ExecStop=/bin/sh -c '. ./profile && copstop'
Group=cft
User=cft
KillMode=process
 
[Install]
WantedBy=multi-user.target
```

#### Define the Transfer CFT service (non multi-node)

Use a standard editor to create the `cft.service` file in the `/etc/systemd/system` directory for a standalone installation as follows:

```
[Unit]
Description=Axway Transfer CFT
After=syslog.target network.target
 
[Service]
Type=forking
WorkingDirectory=/home/cft/Axway/Transfer_CFT/runtime
ExecStart=/bin/sh -c '. ./profile && cft start'
ExecStop=/bin/sh -c '. ./profile && cft stop'
Group=cft
User=cft
 
[Install]
WantedBy=multi-user.target
```

#### Define the Transfer CFT Copilot service (multi-node)

In a multi-node environment, you only need to define the Transfer CFT Copilot as a service. Copilot automatically starts the nodes. Use a standard editor to create the `cftcopilot.service` file in the `/etc/systemd/system` directory as follows:

```
[Unit]
Description=Axway Transfer CFT Copilot
After=syslog.target network.target
 
[Service]
Type=forking
WorkingDirectory=/mnt/axway/cft/runtime
ExecStart=/bin/sh -c '. ./profile && copstart && cft start'
ExecStop=/bin/sh -c '. ./profile && copstop -w'
Group=cft
User=cft
KillMode=process
 
[Install]
WantedBy=multi-user.target
```

Once you have validated that the service starts and stops correctly, you can enable it for automatic starts.

****Start a service****

```
systemctl start <service_name>
```

****Stop a service****

```
systemctl stop <service_name>
```

****Enable a service for automatic start****

```
systemctl enable <service_name>
```

****Disable a service****

```
systemctl disable <service_name>
```

### Using the /etc/inittab file

This procedure is typically valid on all UNIX systems.

The following examples assume that the `su()`
system utility is located in the `/bin` directory.

The start procedures in this section require you to update key system files. If errors
are made, the system may no longer boot correctly, so the system administration should make these modifications.

You can use the `logger()` system command to store error messages
displayed during an automatic start. For this to operate correctly,
the `syslogd()` system daemon must be running on your system. The
system administrator can identify the
specific Transfer CFT messages in the system log files at the:

- Error level and the
    `local0` facility for error messages
- Information level
    and the `local0` facility for a correct start

****Define the {{< TransferCFT/headerfootervariableshflongproductname  >}} service****

Use a standard editor to add the following line
at the end of the `/etc/inittab` file:

```
cft:2:once:/bin/su - cft -c ’. Axway/Transfer_CFT/runtime//profile; cft start –batch’
```

****Define the Transfer CFT Copilot service****

Use a standard editor to add the following line to the end of the `/etc/inittab` file:

```
cftcopilot:2:once:/bin/su - cft -c ’. Axway/Transfer_CFT/runtime//profile; copstart’
```

### Adding a file to /etc/rc2.d

You can only use this method on systems with a `/etc/rc2.d `
directory (SOLARIS for example).

The following examples assume that the `su()`
system utility is located in the `/bin` directory.

The start procedures in this section require you to update key system files. If errors
are made, the system may no longer boot correctly, so the system administration should make these modifications.

You can use the `logger()` system command to store error messages
displayed during an automatic start. For this to operate correctly,
the `syslogd()` system daemon must be running on your system. The
system administrator can identify the
specific Transfer CFT messages in the system log files at the:

- Error level and the
    `local0` facility for error messages
- Information level
    and the `local0` facility for a correct start

****Define the {{< TransferCFT/headerfootervariableshflongproductname  >}} service****

Use a standard editor, *vi* for example, to create a new file called
`/etc/rc2.d/S99cft`.  

Add the appropriate operating system start shell script to this
file. The following shell script provides a basic example:

```
\#!bin/sh
\# Starting Transfer CFT
 
if [ -f /home/cft/Axway/Transfer_CFT/runtime/profile]
then
/bin/su - mycft -c ’. Axway/Transfer_CFT/runtime/profile; cft start –batch’
fi
```

****Define the Transfer CFT Copilot service****

Use a standard editor, vi for example, to create a new file called `/etc/rc2.d/S99cftcopilot`.

Add the appropriate operating system start shell script to this file. The following shell script provides a basic example:

```
\#!bin/sh
\# Starting Transfer CFT Copilot
if [ -f /home/cft/Axway/Transfer_CFT/runtime/profile]
then
/bin/su - mycft -c ’. Axway/Transfer_CFT/runtime/profile; copstart’
fi
```
