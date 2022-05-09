{
    "title": "Troubleshoot the Copilot server",
    "linkTitle": "Troubleshoot the Copilot server",
    "weight": "260"
}Failed cftsu
------------

### {{< TransferCFT/suitevariablesUNIX  >}}

****Create process as user is not set correctly****

If the UCONF parameter copilot.misc.createprocessasuser=NO, the cftsu process cannot start. Check and set to YES.

****There is an incorrect setting in cftsu****

The following message may be due to one of the causes listed below.

```
/home/cft/company/Transfer_CFT/home/bin/cftsu must be launch as setuid root!
```

1. The owner is not root. Check:  
    ```
    ls -l cftsu-rwsrwxrwx 1 cft cft cftsu
    ```

    Fix: Set the root using the chown root:root &lt;file&gt; command.

    ```
    ls -l cftsu-rwsrwxrwx 1 root root
    cftsu
    ```

1. The setuid option (s) is not set for the cftsu file. Check:  
    ```
    ls -l cftsu-rwxrwxrwx 1 root root cftsu
    ```

    Fix: Set using the chmod u+s &lt;file&gt; command.

    ```
    ls -l cftsu
    -rw<span class="underline">s</span>rwxrwx 1 root root cftsu
    ```

1. The nosuid option is set for the disk. Check by executing the mount command:  
    ```  
     > mount
    devpts on /dev/pts type devpts (rw,**nosuid
    ,gid=5,mode=620)
    ```
    **
    1.  If the nosuid flag displays, you cannot set the SetUID (set group id) bit on this disk. You can, though, copy the file to another disk and use the UCONF `copilot.unix.cftsu.fname` parameter to set the path to the new file (see the *Transfer CFT Installation Guide - Unix* for more information).
