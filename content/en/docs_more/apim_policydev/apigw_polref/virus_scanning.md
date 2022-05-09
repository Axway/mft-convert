{
"title": "Virus scanning filters",
  "linkTitle": "Virus scanning filters",
  "weight": 69,
  "date": "2019-10-17",
  "description": "Scan the content of a message for viruses."
}

## Scan with McAfee antivirus filter

The **McAfee antivirus** filter scans incoming HTTP requests and their attachments for viruses and exploits. For example, if a virus is detected in a MIME attachment or in the XML message body, the API Gateway can reject the entire message and return a SOAP Fault to the client. In addition, this filter supports cleaning of messages from infections such as viruses and exploits. It also provides scan type presets for different detection levels, and reports overall message status after scanning.

### Prerequisites for McAfee antivirus

McAfee virus scanner integration requires the McAfee Scan Engine and a valid McAfee license. The required third-party binaries are included in your API Gateway installation.

#### Add third-party binaries to API Gateway

To add third-party binaries to API Gateway, perform the following steps:

1. Add the binary files as follows:

   * Add `.jar`   files to the `INSTALL_DIR/apigateway/ext/lib`   directory.
   * Add `.so`   files to the `INSTALL_DIR/apigateway/<platform>/lib` directory.
2. Restart API Gateway.

#### Add third-party binaries to Policy Studio

To add third-party binaries to Policy Studio, perform the following steps:

1. Select **Window > Preferences > Runtime Dependencies** in the Policy Studio main menu.
2. Click **Add** to select a JAR file to add to the list of dependencies.
3. Click **Apply** when finished. A copy of the JAR file is added to the `plugins` directory in your Policy Studio installation.
4. Click **OK**.
5. Restart Policy Studio with the `-clean` option. For example:

   ```
   cd INSTALL_DIR/policystudio/
   policystudio -clean
   ```

### Configure McAfee antivirus

To configure the **McAfee antivirus** filter, perform the following steps:

