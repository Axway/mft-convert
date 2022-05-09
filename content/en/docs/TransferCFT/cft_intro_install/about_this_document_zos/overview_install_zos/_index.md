{
    "title": "Installation",
    "linkTitle": "Installation",
    "weight": "140"
}This section provides an overview of the Transfer CFT z/OS installation procedure. We recommend that you begin by familiarizing yourself with the [system requirements](../c_about_zos/r_prerequistes_zos), if you have not already done so.

The steps for creating the distribution environment differ depending on if you are performing an [SMP/E]() or [non-SMP/E]() installation. After creating the distribution environment and instance, [customize](t_customize_instance_zos) the files used for installation. You can then either use the [standard installation](zos_auto_install_a05all), which is a compilation of all Jobs required for installation, or use the [advanced installation](), which allows you to execute each installation Job individually. Note that even if you use the standard installation, you can still execute individual advanced installation jobs as needed.

Once you verify the installation, continue on to [post-installation](../t_start_servers_jobs_zos) for a standalone installation, or go to [multi-node installation](../c_multinode_zos) instructions to implement a high-availability installation.

![flow showing installation options](/Images/TransferCFT/install_overview_zos.png)

****About the distribution and instance environments****

The Transfer CFT installation creates the following environments:

- Distribution environment: Contains the files extracted from the delivery. These files are necessary to create the customized target environment.

<!-- -->

- Instance environment: An instance environment is an environment customized for the user. You can create more than one instance environment.

The act of installing Transfer CFT creates a distribution environment that contains product binaries and templates. Do no store any personal files in this environment, or modify existing files in this environment, as they are erased during updates.

> **Note**
>
> Note: You use the distribution environment to create the instance environment, which is your target (runtime).

The directories that are installed during the installation procedure are described in [Verify your installation](../post_install_zos).
