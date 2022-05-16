---

    title: Installing  the Sun Cluster 
    linkTitle: Installing the Sun Cluster 
    weight: 250

---
This topic describes the steps involved in installing the Sun Cluster
with Transfer CFT.

## Pre-installation

Before installing Transfer CFT, create accounts with the same user name
on each node.

## Installation

Proceed with a normal Transfer CFT installation. Use the batch mode
to deploy the product on the multiple cluster nodes.

### Post-installation

After you install Transfer CFT you must modify and reinterpret the profile environment files, in order to share:

- Configuration files
- Transfer catalog
- Communications
    file

****Example of a modified profil**e** file****

`#--------------------------------------------`

`# Starting CFT configuration`

`# Fri Sep 26 17:28:32 MEST 2006`

`#----------------------------------------`

`CFTDIRINSTALL=/home/cft/cft`

`CFTDIRRUNTIME =/home/cft/cft/runtime`

`CFTDIRAPI=$CFTDIRRUNTIME /src/capi/`

`CFTDIRDAT=$CFTDIRRUNTIME/data/`

`CFTDIREXIT=$CFTDIRRUNTIME/src/exit/`

`CFTDIRINS=$CFTDIRRUNTIME/conf/`

`CFTDIRLOG=$CFTDIRRUNTIME/log/`

`CFTDIRPUB=$CFTDIRRUNTIME/pub`

`CFTDIRSEC=$CFTDIRRUNTIME/conf/pki/`

`CFTHINI=$CFTDIRRUNTIME/data/sec.ini`

`CFTHPARM=$CFTDIRRUNTIME/data/secparm`

`CFTACNT=$CFTDIRRUNTIME/data/cft_acnt`

`CFTACNTA=$CFTDIRRUNTIME/data/cft_acnta`

`CFTLOG=$CFTDIRRUNTIME/data/cft_log`

`CFTLOGA=$CFTDIRRUNTIME/data/cft_loga`

`CFTCLUSTERSHARE=/global/cft/param`

`CFTPKIDIR=$CFTCLUSTERSHARE/pki`

`CFTPKU=$CFTCLUSTERSHARE/pki/pkibase`

`CFTCATA=$CFTCLUSTERSHARE/cft_cata`

`CFTCOM=$CFTCLUSTERSHARE/cft_com`

`CFTPARM=$CFTCLUSTERSHARE/cft_parm`

`CFTPART=$CFTCLUSTERSHARE/cft_part`

` `

`PATH=$PATH:$CFTDIRINSTALL/bin:$CFTDIREXIT:$CFTDIRAPI`

`export PATH`

`export CFTDIRINSTALL CFTDIRRUNTIME CFTDIRAPI CFTDIRDAT CFTDIREXIT CFTDIRINS`

`export CFTDIRLOG CFTDIRPUB CFTDIRSEC`

`export CFTHINI CFTHPARM CFTPKIDIR`

`export CFTPKU CFTACNT CFTACNTA CFTCATA CFTCOM CFTLOG CFTLOGA`

`export CFTPARM CFTPART`

`export CFTCLUSTERSHARE`

`#-------------------------------------------`

`# CFT configuration completed`

`# Fri Sep 26 17:28:35 MEST 2003`

`#-----------------------------------------`

`CFTCLUSTERSND=/global/cft/snd`

`CFTCLUSTERRCV=/global/cft/rcv`

`export CFTCLUSTERSND CFTCLUSTERRCV`

<span id="Solaris_Sun_cluster_monitor_configuration"></span>

## Transfer CFT configuration

1. Configure the virtual IP of
    the cluster in the cftnet card:

`cftnet       id     = TCPIP,type     = TCP,call     = inout,host     = 172.17.50.57,     /*   virtual IP of the cluster */maxcnx     = 3,mode     = replace`

1. Enter the paths to the shared
    system files for the files to be sent or received.
