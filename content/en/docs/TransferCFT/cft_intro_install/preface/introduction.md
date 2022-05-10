---
    "title": "Introduction",
    "linkTitle": "Introduction",
    "weight": "150"
---
This document describes how to install Transfer CFT {{< TransferCFT/headerfootervariableshfversion  >}} in a HP NonStop environment. HP NonStop exists on several hardware architectures, such as Itanium and Intel x86. On each of these runs a version of the HP NonStop OS, which is platform dependent. Therefore, there are two different versions of Transfer CFT for HP NonStop:

- Transfer CFT for HP NonStop Itanium, which runs specifically on HP NonStop for Itanium processors.
- Transfer CFT for HP NonStop x86, which runs specifically on HP NonStop for Intel x86 processors.

> **Note**
>
> Note: Transfer CFT for HP NonStop, globally stands for both versions of this product.

The NonStop OS basically consists of a Guardian layer, which is a lower level of the operating system, and an OSS layer that rests on top of Guardian and implements a Unix-like interface for the underlying Guardian layer. This document may contain references to Guardian, NonStop, or OSS, all of which refer to the same overall HP NonStop platform.

> **Note**
>
> Note: To accommodate changing product versions, the convention &lt;version&gt; is used in place of the actual product version in examples and lists. You should replace this with the actual value. Using the same logic, you can replace &lt;os&gt;, &lt;arch&gt;, and &lt;xx&gt; with your target platform details. For example, Transfer_CFT_&lt;version&gt;_&lt;os&gt;-&lt;arch&gt;-&lt;xx&gt; becomes Transfer_CFT_3.2.x_hp_nonstop_oss-ia64-32 when referring to Transfer CFT V3.2.x for HP NonStop OSS-IA64-32bits.

Delivered components
--------------------

Axway delivers the Transfer CFT product for NonStop OS as an electronic software download, which is available from the Axway support site at [support.axway.com](http://www.support.axway.com/).

Prerequisites
-------------

Perform any prerequisites operations using the user account intended for the Transfer CFT installation in the OSS environment.

### Disk space requirements

The following disk space calculations are the minimum requirements for a test environment. This calculation includes space required for service packs and a basic instance of the product. In a production environment, you must evaluate and modify the required disk and memory space per your requirements.

- Disk space: 430 MB for Transfer CFT for HP NonStop Itanium
- Disk space: 400 MB for Transfer CFT for HP NonStop x86

### Pseudo Random Number Generator Daemon

Transfer CFT needs to access a random device for its security operations. Since HP NonStop OSS is not equipped with such a device, your system administrator should activate this daemon under SUPER.SUPER user. The start command resembles the following:

```
/usr/coreutils/sbin/prngd -c /usr/local/etc/prngd/prngd.conf /etc/egd-pool
```

### Additional requirements

- Transfer CFT for HP NonStop Itanium is built on an J-series TNS/E Itanium machine. The operating system Release Version Update (RVU) used is J06.21.
- Transfer CFT for HP NonStop x86 is built on an L-series TNS/X x86 machine. The operating system Release Version Update (RVU) used is L15.08.

> **Note**
>
> Note: Even though it may be possible to run Transfer CFT for HP NonStop on an older RVU, we recommend that you run the product on a more recent RVU (or similar to the following) to ensure that the product stability and performance is not impaired by an old-system component. More information is available in the HP Release Version Update Compendium manuals.

- The Open System Services (OSS) subsystem should be installed.

{{% TransferCFT/snippets/install_elua%}}

Limitations
-----------

- CreateProcessAsUser is not supported on HP NonStop.
- The FACTION parameter ARCHIVE option (for CFTSEND, SEND) is not supported with Guardian files.
