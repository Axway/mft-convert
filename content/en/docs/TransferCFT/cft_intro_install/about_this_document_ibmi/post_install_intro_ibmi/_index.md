---
    title: "Post-installation"
    linkTitle: "Post-installation"
    weight: 160
---## Transfer CFT in production

This chapter describes how to operate Transfer CFT, and is comprised of the following sections:

- [Predefined commands](operating_cft/predefined_commands)
- [CFTUTIL commands](operating_cft/submit_cftutil_commands)
- [Source files interpretation](operating_cft/interpret_source_member)
- [Start/stop the UI server](start_copilot_server)
- [Run Transfer CFT](operating_cft/running_transfer_cft) jobs:
    -   [Query Transfer CFT messages]()
    -   [Start Transfer CFT](operating_cft/start_cft)
    -   [Submit a transfer command](submit_transfer_command)
- [Transfer CFT shutdown](operating_cft/shut_cft)

<span id="Transfer"></span>

## Transfer CFT menu usage

Other than during installation, which runs the Transfer CFT manager automatically, you can enter one of the following commands to start the manager:

- `CFT`
- `CALL CFTMENU`

## Create Transfer CFT system object

Now that you have installed your {{< TransferCFT/headerfootervariableshflongproductname  >}}, you will want to recreate Transfer CFT objects. Proceed to the Transfer CFT menu and press 4 to access the following options:

****1. Create job queue.****

Create a JOBQ object to launch Transfer CFT jobs using the default values.

****2. Create job description.****

Create a JOBD object to launch Transfer CFT jobs using the default values. This programs the initial library list and the {{< TransferCFT/headerfootervariableshflongproductname  >}} production libraries (CFTPGM and CFTPROD).

****3. Create subsystem.****

Create an SBS object to launch Transfer CFT jobs. This creates a subsytem with a storage size of 250MB.

****4. Add job-queue entry.****

Add a job queue entry to the subsystem that you created in step 3 to link it with the JOBQ that you created in step 1. The maximum active job is unlimited.

****5. Create class.****

Create a class CLS object for Transfer CFT with a time slice of 5000 milliseconds.

****6. Add routing entry.****

Add a routing entry to the subsytem created in step 3 to link it with the class created in step 5. This uses the SEQNBR(9999) CMPVAL(\*ANY) and PGM(QCMD) options.

****7. Add communication entry.****

Add a communication entry to link the subsystem created in step 3 with the JOBD created in step 2. This uses the DFTUSR(CFT) ??MODE(\*ANY) ??MAXACT(\*NOMAX) options.

****8. Change profile.****

Change the user profile of the current user to specify that the user now uses the JOBD in step 2, and subsequently the subsystem and JOBQ created in the other steps and linked with the JOBD.

## Verify your installation

See the installation if you encounter problems with starting {{< TransferCFT/axwayvariablesComponentLongName  >}}.

### Installed directories

The act of installing Transfer CFT creates a library that contains product binaries and templates. Do no store any personal files in this library, or modify existing files in this library, as both are erased during updates.

> **Note**
>
> By default, this library is called CFTPGM.

## Register with {{< TransferCFT/PrimaryCGorUM  >}}

If you intend to implement {{< TransferCFT/PrimaryCGorUM  >}}, please refer to the {{< TransferCFT/axwayvariablesComponentLongName  >}} *User's Guide &gt; [*Register with* {{< TransferCFT/PrimaryCGorUM  >}}](https://docs.axway.com/bundle/TransferCFT_36_UsersGuide_allOS_en_HTML5/page/Content/cft_installation/migrate/register_CG.htm)* page for registration details.
