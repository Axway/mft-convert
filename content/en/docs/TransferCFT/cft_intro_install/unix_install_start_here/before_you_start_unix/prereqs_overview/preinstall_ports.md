---
title: "Default ports"
linkTitle: "Ports"
weight: 180
--- The following list contains the default {{< TransferCFT/axwayvariablesComponentShortName  >}} port numbers used for installation. You can check in advance that these ports do not conflict with ports used by other applications on the same machine.

You may need to modify the default port numbers, depending on your configuration.

The Internet Assigned Numbers Authority (IANA) reserves the TCP ports 1761- 1768 for {{< TransferCFT/axwayvariablesComponentShortName  >}}. For more information, refer to: [www.iana.org/assignments/service- names- port- numbers/service- names- port- numbers](http://www.iana.org/assignments/service- names- port- numbers/service- names- port- numbers.xhtml?&page=31).

| Component  | Port |
| - - - | - - - |
| PeSIT  | 1761  |
| SSL  | 1762  |
| SFTP  | 1763  |
| COMS  | 1765  |
| Copilot  | 1766  |
| Transfer CFT UI (Copilot) server for {{< TransferCFT/suitevariablesCentralGovernanceName  >}}  | 1767  |
| REST API  | 1768  |
| Flow Manager or {{< TransferCFT/PrimaryCGorUM  >}}  | 12553  |
| Flow Manager or {{< TransferCFT/suitevariablesCentralGovernanceName  >}} SSL  | 12554  |
| {{< TransferCFT/suitevariablesSecureRelayName  >}} MA<br/> ma.comm_port |  <br/> 6801 |
| {{< TransferCFT/suitevariablesSecureRelayName  >}} RA<br/> • ra.comm_port<br/> • ra.admin_port |  <br/> • 6811<br/> • 6810 |

\*not supported

Legend:

- PeSIT (PESITANY protocol): PeSIT in plain text
- SSL: PeSIT protocol over SSL/TLS
- COMS: Synchronous transfers
- Copilot: Provides access to {{< TransferCFT/axwayvariablesComponentShortName >}} UI server from an Internet browser
- Copilot for {{< TransferCFT/suitevariablesCentralGovernanceName >}}: Provides secure access for{{< TransferCFT/PrimaryCGorUM >}} (mutual authentication)
- {{< TransferCFT/PrimaryCGorUM >}}: Used to connect to Central Governance
