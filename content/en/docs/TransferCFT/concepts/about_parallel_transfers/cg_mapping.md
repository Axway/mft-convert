---

    title: Parameter equivalents in Central Governance
    linkTitle: Parameter equivalents in Central Governance
    weight: 300

---

| CG field  | CG values  | CFTUTIL <br/> parameter | Description  |
| --- | --- | --- | --- |
| Maximum simultaneous transfers  | Linux and Windows: 2-1000 (<u>128</u>)<br/> z/OS and IBM i: 2-990 (<u>128</u>) | MAXTRANS  | The maximum number of simultaneous connections that {{< TransferCFT/axwayvariablesComponentShortName  >}} accepts to establish for a network resource.  |
| Connections  | 1-2000 (128)  | MAXCNX  | The maximum number of simultaneous connections that Transfer CFT accepts to establish on a given network resource.  |
| Disconnect timeout  | 0-3600 (60)  | DISCTR, DISCTC  | The wait timeout for either a response to the protocol connection request or to the partner in the connection, before disconnecting.  |


****Related topics****

- [About parallel transfers](../)
- [FAQ and troubleshooting](../faq)
