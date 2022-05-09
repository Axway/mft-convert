{
"title": "Virus scanning filters",
  "linkTitle": "Virus scanning filters",
  "weight": 69,
  "date": "2019-10-17",
  "description": "Scan the content of a message for viruses."
}

## Scan with ClamAV antivirus filter

You can use the **ClamAV antivirus** filter to check messages for viruses by connecting to a ClamAV daemon running on network. The ClamAV daemon inspects the message and if the daemon finds a virus, it returns a corresponding response to the API Gateway, which can then block the message, if necessary.

Complete the following fields to configure the **ClamAV antivirus** filter:

* **Name**: Enter an appropriate name for this filter to display in a policy.
* **ClamAV Daemon Host**: Enter the host name of the machine on which the ClamAV daemon is running.
* **ClamAV Daemon Port Number**: Enter the port on which the ClamAV daemon is listening.

## Scan with Sophos antivirus filter

The **Sophos antivirus** filter uses the Sophos antivirus Interface (SAVI) to screen messages for viruses. You can configure the behavior of the Sophos library using configuration options available in the **Sophos antivirus** filter.

{{< alert title="Note" color="primary" >}}Because the API Gateway does not ship with any Sophos binaries, the API Gateway must be installed on the same machine as the Sophos AV distribution. On Linux, before starting the API Gateway, ensure that the Sophos AV `lib`
directory is on your `LD_LIBRARY_PATH`.{{< /alert >}}

### Prerequisites for Sophos antivirus

Integration with Sophos requires Sophos SAV Interface version 4.8. You must add the required third-party binaries to your API Gateway and Policy Studio installations.

See [Add third party binaries to API Gateway](#add-third-party-binaries-to-api-gateway) and [Add third party binaries to Policy Studio](#add-third-party-binaries-to-policy-studio).

### Configure Sophos antivirus

All SAVI configuration options take the form of a name-value pair. Each name is unique and its corresponding value controls specific behavior in the Sophos antivirus library (for example, decompress `.zip` files to examine their content). You can specify these SAVI configuration settings in the **Sophos configuration settings** section:

**Name**: The **Sophos antivirus** filter ships with two sets of default configuration settings: one for UNIX-based platforms, and the other for Windows platforms. Select the
Linux configuration settings for your target platform from the list.

You can create a new set of configuration options by clicking the **Add** button, and adding the name-value pairs to the table provided. For convenience, you can base a new configuration set on a previously existing one, including the default Linux set. In this way, you can create a new configuration set that *inherits* from the default set, and then adds more configuration options.

To add a new configuration name-value pair, click the **Add** button under the table, and configure the following fields in the dialog:

**Name**: Enter a name for the SAVI configuration option. This name must be available in the version of the SAVI library that is used by the API Gateway. See your SAVI documentation for a complete reference on available options.

**Value**: Enter an appropriate value for the SAVI configuration option entered above. See your SAVI documentation for more information on acceptable values for this configuration option.

**Type**: Select the appropriate type of this configuration option from the list. See your SAVI documentation for more information on the type of the value for this configuration option.
