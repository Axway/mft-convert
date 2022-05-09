{
"title": "Configure a directory scanner",
  "linkTitle": "Configure a directory scanner",
  "weight": 7,
  "date": "2019-10-17",
  "description": "Scan a specified directory on the file system for files containing messages."
}
The Directory Scanner enables you to scan a specified directory on the file system for files containing messages (for example, in XML or JSON format). When the messages have been read, they can be passed into the core message pipeline, where the full range of message processing filters can act on them. The Directory Scanner is typically used in cases where an external application is dropping files (perhaps by FTP) on to the file system so that they can be validated, modified, and potentially routed on over HTTP or JMS. Alternatively, they can be stored to another directory where the application can pick them up again.

This sort of protocol mediation is very useful in cases where legacy systems are involved. For example, instead of making drastic changes to the legacy system by adding an HTTP engine, the API Gateway can pull the files from the file system, and route them on over HTTP to another back-end system. The added benefit is that messages can be exposed to the full range of message processing filters in the API Gateway. This ensures that only properly validated messages are routed on to the target system.

To add a new Directory Scanner, in the Policy Studio tree, under the **Environment Configuration** > **Listeners** node, right-click the name of the API Gateway instance (for example, `API Gateway`), and select the **Directory Scanner**

**Add** menu option. This topic describes how to configure the fields on the **Directory Scanner Settings** dialog.

## Input settings

The fields in this section configure where to scan for files, what files to scan, and when to scan.

**Input Directory**: Enter or browse to the input directory that the API Gateway scans for files. This setting is required.

**Match File**: Select one of the following options to specify how inbound files are filtered:

* **By Name**:   You can specify a comma-separated list of specific names, wildcarded names, or leave this field blank to accept all files by name. For example, a value of `"*.xml,*.xsl"`   only accepts XML and XSL files in the input directory.
* **By Extension**:   You can specify a comma-separated list of file extensions (excluding the `.`   character), or leave this field blank to accept all files by extension. For example, `"xml,xsl"`   only accepts XML and XSL files in the input directory.
* **By Pattern (regex)**:   If you wish to scan only for files based on some pattern, you can specify this as a regular expression. For example, if you wish to scan only for files with a particular file extension, such as `.xml`   , you can enter a regular expression such as the following:

  ```
  ^[a-zA-Z\s]*.xml
  ```
* Similarly, if a particular naming scheme is used when dropping the files into the configured directory, you can enter a regular expression to scan for these files only. For example, the following regular expression scans only for files named using a `yyyy-mm-dd` date format:

  ```
  ^(\d{4})[- /.]((1[012])|(0?[1-9]))[- /.]((3[01])|([012]?[1-9])|([12]0))$
  ```

**Poll Rate (ms)**: Specifies the amount of time (in milliseconds) between each scan of the **Input Directory** for new files. Defaults to `60000`.

## Processing settings

The fields configured in this section determine what processing is performed on the input files, and where files are placed before and after processing.

**Processing Directory**: Enter or browse to the directory to which the input file is copied prior to processing. This field is optional. If it is not specified, the input file is moved to the processing directory specified by the processing policy.

**Response Directory**: Enter or browse to the directory to which the response file is copied. This field is optional. If this is not specified, the response file is not written to disk.

**Processing Policy**: Select the policy executed on the input file. For example, the policy could perform message validation, routing, virus checking, or XSLT transformation. This setting is required.

**File Type**: Specifies how the input file is interpreted. Select one of the following options:

* **Raw**:   Assumes a content-type of `application/octet-stream`. This is the default.
* **Treat as HTTP Message (including headers)**:   Assumes the inbound file contains an HTTP request (optionally with HTTP headers).
* **Infer content-type from extension**:   Performs a lookup on configured MIME types to determine the content-type of the file based on its extension.
* **Use Content-type**:   Enables you to specify a content-type in the text box.

**Sort files**: Select whether files are sorted, and select one of the following sort types from the list:

* `Date last modified`
* `Extension`
* `Extension (case-sensitive)`
* `Name`
* `Name (case-sensitive)`
* `Size`

The default is to sort by `Name`.

Select one of the following sort directions:

* `Ascending`
* `Descending`

The default is `Ascending`.

**Process files**: Select whether to process files in sequence or in parallel. Select **In sequence** to process files in a particular order (queued). To process files in parallel, select **In parallel, max workers** and enter the maximum number of threads processing scanned files simultaneously.

To limit the number of files queued for processing during each directory scan, select **Restrict max number of files processed on each run to** and enter the maximum value. The default is 1000.

This option is useful if you need to process very large files, or process files in a particular order (when used in conjunction with the **Sort Files** option).

## On completion settings

You can specify what to do when the file processing has completed. Select one of the following options:

* **Do Nothing**:   The input file remains in the **Input Directory**   or **Processing Directory**. This is the default.
* **Delete Input File**:   The input file is deleted from the **Input Directory**   or **Processing Directory**.
* **Move Input File**:   The input file is moved (archived) to the directory specified in the **To Directory**   field. You can also specify an optional **File Prefix**   or **File Suffix**   for the archived file.

## Traffic monitor settings

The **Traffic Monitor** tab enables you to configure traffic monitoring settings for the directory scanner. To override the system-level traffic monitoring settings, select **Override system-level settings**, and configure the relevant options.