{
    "title": "About kernel customization",
    "linkTitle": "Customize the kernel",
    "weight": "170"
}Depending on the operating system, some customization may
be mandatory to ensure that {{< TransferCFT/axwayvariablesComponentShortName  >}} operates correctly, and to maintain
the overall system performance levels.

Kernel customization is not mandatory for installation, but is needed to run {{< TransferCFT/axwayvariablesComponentShortName  >}}.

This section describes the
kernel parameters that you need to upgrade to ensure that the
{{< TransferCFT/axwayvariablesComponentShortName  >}} operates correctly, and is comprised of the following topics:

- [Why
    customize the kernel?](why_customize_the_kernel_)

System specific
customization procedures are described for the following systems:

- [HP-UX](customizing_hp_ux)
- [Linux](customizing_linux)
- [Solaris](customizing_solaris)

> **Note**
>
> Note: No additional steps are necessary to modify the IBM AIX kernel as the ODM (Object Data Manager) manages this.

For additional information on customization issues,
contact your system administrator or the manufacturer's support service.

Customization details
---------------------

Kernel customization concerns one or more of the following aspects:

- Size
    of the global shared memory segment: modification **recommended**
    but not mandatory
- Message
    queue depth: modification **mandatory**
- Memory
    allocated to TCP: modification **mandatory**
- Number
    of files used by a process: modification **mandatory**

Depending on the operating system, some customization may
be mandatory to ensure that {{< TransferCFT/axwayvariablesComponentShortName  >}} operates correctly, and to maintain
the overall system performance levels.

Note:

- The kernel customization
    requires an in-depth knowledge of UNIX. To make the recommended changes,
    you must have super-user privileges (*root*). An incorrect setting
    could seriously affect the integrity of your system.
- The figures indicated
    are based on a standard system, which is the default configuration
    used when the operating system was installed. If you have already changed
    a specific setting for another application, the current parameter value
    must be increased by the one recommended for {{< TransferCFT/axwayvariablesComponentShortName  >}}.
