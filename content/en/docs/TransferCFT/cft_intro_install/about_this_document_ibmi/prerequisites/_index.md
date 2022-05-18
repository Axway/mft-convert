---
title: "Prerequisites"
linkTitle: "Prerequisites"
weight: 140
--- This section describes the prerequisites for Transfer CFT {{< TransferCFT/PrimaryTransferCFTversion  >}} IBM i and covers:

- Hardware environment
- Software environment

## Hardware prerequisites

Transfer CFT {{< TransferCFT/PrimaryTransferCFTversion  >}} IBM i can only be installed on an system based on the RISC architecture.

### Disk space

For performance reasons, you are advised to configure a storage pool size of at least 250,000 Kbytes (245 MB). See .

You must ensure that the:

- Disk space used to restore the Transfer CFT objects is approximately 250 MB, excluding files to be transferred.
- Total disk space used on the system still allows acceptable performance levels to be maintained (&lt; 80%).

## Software environment

This section describes the Transfer CFT {{< TransferCFT/PrimaryTransferCFTversion  >}} IBM i software environment:

- Transfer CFT IBM i {{< TransferCFT/PrimaryTransferCFTversion >}} supports V7R2 (7.2) and higher.
- The file management mechanism uses the standard OS database management system and IFS (Integrated File System). The Transfer CFT IBM i Manager uses the PDM (Program Development manager) and SEU (Source Entry Utility).
- The Transfer CFT IBM i Manager uses the UIM (User Interface Manager) V7R2 and higher.
- The Transfer CFT APIs only support an ILE environment.

## Java

When using Secure Relay, you require Java to be installed in the same environment as the Transfer CFT installation. The Master Agent is managed, but the Router Agent can be in another environment.

Check your Java version, {{< TransferCFT/suitevariablesSecureRelayName  >}} requires Java JRE 8.

## End User License Agreement

You should read and accept the End User License Agreement (EULA) prior to installing Transfer CFT. The EULA file is in the directory where you decompressed the Transfer CFT package.
