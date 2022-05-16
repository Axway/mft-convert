---

    title: How to free disk space from service packs
    linkTitle: How to free disk space from updates
    weight: 230

---
This topic describes how to reclaim disk space that is taken up by Service Packs and updates applied to Transfer CFT over time.  

## Use the purge command

Use the purge command followed by the appropriate options to removed accumulated backups of installed Transfer CFT updates.  

1. Navigate to the Transfer CFT installation directory.  
1. Run the <span class="code">`purge `</span>command as described in the following sections.<span class="code">``</span>

****Syntax****

`purge [-h | --help] | [-k | --keep] [number]  [-p | --pretend]`

- UNIX:<span class="code">` purge.sh`</span>
- Windows: <span class="code">`purge64.exe `</span>or <span class="code">`purge32.exe`</span> (from a command line window)  

Where:  

- \[-h | --help\]: Displays the command help.  
- \[-k | --keep\] \[number\]: Specifies the number of updates that should be kept.
- \[-p | --pretend\]: Previews the action to be done.  

****Examples  
****

Keep only the last backup, meaning you can remove the current patch or SP.

```
<Transfer_CFT_install_dir>/purge.sh -k 1
```

Remove all backups, meaning that you cannot remove the current patch or SP.

```
<Transfer_CFT_install_dir>/purge.sh -k 0
```

&lt;Options> include:

- \[-d | --debug\]: Generates debug information in the log file.

****Example****

```
<Transfer_CFT_install_dir>/purge.sh -k 1 -d
```
