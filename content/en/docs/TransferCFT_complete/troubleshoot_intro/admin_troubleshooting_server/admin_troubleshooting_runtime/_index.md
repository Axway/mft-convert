{
    "title": "Check the runtime",
    "linkTitle": "Troubleshoot the Transfer CFT runtime",
    "weight": "280"
}Runtime issues can include the following for the server, Copilot, CFTUTIL and other system services:

- Abend
- Performance
- Start disk space, multi-node issues, restart
- Unexpected shut down, such as when the catalog is full
- System freezes or infinite looping
- Service pack issues (applying or removing), also for migrating issues, updates

Common causes
-------------


| Issue vs<br /> Possible causes  | Hard disk bottleneck  | Catalog<br/> full | Network<br/> bottleneck | Memory or processor bottleneck*  | Corrupt<br/> file or DB ** |
| --- | --- | --- | --- | --- | --- |
| Performance  | Disk check  |   | Network checks  | Check application  |   |
| Start issue  | Disk check  | Catalog check  | Network checks  | timeout  | Check {{< TransferCFT/axwayvariablesComponentLongName  >}} files  |
| Unexpected stop  | Disk check  | Catalog check  | Network checks  |   | Check {{< TransferCFT/axwayvariablesComponentLongName  >}} files  |
| Transfer freeze or infinite looping  | Disk check  |   | Network checks  | Check application  | Check {{< TransferCFT/axwayvariablesComponentLongName  >}} files  |
| SP and updates  | Disk check  |   |   |   |   |
| Crash  | Disk check  |   | Network checks  |   | Check {{< TransferCFT/axwayvariablesComponentLongName  >}} files  |


\* Axway or third party software.

\*\* {{< TransferCFT/axwayvariablesComponentLongName  >}} internal database.

Initial checks and actions
--------------------------

### Disk check

- No space left on the device
    -   Free space
    -   Check Sentinel connectivity an{{< TransferCFT/axwayvariablesComponentShortName  >}}d verify the size of the runtime/data/trkapi.buf file, which may be voluminous
- Check for problematic file transfers and output, and clean
- Check to see if traces are set, which may lead to multiple large files in the "run" directory
- Check to see if you have enabled dynamic catalog resizing

### Catalog check

When the catalog is full stops and you cannot restart it without a correction action to reduce the size of the catalog. Note that this does not necessarily mean that the disk is full.

- See the [Catalog housekeeping](../../../admin_intro/admin_monitoring_intro/housekeeping_catalog) section
- Backup your catalog and export the existing catalog

### Export the full catalog

Create a new catalog file configured for a greater number of records, and re-import the original catalog file.

Use the command: cftmi

1. Export the full catalog as shown in the following example.
1. Check that the exported file is not empty.
1. Create new catalog:
1. Import the exported catalog file into the new catalog.
1. Move (rename) depending on your operating system:

### Network checks

These corrective measures are often system dependent.

- Perform basic tests such as pinging the address, and telnet (to check the port)

### Check additional products

- Check if another product is consuming all of the CPU/memory
- Check {{< TransferCFT/PrimaryCGorUM  >}} interoperability, such as the {{< TransferCFT/PrimarySentinel  >}} database
- Scripts or end-of-transfer procedures may indirectly
