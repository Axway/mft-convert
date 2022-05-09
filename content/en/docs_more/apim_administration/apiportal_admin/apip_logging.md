---
title: Supported log files
linkTitle: API Portal Supported log files
weight: 85
date: 2020-11-17
description: Learn about the different types of logs that API Portal creates,
  where they are located, what is their purposes, and how you can use the logs
  for debugging and troubleshooting errors.
---

API Portal includes several log files, which hold different data:

* `com_apiportal.access.log` - Contains records related to the access to API Portal, for example, successful logins and logouts from the system.
* `com_apiportal.api.log` - Contains all debug data. When the [debug mode is enabled through JAI](/docs/apim_installation/apiportal_install/secure_harden_portal/#configure-joomla-administrator-interface-jai), all requests and responses can be reviewed under this file.
* `com_apiportal.error.log` - Whenever an exception is caught, it is logged in this file.
* `com_apiportal.txt` - Contains logs generated during the installation or upgrade of API Portal.

In all those log files, personally identifiable information (PII) is protected using Global Unique Identifiers (GUIDs). Each user in API Portal is associated to a GUID, which is used instead of the user name to protect the user data in compliance with General Data Protection Regulation (GDPR). Therefore, all log records are personalized and can be used to check for malicious activity coming from a specific user without compromising their personal information.

## Change the location of API Portal log files

By default, API Portal saves the log files in the `htdoc/logs` directory. For increased security, you can configure a different location to save the log files:

1. In the JAI, click **System > Global Configuration**.
2. On the **System** tab, enter the new location in the **Path to Log Folder** field. Apache must have permission to write to the new location.
3. Click **Save**.

## Security best practices for storing log files

You must ensure that security logs are protected against tampering, repudiation, and unauthorized access or modification. Store logs in a secure and tamper-proof location so that the logs can be used as evidence, for example, in any form of legal proceedings.

To protect the integrity of the application generated logs:

* Store logs on write-once media.
* Forward a copy of the logs to a centralized security information and event management (SIEM) system.
* Generate message digests for each log file.

This approach ensures that you can detect and prevent tampering.

## Logging and error reporting files

For more information on the troubleshooting logs of API Portal see the [Troubleshooting API Portal](/docs/apim_administration/apiportal_admin/troubleshooting/#logging-and-error-reporting).