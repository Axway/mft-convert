---
    "title": "Tracking configuration changes",
    "linkTitle": "Configuration change logging",
    "weight": "140"
---
{{% TransferCFT/snippets/audit_changes%}}

Disable XFB.Log
---------------

By default, `sentinel.xfb.log` is set to `IEWF `(information, error, warning, and fatal), which sends Transfer CFT log information to Sentinel. To disable the XFB.Log, use the uconf utility to set this value to ' '.

```
CFTUTIL uconfset id=sentinel.xfb.log, value=' '
```

****Related topics****

- UCONF:[unified configuration](../../../admin_intro/uconf)
- [XFBTransfer]()