1. Enter a name that indicates the role of this filter in the **Name** field.
2. Select a **Scan type** from the list. The available options are as follows:

   * **Custom**: Enables you to set the **Custom options**   described in [Custom options](#custom-options). This provides backwards compatibility with earlier API Gateway versions.
   * **Normal**: Processes the entire message detecting exploits and viruses in the message headers, macros, multi-file archives, executables, MIME-encoded/UU-encoded/XX-encoded/BinHex and TNEF/IMC format files. Performs heuristic analysis to find new viruses and potentially unwanted programs. This is the default scan type.
   * **Fast**: Detects infections in the top level of each message part, such as exploits that use headers and multiple bodies. The detection is less precise, but the performance is better if the top-level object is infected.
   * **Multi-pass**: Combines the **Normal** and **Fast** scan types. The **Fast**   scan (pass 1) runs first on the whole message with no cleaning. The scanner stops if it finds an infected object, and if the clean type is set to **No cleaning**, the scanner reports the infection, or otherwise deletes the message. If pass 1 does not detect any virus or exploit, the **Normal**   scan (pass 2) runs with the specified clean type and provides more precise detection.
3. Select a **Clean type** from the list. The available options are as follows:

   * **No cleaning**: Fails if any infection is detected. This is the default clean type.
   * **Always remove infected parts**: Removes the infected message part, and does not try to repair it.
   * **Attempt to repair infected parts**: Attempts to repair the found infection (if repairable), otherwise deletes the infected message part.

When existing policies are upgraded to the current API Gateway version, the **McAfee antivirus** filter scan type is set to **Custom** and the clean type is set to **No cleaning** for backwards compatibility.

#### Custom options

When you configure a custom scan type, the following **Custom options** are available:

**Decompress Archives**: This instructs the filter to scan each file in an archive for viruses. Types of archived files include the ZIP, JAR, TAR, ARJ, LHA, PKARC, PKZIP, RAR, WinACE, BZip, and Zcompress formats.

**Decompress Executables**: Executables are sometimes compressed to decrease overall message size. In such cases, any embedded viruses are also compressed and may be missed by conventional scans. If this option is selected, the filter decompresses the executable before scanning it for viruses.

**Fail Any Macros**: A *macro* is a series of commands that can be invoked in a single command or keystroke. While calling the macro can appear to be harmless, the initiated command sequence may be harmful. Macros are usually configured to run automatically when the host document is opened. When this option is selected, the API Gateway fails if any macro is detected in a compound document (whether it matches a virus signature or not). An appropriate SOAP Fault is returned to the client.

**Heuristic Program Analysis**: A heuristic virus detection algorithm runs a series of probing tests on a file in an attempt to solicit virus-like behavior from it. Based on the results of these tests, the algorithm can then make an educated guess on whether the file represents a potential threat or not. For example, programs that attempt to modify or delete files, invoke email clients, or replicate themselves all display virus-like behavior and so may be treated as viruses by the scanner.

The major advantage of this type of analysis is that new viruses can be detected. With the signature detection method, the scanner attempts to find a fixed number of known virus signatures in a file. Because the number of known signatures is fixed, new or unknown viruses can not be detected. If this option is selected, the filter runs heuristic analysis on executables only.

**Heuristic Macro Analysis**: When this option is enabled, the filter runs heuristic detection analysis on macros contained in any body parts of the message. If any viruses are detected, the message is blocked. If this option is selected, the API Gateway searches for virus signatures in the respective body parts of a MIME message. However, it can only search for *known* viruses using this method.

{{< alert title="Note" color="primary" >}}Macros embedded in MIME parts are also scanned for virus signatures.{{< /alert >}}

**Scan Embedded Scripts**: The API Gateway can scan MIME parts, such as HTML documents, for embedded scripts. If this option is selected, the filter scans for embedded scripts.

**Scan for Test Files**: When this option is selected, the API Gateway fails if it encounters an antivirus test file (for example, `eicar.com`). This is a convenient way to check that the antivirus filter successfully detects known viruses.

#### Custom options for normal or fast scans

The custom options are disabled when you select the normal or fast scan types. The custom options used for these options (where applicable) are as follows:

|     Options               | Normal scan type                      | Fast scan type                                       |
| -------------------------- | ------------------------------------- | ---------------------------------------------------- |
| Decompress Archives        | On                                    | Off                                                  |
| Decompress Executables     | On                                    | N/A (instead, ignore certain compressed executables) |
| Fail Any Macros            | N/A (instead, all macros are scanned) | N/A (instead, ignore macros in compound documents)   |
| Heuristic Program Analysis | On                                    | Off                                                  |
| Heuristic Macro Analysis   | On                                    | Off                                                  |
| Scan Embedded Scripts      | On                                    | Off                                                  |
| Scan for Test Files        | Off                                   | Off                                                  |

### Message status

When the scan is complete, the **McAfee antivirus** filter reports the overall message status in the `mcafee.status` message attribute, which is generated by the filter. This reflects the overall status of the scan for all message parts, and includes one of the following values:

| Value               | Description                                                      |
| ------------------- | ---------------------------------------------------------------- |
| `NOVIRUS`           | No virus or exploit detected in the message                      |
| `INFECTED`          | Infection detected in the message                                |
| `REPAIRED`          | Message repaired                                                 |
| `REMOVED`           | Some or all message parts successfully removed                   |
| `REPAIRED, REMOVED` | Some message parts successfully repaired and some others removed |

### Load McAfee updates

When the **McAfee antivirus** filter has been loaded, it searches for virus definitions in the following directory:

```
INSTALL_DIR/apigateway/conf/plugin/mcafee/datv2
```

When these have been loaded, it periodically checks for the presence of a directory named as follows:

```
INSTALL_DIR/apigateway/conf/plugin/mcafee/datv2.new
```

If the `datv2.new` directory is found, the scanner is stopped and the `datv2` directory is renamed to `datv2.0`. If a `datv2.0` directory already exists from a previous rollover, a `datv2.1` directory is created instead, and so on, until the limit of 50 is reached (`datv2.50`). When the limit of 50 is reached you must remove the old files. This means that the server never deletes the old files, and rolls them out of the way.

When the engine is stopped and restarted, any messages that require scanning are suspended until the restart completes. In addition, an initiated reload is suspended until all currently active scans are completed.

Like all file system scanning approaches, there is an inherent ordering problem. If you create the `datv2.new` directory before copying the files into the directory, the scanner may pick up the new directory before it is ready to be used. For example, you might experience problems if you enter the following commands from the `INSTALL_DIR/apigateway/conf/plugin/mcafee` directory:

```
mkdir datv2.new
cp /var/tmp/mcafee/newfiles/*.* datv2.new
```

You can use the following commands to prevent this problem:

```
mkdir datv2.tmp
cp /var/tmp/mcafee/newfiles/*.* datv2.tmp
mv datv2.tmp datv2.new
```

These create a temporary folder, copy the files into this folder, and rename the temporary folder to `datv2.new`. In this way, the scanner is guaranteed to pick up the virus definition files when it detects the new directory.

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
