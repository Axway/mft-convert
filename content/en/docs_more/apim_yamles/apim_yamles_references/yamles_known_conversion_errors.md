{
"title": "Known conversion errors",
"linkTitle": "Known conversion errors",
"weight":"40",
"date": "2020-09-25",
"description": "Learn how to fix errors that might occur when converting XML federated configuration to YAML configuration."
}

This section covers examples of issues that might occur when using the `fed2yaml` or `frag2yaml` options of the `yamles` CLI tool.

## Upgrade of the config fragment not supported

|yamles option |Severity | Exception raised |
|---           |---      | ---              |
|`frag2yaml`   |ERROR    | UnsupportedUpgradeError: The config fragment is missing metadata needed for upgrade. You can upgrade using the upgradepolicy script, or turn upgrade off using the --no-upgrade option.|

The upgrade of the config fragment is not supported from `yamles` when type `metaInfo` is missing from the configuration fragment. The conversion of the configuration fragment is most likely still possible as long as the type information is located within the XML file.

If upgrade is not required because the XML configuration fragment was created using the current version of the product, you can bypass upgrade using the `--no-upgrade` parameter for `frag2yaml`. If your XML configuration fragment was created using an older version of the product, ensure that your XML configuration fragment has been upgraded via the `upgradepolicy` script, or by importing and re-exporting from Policy Studio.

## Incorrect passphrase for XML configuration fragment

|yamles option  |Severity | Exception raised |
|---            |---      | ---              |
|`frag2yaml`    |ERROR    | IncorrectPassphraseError: The supplied passphrase is incorrect. Either supply the correct passphrase, or turn off passphrase checking via --no-passphrase-check. This is normally safe to turn off unless the upgrade migration steps need to decrypt or encrypt data.|

The XML configuration fragment you are converting has an `ESConfiguration` entity, so passphrase checking is enabled by default but you have not provided the correct passphrase.

To fix this issue, pass the correct passphrase using the `--passphrase` parameter.

The `frag2yaml` option will _upgrade_ the XML configuration fragment before it _converts_ it to YAML. The conversion process does not require the passphrase as it does not decrypt or encrypt data.

The upgrade process only uses the passphrase when a migrate step is performing encryption or decryption, which is an uncommon situation. So, in general, it is safe to turn off the passphrase check with the `--no-passphrase-check` parameter.

After running `frag2yaml`, you can try to change the passphrase using the `change-passphrase` option if you can guess the original passphrase. If all attempts to figure out the passphrase fail, you can manually edit all encrypted data in the YAML files using the `encrypt --text` and `encrypt --text` options. All sensitive data must be encrypted with the same passphrase in order for the configuration to be loaded successfully by an API Gateway.

You can turn off the entire _upgrade_ process via the `--no-upgrade` parameter, which will also turn off the passphrase check.

If your XML configuration fragment was created using the current version of the product, then you do not need to upgrade it. If your XML configuration fragment was created using an older version of the product, ensure that your XML configuration fragment has been upgraded via the `upgradepolicy` script, or by importing and re-exporting from Policy Studio. If you use `--no-upgrade`, there is no need to also specify `--no-passphrase-check`.

## Missing parent entity in XML configuration fragment

|yamles option  |Severity | Exception raised |
|---            |---      | ---              |
|`frag2yaml`    |ERROR    |EntityStoreException: com.vordel.es.EntityStoreException: Parent Entity does not exist: `<key type='NetService'><id field='name' value='Service'/><key type='HTTP'><id field='name' value='Default Services'/></key></key>`|

The XML configuration fragment you are converting contains an entity with a parent entity that is not contained in the XML configuration fragment. To fix this, regenerate your XML configuration fragment to include parent entities by selecting EXPORT_CLOSURE or EXPORT_TRUNKS in ES Explorer.

## No portable keys in XML configuration fragment

|yamles option  |Severity | Exception raised |
|---            |---      | ---              |
|`frag2yaml`    |ERROR    |EntityStoreException: com.vordel.es.EntityStoreException: The config fragment has no metaInfo but contains entities, check it is a valid config fragment created with EXPORT_PORTABLE_ESPKS|

The XML configuration fragment you are converting might not have been created as a portable configuration fragment with the EXPORT_PORTABLE_ESPKS option. If this is the case, the entities in the XML file will have `entityPK` and `parentPK` attributes. To fix this, regenerate your XML configuration fragment.

## No type information in XML configuration fragment

|yamles option  |Severity | Exception raised |
|---            |---      | ---              |
|`frag2yaml`    |ERROR    |EntityStoreException: com.vordel.es.EntityStoreException: Cannot convert config fragment to entity store as the config fragment is missing entityType information|

The XML configuration fragment you are converting has no type information. To fix this, regenerate your XML configuration fragment.
