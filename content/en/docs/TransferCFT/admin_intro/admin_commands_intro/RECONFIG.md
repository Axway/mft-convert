---
title: "Manage configuration updates - RECONFIG"
linkTitle: "RECONFIG - Modify the configuration"
weight: 310
--- Use this command to manage configuration updates for the types listed below.

********Syntax********

**`RECONFIG`**

`[ TYPE   = { CRON &#124; UCONF &#124; CAT &#124; FOLDER  &#124; PARMCACHE &#124; AM   } ] `

********CRON********

When the type is set to CRON, Transfer CFT sends a notification to reload the
enabled CRONJOBs. This command is used when a CFTCRON has been modified by:

- Inserting a new cronjob (with
    a CRONTAB value that matches the CRONTABS list)
- Deleting a new cronjob
- Modifying a cronjob

Messages are then displayed in the log ([CFTI36I](../../../troubleshoot_intro/messages_and_error_codes_start_here/cfti_messages) and [CFTI37I](../../../troubleshoot_intro/messages_and_error_codes_start_here/cfti_messages)).

For example:

```
CFTUTIL RECONFIG TYPE=CRON
```

********UCONF********

When TYPE=UCONF, the UCONF reconfigurable variables are reloaded. Messages are then displayed in the log ([CFTS43I](../../../troubleshoot_intro/messages_and_error_codes_start_here/cfts_messages)). Note that only the UCONF parameters flagged with RECONFIG/IRECONFIG are affected.

For example:

```
CFTUTIL RECONFIG TYPE=UCONF
```

********CAT********

Use this type parameter to dynamically increase the catalog size. Messages are then displayed in the log ([CFTC13I](../../../troubleshoot_intro/messages_and_error_codes_start_here/cftc_messages) and [CFTC13E](../../../troubleshoot_intro/messages_and_error_codes_start_here/cftc_messages)). An additional parameter when using CAT is RECNB (number of records in the catalog). For example:

```
CFTUTIL RECONFIG TYPE=CAT,RECNB=xxx
```

****FOLDER****

Use this command if you need to reload the folder objects after a modification. For example:

```
CFTUTIL RECONFIG TYPE=FOLDER
```

****PARMCACHE****

Use this parameter to clear the cache while Transfer CFT is running. After the command execution, all changes applied to dynamic objects are taken into account, without restarting Transfer CFT. If you have set the UCONF `cft.server.parm.cache_size` value to something other than zero (0), this command reloads both the CFTPART and CFTPARM objects. For example:

```
CFTUTIL RECONFIG TYPE=PARMCACHE
```

****AM****

Reload roles (CFTROLE) and privileges (CFTPRIV). You can manually create these objects or they can be deployed via Flow Manager. For more information on CFTROLEs and CFTPRIVs, please see [Access Management using Flow Manager](../../../internal_a_m_start_here/fm_access_management). For example:

```
CFTUTIL RECONFIG TYPE=AM
```
