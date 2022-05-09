{
    "title": "Migrate or upgrade Transfer CFT",
    "linkTitle": "Migrate or upgrade Transfer CFT",
    "weight": "220"
}This chapter is designed to assist administrators or users who are tasked with upgrading or migrating from an existing Transfer CFT version to Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}}.

- [Migrate](#Migrate): Use this procedure to migrate an existing Transfer CFT 2.3.2 installation
- [Upgrade](#Upgrade): Use this procedure to automatically upgrade an existing Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}}

<span id="Importan"></span>

Important information
---------------------

Before performing a migration or upgrade procedure, you must:

- Update your Transfer CFT to the most recent service pack version.
- Back up Transfer CFT.
- Set the Transfer CFT profile.
- Stop the existing version of Transfer CFT and the UI server.

> **Note**
>
> Note: You can uninstall an upgrade if needed. However, doing so rolls back to the previous version before the upgrade, so all transfers and configuration modifications that were performed since the upgrade are lost.

<span id="Migrate"></span>

Migrate
-------

This section describes how to migrate the following elements:

- Partner file
- Parameter file
- Client exits and applications

Before performing a migration be certain to review the section [Important information](#Importan).

> **Note**
>
> Note: You cannot migrate the Transfer CFT Guardian 2.3.2 catalog and communication files to Transfer CFT HP NonStop 3.10.

### Partner and parameter files

This section explains how to extract the definitions and import them in the new environment as part of a migration.

1. From the Transfer CFT 2.3.2 environment, export the configuration in a sequential file using the command:
1. Transfer the file from the native environment to the OSS environment.
1. If the configuration contains symbolic variables, replace the circumflex (^) character with an ampersand (&). For example, `^NFNAME` becomes `&NFNAME`.
1. In the Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}} OSS environment, import as follows:

    1.  Create the partner and parameter files:
    2.  Import the configuration from the generated file:

### System configuration file

Transfer CFT 2.3.2 is based on a CFGSYS configuration file. While you cannot use this configuration file in the current version of Transfer CFT, most of the previous parameters have an equivalent in the current version.

System configuration file example

```
KW     Type    Volume.Subvolume
[CFTWRK](#CFTWRK)  default $DATA00.CFT232UD
KW   AP  [CPU](#CPU) PNL SMS  MMS [TERM](#TERM)    IN       COL  SWAPF    MEAS  [CPUBACKUP](#CPUBACKUP)
CFTAPP 1  0   5  9600 600 $STTI  $STTI    $0   FSWAP01   Y     1
CFTAPP 2  1   X  0    0   default default  $0   none     X     X
CFTAPP 3  0   5  0    0   default default  $0   none     X     X
CFTAPP 4  0   X  0    0   default default  $0   none     X     X
KW      AP  LMS  ID       [PN](#PN)     [PRI](#PRI) CPU  OBJECT                  OUT
CFTTASK 1  3200 CFTMAIN  $LAMGr  150  X   $DATA00.CFT232X.CFTMGRx   $s.\#amgr
CFTTASK 1    30 CFTLOG   $LALoG  150  X   $DATA00.CFT232X.CFTLOGx   $s.\#alog
CFTTASK 1    30 CFTTCOM  $LACoM  150  X   $DATA00.CFT232X.CFTCOMX   $s.\#acom
[…]
KW       OP       TERM
[CFTTERM](#CFTTERM)  OPLOG    $0
CFTTERM  OP000001 $S.\#OPOUT
CFTTERM  OP000002 $S.\#OPOUT
CFTTERM  OP000003 $TRM3.\#A
KW      DEF       VAL     
[CFTDEF](#parameters)  [CFGSYS](#CFGSYS)    $DATA00.moncfg.CFGSYS
CFTDEF  [TRKCNF](#TRKCNF)    $DATA00.moncfg.CONFSENT
CFTDEF  [CFTXFB](#CFTXFB)    $DATA00.moncfg.CONFIUI
CFTDEF  CFTPARM   $DATA00.monfiles.FPARM
CFTDEF  CFTPART   $DATA00.monfiles.FPART
CFTDEF  CFTCATA   $DATA00.monfiles.FCAT
CFTDEF  CFTCOM    $DATA00.monfiles.FCOM
CFTDEF  CFTLOG    $DATA00.monfiles.FLOG
CFTDEF  CFTLOGA   $DATA00.monfiles.FLOGA
CFTDEF  CFTACCNT  $DATA00.monfiles.FACCNT
CFTDEF  CFTACCNTA $DATA00.monfiles.FACCNTA
CFTDEF  CFTPKU    $DATA00.monfiles.PKIBASE
CFTDEF  CFTHICNF  $DATA00.monfiles.SECCNF
CFTDEF  CFTHINI   $DATA00.moncfg.SECINI
CFTDEF  CFTHPARM  $DATA00.monfiles.SECPARM
CFTDEF  CFTHACT   $DATA00.monfiles.SECFRACT
CFTDEF  CFTHOBJ   $DATA00.monfiles.SECFROBJ
KW     [MANAGER](#MANAGER)   [NB-ATTACH-SET](#NB)
CFTBT    TACL     NBASCFTLI
KW       PARM                  VAL
TCPPARM  TCPIP^HOST^FILE       $SYSTEM.ZTCPIP.HOSTS
TCPPARM  TCPIP^PROCESS^NAME    $ZTC0
TCPPARM  TCPIP^HOST^FILE^2     $SYSTEM.ZTCPIP.HOSTS
TCPPARM  TCPIP^PROCESS^NAME^2  $ZTC0
KW       PARM                  VAL
[X25PARM](#X25PARAM)  X25^CNX^TIME^OUT       40
KW       PARM                  VAL
NONSTOP^RESTART^DELAY           20
NONSTOP^PLIST^TEMP^FILE         $DATA00.montemp.PTMPLIST
```

#### Equivalents in Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}}

- <span id="CFTWRK"></span>CFTWRK:
    [cft.guardian.cftwrk](../intro_os_features/hp_ns_batch#cft.guardian.cftwrk)
- <span id="CPU"></span>CPU: [cft.guardian.processor](../intro_os_features/hp_ns_batch#cft.guardian.processor)
- <span id="TERM"></span>TERM: [cft.guardian.hometerm](../intro_os_features/hp_ns_batch#cft.guardian.hometerm)
- <span id="CPUBACKUP"></span>CPUBACKUP: [cft.guardian.backup_processor](../intro_os_features/hp_ns_batch#cft.guardian.backup_processor)
- <span id="PN"></span>PN: [cft.guardian.process_name_prefix](../intro_os_features/hp_ns_batch#cft.guardian.process_name_prefix)
- <span id="PRI"></span>PRI: [cft.guardian.priority](../intro_os_features/hp_ns_batch#cft.guardian.priority)
- <span id="CFTTERM"></span>CFTTERM is no longer supported.
- <span id="parameters"></span>CFTDEF:
    -   Most parameters have become environment variables that are accessible under OSS.
    -   <span id="CFGSYS"></span>CFGSYS is deprecated.
    -   <span id="TRKCNF"></span>TRKCNF is replaced by the Sentinel configuration in UCONF.
    -   <span id="CFTXFB"></span>CFTXFB is deprecated.
- <span id="MANAGER"></span>MANAGER: TACL is the default manager.
- <span id="NB"></span>NB-ATTACH-SET: [cft.guardian.netbatch.attachment_set](../intro_os_features/hp_ns_batch#cft.guardian.netbatch.attachment_set).
- <span id="X25PARAM"></span>X25PARAM is not supported.

<span id="Upgrade"></span>

Upgrade
-------

Before performing an upgrade be certain to review the section [Important information](#Importan).

### Overview

You can perform an upgrade by installing Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}} over an existing Transfer CFT 3.2.4 installation using the procedure described in [Install Transfer CFT](). However, the installation directory `<installation_directory>` should point to the installation directory of the existing Transfer CFT 3.2.4 installation. You can then provide the same additional parameters.

The installation procedure upgrades Transfer CFT, where the configuration of the existing installation is exported, and is automatically re-imported after the upgrade.

### After auto-importing

The installation creates a new directory called `up-<version>` in the runtime directory. This directory stores all of the information used during the auto-import process. You can modify the extracted files and directory, and manually re-import this data at any time.

****Extracted data****

The auto-import directory contains the following:


| File  | Directory  | Description  |
| --- | --- | --- |
| cftcat.xml  |   | Catalog file  |
| cftcom.xml  |   | Communication media  |
| cftconf.cfg  |   | The general Transfer CFT configuration, which is applied to the new installation (contained in CFTPARM/CFTPART internal datafiles)  |
| cftpki.cfg  |   | The PKI configuration that is applied to the new installation  |
|   | PKI directory  | The extracted SSL certificates  |


Client exits and applications
-----------------------------

The following rules apply to migrating client exits and client applications:

- The use of client exits and applications have not changed, but you must recompile the client programs to take into account the new data structure.
- The new data structures are described in a DDL format, have the same name as in version 2.3.2, and are located in $volume.&lt;subversion&gt;IH.

<!-- -->

- Additionally, the C language definition derived from the DDL definition is also part of the packaging.
