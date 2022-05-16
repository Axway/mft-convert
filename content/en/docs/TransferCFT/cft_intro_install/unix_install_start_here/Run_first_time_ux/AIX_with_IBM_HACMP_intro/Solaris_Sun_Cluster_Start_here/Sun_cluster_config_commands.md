---

    title:   Sun cluster service configuration commands
    linkTitle: SCSWITCH and SCSTAT commands
    weight: 270

---
Sun Cluster services are managed and monitored using the following
service configuration commands:

- scswitch for startup,
    stop, and switch over
- scstat for monitoring

<span id="scswitch"></span>

### Sun cluster scswitch command

The commands are as follows:

`scswitch -z -g resource_grp[,...]   -h node[,...]`

`scswitch -z -D device_group_name[,...]   -h node`

`scswitch -S -h from_node`

`scswitch -R -h node[,...]   -g resource_grp[,...]`

`scswitch -m -D device_service_name[,...]`

`scswitch -e | -n [-M]   -j resource[,...]`

`scswitch -u | -o -g resource_grp[,...]`

`scswitch -c -h node[,...]   -j resource[,...] -f flag_name`

`scswitch -Z [-g resource_grp[,...]]`

`scswitch -F -g resource_grp[,...]   | -D device_group_name[,...]`

`scstat [-DWgnpqi] [-v[v]]   [-h <host>]`

The following sections describe the commands necessary
to monitor, start, stop and switchover services. Refer to the Sun documentation
and the "man" pages for additional details.

### Sun cluster scstat command

Example of scstat exit :

root@chou # scstat

`-Resource Groups --`

`Group Name Node Name State`

`---------- --------- -----`

`Group: xos-rg chou Online`

`Group: xos-rg fleur Offline`

`Group: pel-rg chou Online`

`Group: pel-rg fleur Offline`

`Group: CFT-rg chou Offline`

`Group: CFT-rg fleur Online`

 

`-- Resources --`

`Resource Name Node Name State Status Message`

`------------- --------- ----- --------------`

`Resource: xos-disk chou Online Online`

`Resource: xos-disk fleur Offline Offline`

`Resource: ops-ip chou Online Online - LogicalHostname online.`

`Resource: ops-ip fleur Offline Offline`

`Resource: xos-sm chou Online Online`

`Resource: xos-sm fleur Offline Offline`

`Resource: xos-sp chou Online Online`

`Resource: xos-sp fleur Offline Offline`

`Resource: pel-ip chou Online Online - LogicalHostname online.`

`Resource: pel-ip fleur Offline Offline - LogicalHostname offline.`

`Resource: pel-disk chou Online Online`

`Resource: pel-disk fleur Offline Offline`

`Resource: pel-gds chou Online Online`

`Resource: pel-gds fleur Offline Offline`

`Resource: cft-ip chou Offline Offline - LogicalHostname offline.`

`Resource: cft-ip fleur Online Online - LogicalHostname online.`

`Resource: cft-disk chou Offline Offline`

`Resource: cft-disk fleur Online Online`

`Resource: cft-gds chou Offline Offline`

`Resource: cft-gds fleur Online Online`

`---------------------------------------------------------------`

## Starting the service

The following command activates the Transfer CFT-rg service:

scswitch
–Z –g CFT-rg

View the log extract for starting Transfer CFT on a node

`Oct   10 18:35:18 fleur Cluster.RGM.rgmd: [ID 707948 daemon.notice] launching   method <hafoip_prenet_start> for resource <cft-ip>, resource   group <CFT-rg>, timeout <300> seconds`

`Oct   10 18:35:18 fleur Cluster.RGM.rgmd: [ID 148023 daemon.notice] method <hafoip_prenet_start>   completed successfully for resource <cft-ip>, resource group <CFT-rg>`

`Oct   10 18:35:18 fleur Cluster.RGM.rgmd: [ID 707948 daemon.notice] launching   method <hastorageplus_prenet_start> for resource <cft-disk>,   resource group <CFT-rg>, timeout <1800> seconds`

`Oct   10 18:35:20 fleur Cluster.Framework: [ID 801593 daemon.notice] stdout:   becoming primary for dsk/d16`

`Oct   10 18:35:20 fleur Cluster.RGM.rgmd: [ID 148023 daemon.notice] method <hastorageplus_prenet_start>   completed successfully for resource <cft-disk>, resource group <CFT-rg>`

`Oct   10 18:35:20 fleur Cluster.RGM.rgmd: [ID 707948 daemon.notice] launching   method <hafoip_start> for resource <cft-ip>, resource group   <CFT-rg>, timeout <500> seconds`

`Oct   10 18:35:21 fleur Cluster.RGM.rgmd: [ID 148023 daemon.notice] method <hafoip_start>   completed successfully for resource <cft-ip>, resource group <CFT-rg>`

`Oct   10 18:35:21 fleur Cluster.RGM.rgmd: [ID 707948 daemon.notice] launching   method <hafoip_monitor_start> for resource <cft-ip>, resource   group <CFT-rg>, timeout <300> seconds`

`Oct   10 18:35:21 fleur Cluster.RGM.rgmd: [ID 707948 daemon.notice] launching   method <hastorageplus_start> for resource <cft-disk>, resource   group <CFT-rg>, timeout <90> seconds`

`Oct   10 18:35:21 fleur Cluster.RGM.rgmd: [ID 148023 daemon.notice] method <hafoip_monitor_start>   completed successfully for resource <cft-ip>, resource group <CFT-rg>`

