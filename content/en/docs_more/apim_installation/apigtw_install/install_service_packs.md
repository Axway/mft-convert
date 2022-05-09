{
"title": "Update API Gateway with a service pack or patch",
"linkTitle": "Update API Gateway",
"weight":"36",
"date": "2019-10-07",
"description": "Apply service packs or patches to API Gateway components."
}

## Apply a service pack or patch

To apply a service pack or patch to an existing installation of API Gateway, follow these general guidelines:

1. Stop any Node Managers and API Gateway servers.
2. Back up your existing installation. For more information on backing up, see [API Gateway backup and disaster recovery](/docs/apim_administration/apigtw_admin/manage_operations/#api-gateway-backup-and-disaster-recovery).
3. Download the service pack or patch and the associated *Readme* from <https://support.axway.com>.
4. Review the *Readme* for any specific installation instructions (for example, backing up any customized files used by API Manager or third-party tools).
5. Unzip and extract the service pack or patch. A service pack or patch contains new API Gateway binaries and does not overwrite the existing API Gateway configuration.
6. Restart the Node Managers and API Gateway servers.
7. To verify that the service pack or patch have been installed correctly, run the `managedomain --version` command.

## Resolve patch validation issues

You can use the `managedomain --version` command to list and validate the patches installed. This command uses the information in the in the `META-INF/<patch>.id` file to validate the patch. For more information on running the `managedomain --version` command, see [Find your installed version and list patches using managedomain](/docs/apim_administration/apigtw_admin/trblshoot_get_help/#find-install-version).

A patch that validates successfully is listed with no messages. If patch validation fails, a status message is displayed for each patch entry that failed to validate in the following format:

```
<status>: <patch_entry>: <message>
```

The possible message statuses are:

* **Error**: An error has been detected for the `<patch_entry>`. You must take some action to fix the error.
* **Info**: An informational message about the `<patch_entry>`. This does not usually require any corrective action.
* **Warning**: A warning message about the `<patch_entry>`. Warnings indicate potential problems which might require you to take some action.

The `<patch_entry>` indicates the file or directory within the patch to which the message applies.

Some common messages, along with descriptions and suggested actions, are detailed in the following tables.

### Cannot validate, no checksum available

* **Status**: Info
* **Message** Cannot validate, no checksum available
* **Description**: The `<patch_entry>` does not have a checksum value assigned in the `META-INF/<patch>.id` file.
* **Action**: No action required if the `<patch_entry>` is a directory.

### Content changed

* **Status**: Warning
* **Message**: Content changed
* **Description**: The `<patch_entry>` checksum value differs from the one configured in the `META-INF/<patch>.id` file. This indicates that the file on disk has changed since the patch was installed.
* **Action**: No action is required if the `<patch_entry>` indicates a configuration file that you have customized after patch installation.
If the `<patch_entry>` does not indicate a configuration file that you have customized, check if there are two patches installed that patch the same file. You might be able to remove one of the patches (unless it also patches other files). If this is not the case, reinstall the patch.

### File not found

* **Status**: Error
* **Message**: File not found
* **Description**: An expected `<patch_entry>` cannot be found in the installation directory. This might indicate a partially removed patch, for example, a patched JAR file has been removed, but not the related `META-INF/<patch>.id` file.
* **Action**: If you meant to delete the patch, remove the patch `.id` file to delete it completely. If you did not mean to delete the patch, reinstall it.

### Malformed file

* **Status**: Error
* **Message**: Malformed file
* **Description**: Either a `<patch_entry>` is misconfigured with more than one checksum value or the `META-INF/<patch>.id` file is malformed.
* **Action**: Reinstall the patch, as the `.id` file might have been corrupted in some way.

### Unexpected file

* **Status**: N/A
* **Message**: Unexpected file
* **Description**: A `<patch_entry>` was found in the installation directory but is not expected based on the information in the `META-INF/<patch>.id` file. You might have removed a `META-INF/<patch>.id` file, but not the patch JAR files.
* **Action**: Remove the JAR file. Alternatively, if you think you mistakenly deleted the `.id` file, reinstall the patch.

## Verify which hosts have service packs or patches installed

In a multi-host environment, service packs and patches are installed on a host-by-host basis. In this scenario, you can use the API Gateway Manager web console to verify exactly which hosts have service packs or patches installed.

For example, you could upgrade host 1 to version 7.7 SP1, while host 2 remains at 7.7 for a period of testing. However, a system should run the same version across all hosts. You can use the API Gateway Manager topology and grid views to verify that all hosts in the system are running the same version and service pack. If a version mismatch is identified, you should ensure that the required service pack is installed on hosts that are running older versions.
