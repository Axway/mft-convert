---
title: "PassPort AM persistent cache "
linkTitle: "PassPort AM persistent cache "
weight: 200
--- This section describes how to configure access management when not using {{< TransferCFT/PrimaryCGorUM  >}}.

When enabled, {{< TransferCFT/axwayvariablesComponentShortName  >}} retrieves all permissions from the PassPort AM server and stores this information in the cache. This allows {{< TransferCFT/axwayvariablesComponentShortName  >}} to continue to operate using the stored information if the PassPort AM server is non- operational.

> **Note**
>
> The user/password login are not stored in the cache. This means that if the PassPort server is down, you cannot connect to Copilot, Transfer CFT REST APIs, or the Transfer CFT UI.

| Parameters  | Default  | Description  |
| --- | --- | --- |
| am.passport.persistency.enable  | Yes  | Enables persistent support for PassPort AM. |
| am.passport.persistency.fname  | $(cft.runtime_dir)/data/CFTAM  | Persistent cache file name for PassPort AM.  |
| am.passport.persistency.check_interval  | 600  | Interval in seconds between two checks of access management updates.<br/> See also the information concerning CFTSXPAM or copsxpam below. |
| am.passport.persistency.cftsxpam.enable  | Yes  | Enable the CFTSXPAM process, which updates the PassPort AM cache.  |

## Updating the cache

When Transfer CFT or Copilot is configured to use PassPort AM , it periodically scans for changes in user rights. The changes are then saved in the file defined in `am.passport.persistency.fname`. These scans occur at regular intervals as defined by the `am.passport.persistency.check_interval` parameter.

- To force an immediate cache update, you can manually run CFTSXPAM (as described below).
- If a user (a non- superuser) is not listed in the cache and tries to start Transfer CFT, Transfer CFT cannot start and displays the [CFTX03W](../../../troubleshoot_intro/messages_and_error_codes_start_here/cftx_messages) error in the log. To fix, you can manually execute CFTSXPAM and restart Transfer CFT.

### Manually run CFTSXPAM

1. Log on as a Transfer CFT superuser, meaning a user that is defined in `am.passport.superuser`.
1. Load the Transfer CFT profile.
1. Check that CFTSXPAM is enabled, `am.passport.persistency.cftsxpam.enable = yes`.
1. Execute the command: `CFTSXPAM`

> **Note**
>
> On z/OS platforms the offline version of the CFTSXPAM process is called CFTUXPAM.

#### Recommendation

When using LDAP in PassPort or {{< TransferCFT/PrimaryCGorUM  >}} for access management, it is highly recommended that you run the CFTSXPAM process offline prior to starting Transfer CFT, or the Transfer CFT Copilot server, if the persistent cache does not exist. Neglecting to do so could result in a significant delay in the first login.

> **Note**
>
> If you need to troubleshoot the cache, you can see the information in Extract the Access Management Cache.
