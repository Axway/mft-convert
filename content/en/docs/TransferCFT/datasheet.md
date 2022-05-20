---
title: "System support"
linkTitle: "System support"
weight: 50
---This page provides basic operating system, supported feature, and file system information. Always check the [Supported Platform Guide](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en/resource/Axway_Products_SupportedPlatforms_allOS_en.pdf) for the latest updates.

## Platforms

{{< TransferCFT/axwayvariablesComponentShortName  >}} is deployed on the operating systems and browsers listed in this information sheet.

### Operating systems for x64

* CentOS 6, 7
* Debian 7, 8, 9
* Oracle Linux 6, 7
* Oracle Solaris 10, 11
* Red Hat Enterprise Linux 6, 7, 8
* SUSE Linux Enterprise Server 11, 12, 15
* Ubuntu 16.04, 18.04, 20.04
* Windows 10
* Windows Server 2012, 2012 R2, 2016, 2019

### Operating systems for Z

* Red Hat Enterprise Linux 5, 6, 7, 8
* SUSE Linux Enterprise Server 11
* z/OS 2.1, 2.2, 2.3, 2.4

### Operating systems for Power

* IBM AIX 7.1, 7.2

### Operating systems for SPARC

* Oracle Solaris 10, 11 for SPARC

### Additional operating systems

* HP-UX 11.31 for PA-RISC
* HP-UX 11.31 for ia64
* HP NonStop H and J series for ia64
* HP NonStop L series for x86-32
* IBM i 7.2, 7.3, 7.4
* OpenVMS 8.3, 8.4 for Alpha and ia64

<span id="Virtuali"></span>

## Virtualization support

* Amazon Elastic Compute Cloud (EC2) instance
* Google Cloud Platform (GCP) VM instances
* Microsoft Azure VM instances
* VMware EXSi Virtual Machine
* Other: Axway provides support for Axway products running in a virtual environment in a manner identical to Axway products running on any other major x86-based system. If, however, Axway suspects that the virtualization layer is the root cause of an incident, then the customer is required to contact the appropriate virtualization support provider to resolve the virtualization issue.

## Container support

* Docker Compose
* Kubernetes 1.14 and higher
* OpenShift 3 and 4
* Helm 2.16 and higher, Helm 3 and higher

## Web browsers

Your browser-based experience is best when using the latest version of one of the following browsers:

* Chrome
* Microsoft Edge
* Firefox (keyboard shortcuts are not enabled)

## File systems for multi-node


| Operating system  | Supported  | Unsupported  |
| --- | --- | --- |
| AIX  | GPFS, NFSv4*  | NFSv3, CXFS, VeritasSF  |
| HP-UX  | NFSv4*  | NFSv3, CXFS, VeritasSF  |
| Linux-x86  | GPFS, NFSv4*  | NFSv3, CXFS, ACFS, OCFSv1, OCFSv2, QFS, VeritasSF  |
| OpenVMS  | RMS  |   |
| Solaris  | NFSv4*  | NFSv3, CXFS, QFS, VeritasSF  |
| Windows-x86  | SMB/CIFS, GPFS  | CXFS, NFS  |
| z/OS  | Sharing DASD across Sysplex  |   |


\*References to NFSv4 imply any version of NFSv4. All NFSv4 minor versions are supported, for example version 4.2.

## Cloud storage

**Available on Unix and Windows**

* Amazon Web Services S3 (Amazon Simple Storage Service)
* Ceph Storage Cluster using the Ceph Object Gateway and its S3 compatible API
* Google Cloud Storage

## Java

If you are implementing either {{< TransferCFT/suitevariablesTrustedFileName  >}} or {{< TransferCFT/suitevariablesSecureRelayName  >}} with {{< TransferCFT/suitevariablesTransferCFTName  >}}, ensure that you have Java JRE 8 (1.8.0 u272 or higher) installed. When using {{< TransferCFT/suitevariablesSecureClientName  >}}, additionally please check that the Master Agent and Router Agent are using the same version of Java.

## Standard defaults

The Internet Assigned Numbers Authority (IANA) reserves the TCP ports 1761-1768 for {{< TransferCFT/axwayvariablesComponentShortName  >}}. For more information, refer to: [www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml?&page=31).


| Component  | Port |
| --- | --- |
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

> **Note**
>
> In this document, the terms Transfer CFT OS/400 and Transfer CFT IBM i may be used interchangeably, as well as VMS with OpenVMS, and MVS with z/OS.

## Third party licenses

Available on the Axway support site, at [support.axway.com](https://support.axway.com/en/documents/document-details/id/1441679).
