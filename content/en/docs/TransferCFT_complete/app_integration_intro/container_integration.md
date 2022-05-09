{
    "title": "Integrate a containerized application",
    "linkTitle": "Containerized application integration",
    "weight": "200"
}This page is intended to help you understand the various ways to use Transfer CFT and associated applications in a container environment, providing a high level overview of the possible integration architectures.

You may want to review the two methods  for installing and operating a containerized {{< TransferCFT/suitevariablesTransferCFTName  >}}, using either [Docker Compose or Kubernetes with Helm](../../cft_intro_install/contanier_intro/install_container).

Kubernetes concepts
-------------------

****Nodes****

Kubernetes runs your workload by placing containers into Pods to run on Nodes. A node may be a virtual or physical machine, depending on the cluster. Each node contains the services necessary to run Pods, managed by the control plane.

Typically you have several nodes in a cluster; in a learning or resource-limited environment, you might have just one.

****Pods****

Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.

A Pod is a group of one or more containers, with shared storage/network resources, and a specification for how to run the containers. A Pod's contents are always co-located and co-scheduled, and run in a shared context. A Pod models an application-specific "logical host": it contains one or more application containers that are relatively tightly coupled. In non-cloud contexts, applications executed on the same physical or virtual machine are analogous to cloud applications executed on the same logical host.

Definitions are provided by Kubernetes at: [https://kubernetes.io/docs/concepts](https://kubernetes.io/docs/concepts/)

Producer/consumer application integration
-----------------------------------------

Your producer/consumer application integration with Transfer CFT is based on:

- File storage: Where and how files transferred by Transfer CFT are shared with the application
- Transfer triggering: How the application triggers a transfer request
- Post-processing: How Transfer CFT handles post-processing

### File storage

There are 3 possible file storage implementations when you have Transfer CFT as a container along with a producer/consumer application.

#### {{< TransferCFT/suitevariablesTransferCFTName  >}} is the application sidecar

The application and {{< TransferCFT/suitevariablesTransferCFTName  >}} run in the same pod but in different containers. Both the data produced/consumed by the application and {{< TransferCFT/suitevariablesTransferCFTName  >}} reside on a persistent volume attached to a node where the pod is running. The persistent volume supports ReadWriteOnce access mode and is a local volume (AWS ESB, GCEPersistentDisk, AzureDisk).

********Sidecar architecture                     ![](/Images/TransferCFT/pod1.png)********

#### {{< TransferCFT/suitevariablesTransferCFTName  >}} and the application run on different pods but on the same node

The application and {{< TransferCFT/suitevariablesTransferCFTName  >}} run on different pods on the same node. If using a local volume, both pods must run on the same node. Both the data produced and consumed by the application and {{< TransferCFT/suitevariablesTransferCFTName  >}} reside on a persistent volume attached to the node where the pods are running. The persistent volume supports ReadWriteOnce access mode, and is a local volume (AWS ESB, GCEPersistentDisk, AzureDisk).

********Two pods one node architecture            ![](/Images/TransferCFT/pod2.png)********

******** ********

<span id="__RefHeading___Toc2647_2515630742"></span>

#### {{< TransferCFT/suitevariablesTransferCFTName  >}} and the application run on different nodes

The application and {{< TransferCFT/suitevariablesTransferCFTName  >}} run on different nodes both of which support the **ReadWriteMany** access mode, where:

- The Transfer CFT data resides on a persistent volume that can be a shared volume (Ceph, GlusterFS, NFS, ...).
- The data produced and consumed by the application resides on a persistent volume that can be a shared volume (Ceph, GlusterFS, NFS, ...) or cloud storage (AWS S3, GCS). In this implementation, you configure the cloud storage at the flow deployment level, not in the product deployment.

********Two pods two nodes architecture                    ![](/Images/TransferCFT/pod3.png)********

<span id="__RefHeading___Toc2649_2515630742"></span>

####  Architecture versus file storage summary


| Architecture | Local volume | Shared volume | Cloud storage |
| --- | --- | --- | --- |
| 1. Same pod | yes | yes | yes |
| 2. Two pods on the same node | yes | yes | yes |
| 3. Two pods regardless of the running node | no | yes | yes |


### Transfer triggering

In a standard environment, on bare metal, or virtual machines, an application can trigger transfers using the following methods:

- Batch mode (CFTUTIL)
- REST API
- Folder monitoring

When using a container environment however, you cannot use batch mode since Transfer CFT is isolated in a container. Available methods include:

- REST API
- Folder monitoring

****Application invokes Transfer CFT REST APIs            ![](/Images/TransferCFT/trigger_restapi.png)****

 

 

****Folder monitoring              ![](/Images/TransferCFT/foldermonitoring_trigger.png)****

 

### Post-processing

The application and Transfer CFT run in different containers. Therefore, for post-processing Transfer CFT can interact with the application by:

- Invoking the application’s REST API using the cURL command in the post-processing script
- Invoking a Kubernetes job using the Kubernetes API in the post-processing script
- Storing a file in a directory monitored by the application

****Transfer CFT notifies the application using REST API          ![](/Images/TransferCFT/cft_container_app_post_processing.png)****

 

 

****The application monitors a directory                  ![](/Images/TransferCFT/foldermonitoring_container.png)****