`Oct   10 18:35:21 fleur Cluster.RGM.rgmd: [ID 148023 daemon.notice] method <hastorageplus_start>   completed successfully for resource <cft-disk>, resource group <CFT-rg>`

`Oct   10 18:35:21 fleur Cluster.RGM.rgmd: [ID 707948 daemon.notice] launching   method <hastorageplus_monitor_start> for resource <cft-disk>,   resource group <CFT-rg>, timeout <90> seconds`

`Oct   10 18:35:21 fleur Cluster.RGM.rgmd: [ID 707948 daemon.notice] launching   method <gds_svc_start> for resource <cft-gds>, resource group   <CFT-rg>, timeout <300> seconds`

`Oct   10 18:35:21 fleur Cluster.RGM.rgmd: [ID 148023 daemon.notice] method <hastorageplus_monitor_start>   completed successfully for resource <cft-disk>, resource group <CFT-rg>`

`Oct   10 18:35:31 fleur Cluster.RGM.rgmd: [ID 148023 daemon.notice] method <gds_svc_start>   completed successfully for resource <cft-gds>, resource group <CFT-rg>`

`Oct   10 18:35:31 fleur Cluster.RGM.rgmd: [ID 707948 daemon.notice] launching   method <gds_monitor_start> for resource <cft-gds>, resource   group <CFT-rg>, timeout <300> seconds`

`Oct   10 18:35:31 fleur Cluster.RGM.rgmd: [ID 148023 daemon.notice] method <gds_monitor_start>   completed successfully for resource <cft-gds>, resource group <CFT-rg>`

### Stopping the service

The following command deactivates the Transfer CFT resources. To stop
Transfer CFT, deactivate the cft-gds resource. When you stop Transfer
CFT using cft stop, an automatic restart of Sun Cluster is triggered.

`scswitch   –n –j cft-gdsscswitch   –n –j cft-ipscswitch   –n –j cft-disk`

View the log extract for stopping Transfer CFT on a node

`Oct   10 18 :36 :34 fleur Cluster.RGM.rgmd : [ID 707948 daemon.notice] launching   method <hafoip_monitor_stop> for resource <cft-ip>, resource   group <CFT-rg>, timeout <300> seconds`

`Oct   10 18 :36 :34 fleur Cluster.RGM.rgmd : [ID 707948 daemon.notice] launching   method <hastorageplus_monitor_stop> for resource <cft-disk>,   resource group <CFT-rg>, timeout <90> seconds`

`Oct   10 18 :36 :34 fleur Cluster.RGM.rgmd : [ID 707948 daemon.notice] launching   method <gds_monitor_stop> for resource <cft-gds>, resource   group <CFT-rg>, timeout <300> seconds`

`Oct   10 18 :36 :34 fleur Cluster.RGM.rgmd : [ID 148023 daemon.notice] method   <hastorageplus_monitor_stop> completed successfully for resource   <cft-disk>, resource group <CFT-rg>`

`Oct   10 18 :36 :34 fleur Cluster.RGM.rgmd : [ID 148023 daemon.notice] method   <hafoip_monitor_stop> completed successfully for resource <cft-ip>,   resource group <CFT-rg>`

`Oct   10 18 :36 :34 fleur Cluster.RGM.rgmd : [ID 148023 daemon.notice] method   <gds_monitor_stop> completed successfully for resource <cft-gds>,   resource group <CFT-rg>`

`Oct   10 18 :36 :34 fleur Cluster.RGM.rgmd : [ID 707948 daemon.notice] launching   method <gds_svc_stop> for resource <cft-gds>, resource group   <CFT-rg>, timeout <300> seconds`

`Oct   10 18 :36 :44 fleur Cluster.RGM.rgmd : [ID 148023 daemon.notice] method   <gds_svc_stop> completed successfully for resource <cft-gds>,   resource group <CFT-rg>`

`Oct   10 18 :36 :44 fleur Cluster.RGM.rgmd : [ID 707948 daemon.notice] launching   method <hastorageplus_stop> for resource <cft-disk>, resource   group <CFT-rg>, timeout <1800> seconds`

`Oct   10 18 :36 :44 fleur Cluster.RGM.rgmd : [ID 148023 daemon.notice] method   <hastorageplus_stop> completed successfully for resource <cft-disk>,   resource group <CFT-rg>`

`Oct   10 18 :36 :44 fleur Cluster.RGM.rgmd : [ID 707948 daemon.notice] launching   method <hafoip_stop> for resource <cft-ip>, resource group   <CFT-rg>, timeout <300> seconds`

`Oct   10 18 :36 :44 fleur Cluster.RGM.rgmd : [ID 148023 daemon.notice] method   <hafoip_stop> completed successfully for resource <cft-ip>,   resource group <CFT-rg>`

`Oct   10 18 :36 :44 fleur Cluster.RGM.rgmd : [ID 707948 daemon.notice] launching   method <hastorageplus_postnet_stop> for resource <cft-disk>,   resource group <CFT-rg>, timeout <1800> seconds`

`Oct   10 18 :36 :45 fleur Cluster.RGM.rgmd : [ID 148023 daemon.notice] method   <hastorageplus_postnet_stop> completed successfully for resource   <cft-disk>, resource group <CFT-rg>`

`Oct   10 18 :36 :46 fleur Cluster.Framework : [ID 801593 daemon.notice] stdout   : no longer primary for dsk/d16`

### Switch overs

Switch-over the service to the "fleur" node:   

`scswitch   –z –g CFT-rg –h fleur`
