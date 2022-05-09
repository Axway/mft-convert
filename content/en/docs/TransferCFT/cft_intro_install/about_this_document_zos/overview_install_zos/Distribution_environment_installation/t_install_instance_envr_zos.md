{
    "title": "Create a Transfer CFT instance environment",
    "linkTitle": "Create a Transfer CFT instance ",
    "weight": "190"
}The concept of a target environment has been replaced by what is now referred to as a Transfer CFT **instance**, or run-time environment. This procedure describes how to install the Transfer CFT instance environment.

If during installation you created both the distribution environment and the Transfer CFT instance environment, you may go directly to [Customize the instance environment](../../t_customize_instance_zos). Otherwise, you must first create your runtime environment.

> **Note**
>
> Note: You require an installed Distribution environment before you can install the instance environment. See Distribution environment installation.

****Prerequisites****

{{< TransferCFT/axwayvariablesComponentLongName  >}} requires two additional libraries LE (Language Environment), SCEELIB and SCEECPP, which are used in the (B20 LINK) link step.

****Procedure****

1. Make a copy of the JCL. It is recommended that you work from a copy.
1. From the **distlib.INSTALL** library, customize and submit the **J2IICFT**.

This procedure:

- Creates a Transfer CFT instance run-time environment
- Copies certain Transfer CFT distribution environment elements to the target environment

After creating the target environment, leave the distribution environment in its original state. You should now work exclusively in the target environment.

Next, execute the installation JCLs as described in the table [JCL installation steps]().
