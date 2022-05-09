{
    "title": "Tuning the database cache",
    "linkTitle": "Tuning the database cache",
    "weight": "220"
}This section describes how you can enhance performance using the database cache feature.

Overview
--------

For each new transfer, {{< TransferCFT/axwayvariablesComponentLongName  >}} retrieves information directly from the internal database for **dynamic objects** as described below. This type of accessing generates a lot of file I/O operations for the cftparm file.

In an environment with high-performance file I/O, such as local SSD storage, frequently accessing these files is not an issue. However, on a multi-node Transfer CFT where the databases are located on a shared file system, such as NFS, this type of file accessing could have a significant impact on Transfer CFT's global performance.

To reduce file I/O operations, thus improving performance on a non-performant disk, we recommend that you keep the database cache enabled. Doing so enables {{< TransferCFT/axwayvariablesComponentLongName  >}} to more quickly access the required objects.

Dynamic object considerations
-----------------------------

The database cache feature affects the management of dynamic objects such as transfer objects (CFTAUTH, CFTIDF, CFTXLATE, CFTSEND, CFTRECV, CFTEXIT,...), partner objects (CFTPART, CFTDEST,...), and the security object (CFTSSL direct=client). See Dynamic commands modification for more information.

If you modify an object that is already loaded in the cache, meaning the object was used to execute a transfer since the last Transfer CFT start up, your modifications are not taken into account for following transfers until you either restart {{< TransferCFT/axwayvariablesComponentLongName  >}} or execute the `RECONFIG type=PARMCACHE` command as described below.

Set the database cache size
---------------------------

You can manage the database cache size using the UCONF `cft.server.parm.cache_size` and `cft.server.parm.cache_timeout` parameters. The `size `parameter allows you to define the number of entries in the cache for the CFTPARM database. Setting the value to zero disables the cache, the default is 5000, and the maximum value is 10,000. And the `timeout `defines the parm/part cache expiration time.

If memory is not an issue on the machine where Transfer CFT is running, we recommend setting the cache size to the maximum value (10,000). When setting the cache value to either 100 or 10,000, Transfer CFT consumes no more than an additional 2 MB or 200 MB, respectively.

If you set the cache size to a value that is lower than the space needed to store all objects, used objects typically remain in the cache.

<span id="Clear"></span>

Clear the database cache
------------------------

You can use the command RECONFIG [type](../../c_intro_userinterfaces/command_summary/parameter_intro/type)=PARMCACHE to clear the cache while Transfer CFT is running. After the commands executes, all modifications applied to dynamic objects are taken into account without restarting Transfer CFT.

For more information on the RECONFIG command, see [Manage configuration updates - RECONFIG](../../admin_intro/admin_commands_intro/reconfig).
