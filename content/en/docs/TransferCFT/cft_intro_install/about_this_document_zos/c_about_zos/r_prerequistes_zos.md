---
title: "System requirements"
linkTitle: "System requirements"
weight: 150
---This section outlines the minimum requirements to install Transfer CFT in a z/OS environment.

This chapter describes the system requirements for {{< TransferCFT/axwayvariablesComponentShortName  >}}. System requirements can change when {{< TransferCFT/axwayvariablesCompanyName  >}} releases service packs and patches for a product version. Therefore, you may want to refer to the *[{{< TransferCFT/suitevariablesDocNameSUITESupportedPlatforms  >}}](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en/resource/Axway_Products_SupportedPlatforms_allOS_en.pdf)* document. The document lists the supported operating systems, databases, web servers, and browsers.

## Hardware

### About the z/server processor

You can install Transfer CFT using an IBM supported version of z/OS. Transfer CFT is located in a permanent address space and can process several simultaneous transfer demands locally, which create and manage appropriated tasks.

### Input/output units

Sufficient disk space is required for the Transfer CFT object modules, procedures, and examples.

## Software

Transfer CFT z/OS uses the following software products and versions:

* TCP/IP network
* Execution environment:
    *   Language environment for the z/OS version
* Programming interfaces and EXITS:
    *   COBOL
    *   C
    *   ASM

### Platform requirements

Transfer CFT requires the following z/OS version depending on the JES component used:

* JES2 requires at a minimum z/OS 2.1
* JES3 requires at a minimum z/OS 2.2

## End User License Agreement

You should read and accept the End User License Agreement (EULA) prior to installing Transfer CFT. The EULA file is in the directory where you decompressed the Transfer CFT package.

## OpenMVS requirements

You must define the OpenMVS (OMVS) segment for each user if they need to access the z/OS USS resources or access TCP/IP communication services. You can use CA ACF2, CA Top Secret, or IBM RACF to enable this access. The OMVS segment is required, for example, in the following cases:

* To access TCP/IP services
    *   A user that starts CFTMAIN and/or the Transfer CFT UI server
    *   A user implementing a synchronous API as a batch API, or from CFTUTIL
    *   The am.type=passport option is selected
* To access USS resources
    *   When the option userctrl=yes when transferring HFS files
    *   A user that starts CFTMAIN and/or the Transfer CFT UI server

<span id="SMP/E"></span>

## SMP/E grant user resource permissions

When performing an SMP/E installation, the user performing the installation requires READ access on the RACF CSFSERV CLASS. If the SMP/E CFT JCL "$C10RECV" executes in error,  the user does not have the correct access rights and the following message displays:

`GIM43501S ** THE CALL TO THE CSNBOWH SERVICE FAILED WHEN PROCESSING xxxx.SMPMCS.pax.Z. THE RETURN CODE WAS '00000008'X AND THE REASON CODE WAS '00003E80'X.`

## TSO requirement

Check that the user that starts Copilot and Transfer CFT has a TSO segment in their profile.

****TSO version****

The user ID for the user that starts Copilot and Transfer CFT has a 7-characters maximum when using z/OS 2.2 or lower. Therefore, a user with an 8-character user ID cannot use TSO with a z/OS version lower than v2.3.

****Related topics****

* [About Transfer CFT z/OS](../)
* [Installation overview]()
* [About the installation environment](../r_envi_file_size_format)
