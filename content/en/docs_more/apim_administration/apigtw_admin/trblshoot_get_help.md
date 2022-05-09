{
"title": "Get help with API Gateway",
"linkTitle": "Get help with API Gateway",
"weight":"200",
"date": "2019-10-14",
"description": "Access help and documentation for API Gateway, find your installed version (including any installed service pack and patches), and what other information you need when contacting Axway Support."
}

## Access help and documentation

Context-sensitive help is available in Policy Studio. Click the **Help** button on any window to display the relevant help page for that window.

You can access all of the API Gateway documentation on the Axway Documentation portal at <https://docs.axway.com/>.

## Find your installed version

You can find your installed version of API Gateway in several ways.

### Policy Studio

To view the installed version number in Policy Studio, select **Help > About Policy Studio**.

### API Gateway Manager

You can view the installed version number, including any installed service packs or updates, for API Gateway instances in API Gateway Manager. For more information, see [Check the installed version number of API Gateway instances](/docs/apim_administration/apigtw_admin/managetopology#check-installed-version).

### Process listing

You can list the API Gateway processes to view the installed version number, including any installed service pack or update. For example:

```
$ ps -eaf | grep vshell
user1 13696 13687  0 Nov14 pts/24   00:11:50 APIGateway1 (Group1) (7.7 SP1) (vshell)
user1 19595 13643  0 Nov14 pts/23   00:06:05 NodeManager on Host1 (Node Manager Group) (7.7 SP1) (vshell)
```

### Trace file

You can find the installed version number, including any installed service pack or update, in the header of the trace files for the Node Manager, API Gateway instance, or API Gateway Analytics server. For more information on where to find the trace files, see [Configure API Gateway diagnostic trace](/docs/apim_administration/apigtw_admin/tracing).

For example, the following trace file (for a Node Manager) shows the installed version and service pack:

```
# ProductName=nodemanageronhost1.axway.com 7.7 SP1-2016-12-01 rev. 39e69a0 (Linux.x86_64)
# CurrentDate=Fri, 02 Dec 2016 09:26:14 +0000
# CurrentDateUTC=1480670774
# TZ=GMT
```

## Find your installed version and list patches using managedomain {#find-install-version}

You can use the `managedomain` command with the `-v` or `--version` options to find the installed version number, including any installed service pack, update, or patches, and build information.

The following example shows the output for a system with API Gateway 7.7 installed, with service pack `SP1` installed, and no patches installed:

```
$ ./managedomain --version
Version:    7.7 SP1
Build Date: 2016-11-04 08:34:37 UTC
Commit Id:  05a6440fc3128d8d2f3dfe105904d04fffae67ea
Patch:      None
```

The `Build Date` and `Commit Id` fields in the output relate to the main product version (or the service pack or update, if one is installed). These fields are updated when a service pack or update is installed, but they are not updated when a patch is installed.

When patches are installed, `managedomain --version` displays the names of the patches installed. Patches are named after the internal ticket number and related commit ID (for example, `RDAPI-4563_3y7shb87635bhsaf7864nckzj7r9mp8744n8asa`). It also validates the installed patches and displays messages about any problems with the patch installation.

The following example shows the output for a system with patches installed and no patch validation issues:

```
$ ./managedomain --version
Version:    7.7
Build Date: 2016-12-06 14:56:44 UTC
Commit Id:  ba35aba7bf84a165dda0df81470f4e9a87d73778
Patch:      OpenSSL_1_0_2j-fips
Patch:      RDAPI-5885_44dc0cde1d94f4dd4744327728aeeac5b2c4a802

$ ./managedomain --version
Version:    7.5.3
Build Date: 2016-12-06 14:56:44 UTC
Commit Id:  ba35aba7bf84a165dda0df81470f4e9a87d73778
Patch:      RDAPI-5693_etwe2346zdg567fhfg4ue7645846856766700a32
Patch:      RDAPI-5885_44dc0cde1d94f4dd4744327728aeeac5b2c4a802
```

The following example shows the output for a system with patches installed and various patch validation issues:

```
$ ./managedomain --version
Version:    7.5.3
Build Date: 2016-12-06 14:56:44 UTC
Commit Id:  ba35aba7bf84a165dda0df81470f4e9a87d73778
Patch:      RDAPI-4563_3y7shb87635bhsaf7864nckzj7r9mp8744n8asa
                Info: ./Linux.x86_64/jre: Cannot validate, no checksum available
Patch:      RDAPI-1783_mnc785jnsi84sni6a65l09vxgm93kmi78zd985q
                Warning: ./Linux.x86_64/lib/libssl.so.1.0.0:        Content changed
                Warning: ./Linux.x86_64/lib/libcrypto.so.1.0.0:     Content changed
                Warning: ./Linux.x86_64/lib/engines/libgost.so:     Content changed
                Warning: ./Linux.x86_64/lib/engines/libatalla.so:   Content changed
                Warning: ./Linux.x86_64/lib/engines/lib4758cca.so:  Content changed
                Warning: ./Linux.x86_64/lib/engines/libchil.so:     Content changed
                Warning: ./Linux.x86_64/lib/engines/libcswift.so:   Content changed
                Warning: ./Linux.x86_64/lib/engines/libaep.so:      Content changed
                Warning: ./Linux.x86_64/lib/engines/libubsec.so:    Content changed
                Warning: ./Linux.x86_64/lib/engines/libsureware.so: Content changed
Patch:      RDAPI-5885_44dc0cde1d94f4dd4744327728aeeac5b2c4a802
                Error: ext/lib/com.vordel.circuit.net.QaReflectProcessor.jar: File not found
                Error: ext/lib/com.vordel.circuit.net.ReflectProcessor.jar:   File not found
Patch:      RDAPI-5693_etwe2346zdg567fhfg4ue7645846856766700a32
                Error: META-INF/RDAPI-6666_ba35aba.id: Malformed file
Patch:      ext/lib/com.vordel.circuit.net.UnexpectedProcessor.jar: Unexpected file
```

For more information on the patch validation messages and how to resolve them, see [Update API Gateway](/docs/apim_installation/apigtw_install/install_service_packs/).

The `managedomain --version` command checks the version on the local machine only. You do not need to have an Admin Node Manager or API Gateway running to run this command.

This command lists version information relating to what is installed on disk. You must stop your Node Manager and API Gateways before installing service packs, updates, and patches, you must restart them after installation, or the output of this command might not reflect what is loaded for the runtime of the Node Manager and API Gateways.

This command can also be run in command interpreter mode.

## Information to include when you contact Axway Support

It is important to include as much information as possible when contacting the Axway Support team. This helps to diagnose and solve the problem in a more efficient manner. The following information should be included with any Support query:

* Name and version of the product (for example,  API Gateway 7.7).
* Details of any service pack, update, or patches that were applied to the product, if any.
* Operating system on which the product is running.
* A clear (step-by-step) description of the problem or query.
* If you have encountered an error, the error message should be included in the email. It is also useful to include any relevant trace files from the `/trace` directory of your product installation, preferably with the trace level set to `DEBUG`.
