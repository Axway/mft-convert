---

    title: About Transfer CFT services in C
    linkTitle: Using services in C
    weight: 310

---
This section begins with this topic which provides information about using
the {{< TransferCFT/axwayvariablesComponentShortName  >}} services in C language. It also contains topics
that describe how to use the following services in
C language.

This page describes the Transfer
CFT programming interface. The programming interface is implemented by
the calling application module link, with the {{< TransferCFT/axwayvariablesComponentShortName  >}} interface function
modules.

The library of object modules supplied provides everything the programmer
can requires. This library also contains the file cftapi.h
to be included in the application using the {{< TransferCFT/axwayvariablesComponentShortName  >}} programming interfaces.

<span id="Call_Syntax"></span>

## Call syntax


| ****Syntax**** | rc = cftxx (verb,&amp;ptr,param) |
| --- | --- |
| Element | Definition |
| cftxx | <span >****cftai****</span>: simple Transfer CFT catalog querying services<br/> <span >****cftaix****</span>: extended {{< TransferCFT/axwayvariablesComponentShortName  >}} catalog querying services<br/> <span >****cftau****</span>: transfer services with syntax analysis<br/> <span >****cftac****</span>: transfer services without syntax analysis |
| **<span >verb</span>** | Service requested |
| ptr | Address of the internal control block |
| param | Parameters specific to the requested service |
| rc | Return code |


### Return codes

The return codes are returned by the programming interfaces in the form
of mnemonics.

> **Note**
>
> It is strongly recommended that you test the return codes of services
> provided by the Transfer CFT programming interfaces through mnemonics.
> The corresponding values may change without notice.

The return codes are listed in the cftapi.h source file.
