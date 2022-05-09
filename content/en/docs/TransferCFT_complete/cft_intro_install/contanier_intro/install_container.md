{
    "title": "Installing Transfer CFT in a container",
    "linkTitle": "Installing Transfer CFT in a container",
    "weight": "170"
}There are two methods  for installing and operating a containerized {{< TransferCFT/suitevariablesTransferCFTName  >}}, use either Docker Compose or Kubernetes with Helm. This page describes prerequisites and installation instructions needed to help you install Transfer CFT in a container.

The information in this page may be supplemented, corrected, or even contradicted by the README.md file supplied with the product, in which case the README.md file takes precedence.

Prerequisites
-------------

To get started, you require:

- For Docker-Compose:
    -   Docker 17.11 and higher
    -   Docker-Compose 1.17.0 and higher
- For container orchestration:
    -   Kubernetes 1.14 and up
    -   OpenShift 3 and 4
    -   Helm 2.16 and higher, Helm 3 and higher

Beyond the prerequisites listed above, the installation instructions for each container option provide links to their own prerequisites.

Installation procedures
-----------------------

Depending on which method you plan to use, follow the corresponding link to the appropriate installation instructions.


| Element | Instructions |
| --- | --- |
| Docker Engine  | [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)  |
| Docker-Compose  | [compose/install/](https://docs.docker.com/compose/install/)  |
| Kubernetes  | [https://kubernetes.io/docs/tasks/tools/install-kubectl/](https://kubernetes.io/docs/tasks/tools/install-kubectl/)  |
| Helm  | [https://helm.sh/docs/intro/install/](https://helm.sh/docs/intro/install/)  |
| Git  | [https://git-scm.com/book/en/v2/Getting-Started-Installing-Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) |


Download the Transfer CFT repository
------------------------------------

Begin by downloading or cloning the Transfer CFT repository. To download, navigate to <https://github.com/Axway/docker-cft> and click **Code** &gt; **Download Zip**. Alternatively, to clone using Git:<span id="gitcontainertest"></span>

```
git clone <https://github.com/Axway/docker-cft.git>
cd docker-cft
```

The repository contains several README.md files that correspond to each sub-module, and a CHANGELOG.md file that describes all changes to the current version.

****READMEs****

Use the following links to find more information in the associated README:

- Transfer CFT Docker changelog: <https://github.com/Axway/docker-cft/blob/master/CHANGELOG.md>
- Organizational repository information: <https://github.com/Axway/docker-cft/blob/master/README.md>
- How to build a Docker image: <https://github.com/Axway/docker-cft/blob/master/docker/README.md>
- How to use Docker-Compose: <https://github.com/Axway/docker-cft/blob/master/compose/README.md>
- How to use Kubernetes with Helm:<https://github.com/Axway/docker-cft/blob/master/helm/transfer-cft/README.md>

Download the Transfer CFT Docker image
--------------------------------------

You can download the {{< TransferCFT/suitevariablesTransferCFTName  >}} Docker image from the [Axway support](http://support.axway.com/) site. From the **Downloads** page, search Transfer CFT and click to download the `Transfer_CFT_<version>_DockerImage_allOS_<BN>.zip`.

> **Note**
>
> You must accept the installation General Terms and Conditions to run Transfer CFT in a container.
