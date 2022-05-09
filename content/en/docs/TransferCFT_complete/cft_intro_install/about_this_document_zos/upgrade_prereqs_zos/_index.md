{
    "title": "Update, upgrade, or migrate",
    "linkTitle": "Update, upgrade, or migrate ",
    "weight": "180"
}Before you start
----------------

This section describes how to upgrade or migrate to Transfer CFT {{< TransferCFT/axwayvariablesReleaseNumber  >}}. Additionally, it describes the two methods for updating {{< TransferCFT/axwayvariablesComponentLongName  >}} z/OS and provides links to detailed information.

See the section [System requirements](../c_about_zos/r_prerequistes_zos) for details on system prerequisites.

> **Note**
>
> Note: You should not modify the CSD files (library XMLLIB: members CSDCFT and CSDCG) or the UCONF dictionary (library UPARM: member DEFAULT), as these items are updated in the instance (runtime) during the following procedures.

Update procedure
----------------

Applying a service pack (or patch) updates the product without changing the version. Typically it provides corrections to known issues, and may add minor enhancements. There are two supported methods to apply a service pack on {{< TransferCFT/axwayvariablesComponentLongName  >}} z/OS:

- [Update the Transfer CFT instance with the maintenance identifier (SMP/E)](c_update_zos/maintenance)
- [Update or apply a service pack (non-SMP/E)](c_update_zos/t_install_patch_zos)

Upgrade procedure
-----------------

This procedure involves a change in product version and the replacement of binary artifacts. See [Upgrade](upgrade) for details.

<span id="Upgrade_or_migrate_procedures"></span>

Prerequisites
-------------

### Important information before performing a migration procedure

- You must update your {{< TransferCFT/axwayvariablesComponentShortName  >}} to the most recent service pack version.
- Backup {{< TransferCFT/axwayvariablesComponentShortName  >}} before beginning an upgrade or migration procedure.
- Before beginning the upgrade or migration procedure stop the existing version of Transfer CFT and the UI server. (Meaning you must stop all cluster nodes as a database migration occurs when performing an upgrade.)

### About license keys

- You require a new license key if you are migrating from a version 2.x {{< TransferCFT/axwayvariablesComponentShortName  >}} to a version 3.x.
- For details on how to apply or update a license key, and the new license key location, see the section **Apply a license key**.

> **Note**
>
> Note: You require as many keys as instances of Transfer CFT running at same time. For example, two Transfer CFT instances cannot run at the same time, on the same server, using the same license key.
