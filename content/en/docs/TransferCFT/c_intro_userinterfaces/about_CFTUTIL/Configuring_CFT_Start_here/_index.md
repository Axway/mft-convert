{
    "title": "About CFTUTIL configuration operations",
    "linkTitle": "Object configuration",
    "weight": "250"
}This topic
gives an overview of the Transfer CFT configuration and object conventions.

About the Transfer CFT configuration
------------------------------------

The Transfer CFT initially must have an ******initial******
default configuration in order to startup. After the initial startup the
******ongoing****** Transfer
CFT configuration feature enables you to modify parameters while Transfer
CFT is running. The modifications are taken into account immediately without
having to shut down the Transfer CFT for most object modifications. When
necessary, a message appears instructing you to shut down to confirm configuration
changes.

You can modify the initial and the ongoing configuration either via
command line or the user interface.

<span id="About_parameter_setting_commands"></span>

About parameter setting commands
--------------------------------

During the parameter setting phase, the data describing the Transfer
CFT environment are entered by:

- Command lines interpreted
    by the CFTUTIL utility
- Transfer CFT user interface

The PARAMETERS and PARTNERS index files are updated when the Transfer
CFT parameters are updated.

Transfer CFT is delivered with parameter setting examples, in the form
of source code which can be interpreted by CFTUTIL. During operation,
at least some of the parameter setting commands for the operating configuration,
are kept in a reference source file.

When you set Transfer CFT parameters using the Copilot user interface,
the data entry screens display the same information as is included in
the commands interpreted by CFTUTIL.

For more information on CFTUTIL, refer to Using the CFTUTIL interface:
Start here.

> **Note**
>
> Note: The format that you use to enter commands in CFTUTIL is operating
> system dependent. For more information, refer to the Operations Manual
> that corresponds to your operating system.
