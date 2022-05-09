{
    "title": "Roll back a service pack or patch",
    "linkTitle": "Rollback a service pack or patch (non-SMP/E)",
    "weight": "270"
}The following procedure enables you to rollback after applying a SP or patch to its previous state. This is useful in the case of an incident during the application of a PTF, or when testing the patch validation.

Use the A13RBACK
----------------

Use the A13RBACK to restore the executable file library, USS Copilot and USS {{< TransferCFT/suitevariablesSecureRelayName  >}} environment. This JCL executes the following three JCLs:

- A13RSTOR: Restores the Transfer CFT Load library.
- A13UCOPR: Restores the Transfer CFT USS Copilot environment.
- A13UXSRR: Restores the Transfer CFT USS {{< TransferCFT/suitevariablesSecureRelayName  >}} environment.

Before submitting the JOB configure the following JCL in automatic mode:

..INSTALL(A13RSTOR) &gt;&gt; ID='AUTO'

..INSTALL(A13SDEL) &gt;&gt; ID='AUTO'

..INSTALL(A13UCOPR) &gt;&gt; ID='AUTO'

..INSTALL(A13UCOPD) &gt;&gt; ID='AUTO'

..INSTALL(A13UXSRR) &gt;&gt; ID='AUTO'

..INSTALL(A13UXSRD) &gt;&gt; ID='AUTO'

- Stop Transfer CFT and {{< TransferCFT/suitevariablesCopilotName  >}}.
- Set the parameter 'ID' with the last six characters of the patch id. For example, 0E0500 for the patch id CF0E0500.
- Submit the JCL.

> **Note**
>
> Note: This operation is displayed in the distlib.LOG file.
