{
    "title": "How to free disk space from service packs",
    "linkTitle": "How to free disk space from updates",
    "weight": "220"
}This topic describes how to reclaim disk space that is taken up by Service Packs and updates applied to Transfer CFT over time.  

Use the purge command
---------------------

Use the purge command followed by the appropriate options to removed accumulated backups of installed Transfer CFT updates.  

1. Navigate to the Transfer CFT installation directory.  
1. Run the `purge `command as described in the following sections.``

****Syntax****

`purge [-h &#124; --help] &#124; [-k &#124; --keep] [number]  [-p &#124; --pretend]`

- UNIX:` purge.sh`
- Windows: `purge64.exe `or `purge32.exe` (from a command line window)  

Where:  

- [-h &#124; --help]: Displays the command help.  
- [-k &#124; --keep] [number]: Specifies the number of updates that should be kept.
- [-p &#124; --pretend]: Previews the action to be done.  

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

&lt;Options&gt; include:

- [-d &#124; --debug]: Generates debug information in the log file.

****Example****

```
<Transfer_CFT_install_dir>/purge.sh -k 1 -d
```
