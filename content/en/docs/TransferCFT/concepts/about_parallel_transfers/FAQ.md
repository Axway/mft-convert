---
    title: "Frequently asked questions"
    linkTitle: "Frequently asked questions"
    weight: 280
---This topic focuses on questions related to maximizing transfer volume and adapting the configuration settings to meet specific needs.

****Q: Can I use a MAXTRANS value that is greater than MAXTASK value multiplied by the TRANTASK value?****

**Answer**: Yes. When there are more transfers than tasks multiplied by the number of transfers per task, the excess transfers are balanced between available tasks. The use of this setting allows for more simultaneous transfers. However depending on the infrastructure and resources, for example if you have limited bandwidth, this could have a negative impact on performance.

In general, you should base your MAXTRANS on the transfer peak in your daily activity.

****Q: What is the maximum number of connections between two Transfer CFTs (UNIX or Windows) if the license is "unlimited"?****

**Answer**: On a UNIX or Windows system, with an unlimited license, the maximum number of possible simultaneous connections between the 2 {{< TransferCFT/axwayvariablesComponentLongName  >}}s is 1000.

> **Note**
>
> More simultaneous sessions to a partner does not necessary give you the fastest transfer performance.

****Q: Why doesn't the number of active simultaneous transfers match my license key maximum?****

The Transfer CFT log shows 1000 transfers are authorized, but there are only a few hundred active:

```
CFTI18I On 1000 authorized simultaneous transfer(s), 256 is(are) active
```

**Answer** **1**: The "authorized simultaneous transfers" value comes from the license key, while the number of active transfers comes from the parameter MAXTRANS.

**Answer** **2**: You can override the MAXTRANS value when starting {{< TransferCFT/axwayvariablesComponentLongName  >}} using the following parameter: cft.run.maxtrans (only when using cft start-and-wait)

**Fix**: Check and update the OS specific UCONF value to match the license key limit and appropriate MAXTRANS for simultaneous transfers.

****Q: How do I configure parallel transfers when using SSL?****

**Answer**: We generally recommend that you use the same values as you would for the corresponding parameter parallel transfer parameter, depending on the CPU resources. That is, you can use the same value for SSLTTASK as you would for TRANTASK, and the same value for SSLMTASK as for MAXTASK, but remember that the SSL tasks consume more CPU, which may lead to an unexpected performance.

****Q: What are the Code 418 messages in the log?****

****Answer**:** A code 418 indicates that the maximum connection number for the partner has been reached. Meaning that if, for example, during multiple simultaneous SEND files the number of transfers exceeds the value of the partner CNXOUT parameter, the transfers retains the code 418, which is normal. Transfer CFT automatically manages these remaining transfers with no action required by the user.

****Q: Can I increase the MAXTASK (number of processing files) to improve performance?****

**Answer**: Yes, you can increase MAXTASK to parallelize I/O access. However doing so consumes more memory and might instead have a negative effect on performance if you set this value too high. The correct balance for you depends on the local available resources.

****Q: I used the highest possible value for MAXTRANS and MAXCNX, so why can I not perform more simultaneous transfers?****

**Answer** **1**: While you can modify the value for the maximum number of parallel transfers using CFTUTIL, remember that you cannot exceed your license key transfer value.

**Answer** **2**: Check the UCONF cft.server.max_session value, which may be limiting the MAXTRANS and MAXCNX parameters.
