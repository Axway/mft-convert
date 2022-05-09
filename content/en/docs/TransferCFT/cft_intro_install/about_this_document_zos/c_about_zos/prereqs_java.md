{
    "title": "Java",
    "linkTitle": "Java  ",
    "weight": "170"
}{{< TransferCFT/suitevariablesSecureRelayName  >}} prerequisites
---------------------------------------------------------------------

When using Secure Relay, Java 8 must be installed in the same environment as the Transfer CFT installation. The Master Agent is managed, but the Router Agent can be in another environment.

Java requirements for Transfer CFT z/OS
---------------------------------------

Begin by checking if the z/OS system already has a Java environment available. {{< TransferCFT/suitevariablesSecureRelayName  >}} requires a SDK Java Version 8. If you do not have this Java level installed, visit the IBM Java website for z/OS.

1. Select, download, and install the appropriate Java SDK for your environment.
1. Check if the JZOS Batch Launcher is installed in your environment.
    -   JZOS is a set of tools that enhances the ability for z/OS Java applications to run in a traditional batch environment and/or access z/OS system services.
    -   The function consists of a load module and a sample start proc put into an appropriate PROCLIB.

To identify if your current system has JZOS available:

1. Check the system dataset "SYS1.SIEALNKE" for a member JVMLDMxx where xx is the version and release of the Java you are using, for example:  
    ```
    JVMLDM80 for V8.0.6.10 31-bit SDK
    ```
1. Check in the system PROCLIBs for a member JVMPRCxx where xx is the version and release of the Java you are using, for example:  
    ```
    JVMPRC80 for V8.0.6.10 31-bit SDK
    ```

If these modules are present, then JZOS is available. If it is not available:

1. Go to the IBM JZOS Java Launcher and Toolkit overview page.
1. Choose, download, and install the appropriate JZOS Batch Launcher and Toolkit Installation (we use the JZOS Batch Launcher and Toolkit function for 31-bit Java 8.0.6.10 SDK in our examples).

> **Note**
>
> IBM introduced a specialty processor for running Java applications called the zSeries Application Assist Processor, also known as zAAP. This type of processor is an optional feature in the System z9 hardware.

Once installed and enabled, zAPP allows the customer to benefit from an expansion of the system’s CPU capacity at a relatively low cost, if the workload that is run is based on Java.
