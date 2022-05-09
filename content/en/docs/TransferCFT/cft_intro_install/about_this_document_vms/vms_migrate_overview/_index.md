{
    "title": "Overview",
    "linkTitle": "Migrate, upgrade, update",
    "weight": "190"
}This section is designed to assist administrators or users who are tasked with upgrading or migrating from an existing Transfer CFT version to Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}}.

The Transfer CFT versions that are available to migrate include 2.4, 2.7, 3.0.1 and 3.1.3.

> **Note**
>
> Note: If you are migrating from a previous version of Transfer CFT, be sure to check the Release Notes for new features as well as deprecated features and supported platforms per release.

Migrate, update, and upgrade
----------------------------

### About updates

An update brings Transfer CFT up-to-date with a patch or service pack offering fixes and minor enhancements. For example, you can update a Transfer CFT 3.1.3 SP3 to Transfer CFT 3.1.3 SP8.

### Migration options

The following methods are available for updating your Transfer CFT product version.

- Upgrade (existing)

<!-- -->

- Manual migration

### About upgrades

An upgrade is the process of updating to a newer, enhanced version of the software.

For more information, go to [Manually upgrade a Transfer CFT 3.0.1 or 3.1.3 multi-node installation]().

This mode has the following advantages:

- Allows you to update in the same location

<!-- -->

- You can perform this upgrade yet still revert to the previous state if needed

<!-- -->

- Scripts and APIs remain intact and only require a recompilation for the APIs

### About migrations

A migration means that an initial Transfer CFT is installed in a directory that is not removed or overwritten by the procedure.

This mode has the following advantages:

- The new installation occurs in a new location, and the existing configuration in the existing Transfer CFT environment is not affected

<!-- -->

- You can choose to use either of the versions, if needed, in case of an issue with one of the installations

> **Note**
>
> Note: Configuration and data, such as the catalog, are in two separate locations and data are not shared.

This mode has the following restriction:

- You must copy scripts and APIs from the previous version to the new installation.

### About manual migrations

The manual migration procedure, used to migrate an existing Transfer CFT to Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}}, is described in this document.

The general procedure for migrating from a previous version of Transfer CFT to Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}} is:

1. Export existing information from the previous version. Details vary depending on the existing Transfer CFT version.
1. Import the exported information into Transfer CFT {{< TransferCFT/PrimaryTransferCFTversionlong  >}}.

This mode has the following advantages:

- Because it is manual, you can customize as needed.

<!-- -->

- You can manually migrate from versions older than version 2.7.x.

### Check product details

You can use the display command to check the version, or product details prior to updating.

The display command lists {{< TransferCFT/axwayvariablesComponentLongName  >}} product information.

```
CFTUTIL about
```

### More information

If you encounter issues when migrating Transfer CFT, contact Axway Sphere at [support.axway.com.](https://support.axway.com/)
