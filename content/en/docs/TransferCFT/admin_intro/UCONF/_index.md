---

    title: About the unified configuration tool (UCONF)
    linkTitle: Set UCONF parameters
    weight: 220

---
Configurable {{< TransferCFT/suitevariablesTransferCFTName  >}} parameters fall into two basic categories, definable objects or UCONF values. For some Transfer CFT parameters, you can use either method to define parameter values.

When using {{< TransferCFT/suitevariablesCentralGovernanceName  >}} to manage {{< TransferCFT/axwayvariablesComponentShortName  >}}, some parameter values are specific to UCONF and modifiable only through the {{< TransferCFT/suitevariablesCentralGovernanceName  >}} interface or using the UCONF utility as described in this section.

## What is the unified configuration tool?

{{< TransferCFT/axwayvariablesComponentShortName  >}} features
an easy-to-use configuration tool, UCONF, to standardize and merge functioning for all platforms. This interface enables you to
makes technical parameter value modifications using either CFTUTIL or
the Copilot UI.

## About configuration parameter values

You can enter any string as a UCONF value. Additionally, the values that
you enter can refer to other UCONF values or even environment variables.
For example:

- $(IDENTIFIER.IDENTIFIER...):
    references another UCONF parameter value and contains a period
    in the value name
- $(IDENTIFIER):
    refers to an environment variable

You can see in the above example that the difference between the UCONF value and the environment
variable is a period “.” in the value name. The UCONF parameter values are then expanded at runtime.

### Converting parameters

Several UCONF parameters are automatically converted, or *translated*, when you enter them
as a fname, path,  or
directory (dir). This translation enables uniform file specification across
platforms:

- a/b/c becomes a\\b\\c
    in Windows
- a\\b\\c becomes a/b/c
    in UNIX

## UCONF directory

The [UCONF parameters topic](uconf_directory) contains a complete listing of all options that you can manage using the unified configuration settings. Additionally it provides the possible and default values.

### Flags

**UCONF flags legend**

- EXPERT: Extra care must be taken; only advanced users should change this value.
- RECONFIG: Can be changed dynamically with a CFTUTIL RECONFIG type=UCONF, and a notification is displayed in the LOG.
- IRECONFIG: Can be changed dynamically with a CFTUTIL RECONFIG type=UCONF, but no notification is displayed in the LOG.
- RUNTIME MUTABLE READ\_ONLY: Cannot be changed by a user.
- EXPERIMENTAL: Unsupported feature.
- OBSOLETE: No longer used.

> **Note**
>
> By default, all UCONF parameters are static and require a restart. Only parameters with the RECONFIG or IRECONFIG flags are dynamic; for these dynamic parameters only you can use the reconfig command and no restart is required.

### UCONF data

When you install Transfer CFT, the <span class="code">`home`</span> directory is created and populated under the <span class="code">`Transfer_CFT`</span> installation directory. This <span class="code">`home `</span>directory contains installation libraries, binaries, and templates. Do not store any personal files in the <span class="code">`home `</span>directory, as they are erased during updates.

The UCONF data are stored in both a dictionary located in the <span class="code">`Transfer_CFT>home `</span>directory, and in a runtime file in the<span class="code">` Transfer_CFT>runtime>data`</span> directory. You should not modify the default values stored in the <span class="code">`home `</span>UCONF dictionary.

****Related topics****

- [Using UCONF in CFTUTIL](uconf_w_cftutil)
- [UCONF parameters](uconf_directory): complete listing
